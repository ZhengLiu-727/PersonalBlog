---
title: "[Spirng] 详解AOP — AOP概览"
date: 2021-12-31T10:15:41+08:00
lastmod: 2021-12-31T10:15:41+08:00
draft: false
hiddenFromHomePage: false
featuredImage: https://typora-us.oss-us-west-1.aliyuncs.com/OG-Spring.png

tags: [ "Spring","AOP" ]
categories: 
  - "Spring"
---

> ## 一、对AOP的初印象

首先先给出一段比较专业的术语（来自[维基百科](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%88%87%E9%9D%A2%E7%9A%84%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)）：



**面向切面的程序设计**（Aspect-oriented programming，AOP，又译作**面向方面的程序设计**、**剖面导向程序设计**）是计算机科学中的一种程序设计思想，旨在将横切关注点与业务主体进行进一步分离，以提高程序代码的模块化程度。通过在现有代码基础上增加额外的通知（Advice）机制，能够对被声明为“切点（Pointcut）”的代码块进行统一管理与装饰，如“对所有方法名以‘set*’开头的方法添加后台日志”。该思想使得开发人员能够将与代码核心业务逻辑关系不那么密切的功能（如日志功能）添加至程序中，同时又不降低业务代码的可读性。面向切面的程序设计思想也是面向切面软件开发的基础。

---

要理解切面编程，就需要先理解什么是切面。用刀把一个西瓜分成两瓣，切开的切口就是切面；炒菜，锅与炉子共同来完成炒菜，锅与炉子就是切面。web层级设计中，web层->网关层->服务层->数据层，每一层之间也是一个切面。**编程中，对象与对象之间，方法与方法之间，模块与模块之间都是一个个切面。**

我们一般做活动的时候，一般对每一个接口都会做活动的有效性校验（是否开始、是否结束等等）、以及这个接口是不是需要用户登录。

按照正常的逻辑，我们可以这么做:


<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0e34a7da65a54a0f9dce9594fac09acf~tplv-k3u1fbpfcp-zoom-1.image">


这有个问题就是，有多少接口，就要多少次代码copy。对于一个“懒人”，这是不可容忍的。好，提出一个公共方法，每个接口都来调用这个接口。这里有点切面的味道了。

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9c281297f6f84cb383d5ffb20b7c5b4c~tplv-k3u1fbpfcp-zoom-1.image">

同样有个问题，我虽然不用每次都copy代码了，但是，每个接口总得要调用这个方法吧。于是就有了切面的概念，我将方法注入到接口调用的某个地方（切点）。

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1aec88b74ae84ee78d8787d56c5ec7cf~tplv-k3u1fbpfcp-zoom-1.image">

这样接口只需要关心具体的业务，而不需要关注其他非该接口关注的逻辑或处理。

**红框处，就是面向切面编程。**

> ## 二、AOP中的相关概念

看过了上面的例子，我想大家脑中对AOP已经有了一个大致的雏形，但是又对上面提到的切面之类的术语有一些模糊的地方，接下来就来讲解一下AOP中的相关概念，了解了AOP中的概念，才能真正的掌握AOP的精髓。\
**这里还是先给出一个比较专业的概念定义**：

-   `Aspect`（切面）： Aspect 声明类似于 Java 中的类声明，在 Aspect 中会包含着一些 Pointcut 以及相应的 Advice。
-   `Joint point`（连接点）：表示在程序中明确定义的点，典型的包括方法调用，对类成员的访问以及异常处理程序块的执行等等，它自身还可以嵌套其它 joint point。
-   `Pointcut`（切点）：表示一组 joint point，这些 joint point 或是通过逻辑关系组合起来，或是通过通配、正则表达式等方式集中起来，它定义了相应的 Advice 将要发生的地方。
-   `Advice`（增强）：Advice 定义了在 `Pointcut` 里面定义的程序点具体要做的操作，它通过 before、after 和 around 来区别是在每个 joint point 之前、之后还是代替执行的代码。
-   `Target`（目标对象）：织入 `Advice` 的目标对象.。
-   `Weaving`（织入）：将 `Aspect` 和其他对象连接起来, 并创建 `Adviced` object 的过程

{{< admonition example >}}

**下面我以一个简单的例子来比喻一下 AOP 中 `Aspect`, `Joint point`, `Pointcut` 与 `Advice`之间的关系。**

让我们来假设一下, 从前有一个叫爪哇的小县城, 在一个月黑风高的晚上, 这个县城中发生了命案。作案的凶手十分狡猾, 现场没有留下什么有价值的线索。不过万幸的是, 刚从隔壁回来的老王恰好在这时候无意中发现了凶手行凶的过程, 但是由于天色已晚, 加上凶手蒙着面, 老王并没有看清凶手的面目, 只知道凶手是个男性, 身高约七尺五寸。爪哇县的县令根据老王的描述, 对守门的士兵下命令说: 凡是发现有身高七尺五寸的男性, 都要抓过来审问。士兵当然不敢违背县令的命令, 只好把进出城的所有符合条件的人都抓了起来。

{{< /admonition >}}

来让我们看一下上面的一个小故事和 AOP 到底有什么对应关系。

首先我们知道, 在 Spring AOP 中 `Joint point` 指代的是所有方法的执行点, 而 point cut 是一个描述信息, 它修饰的是 `Joint point`, 通过 point cut, 我们就可以确定哪些 `Joint point` 可以被织入 `Advice`. 对应到我们在上面举的例子, 我们可以做一个简单的类比, **`Joint point` 就相当于爪哇的小县城里的百姓**，**`pointcut` 就相当于老王所做的指控， 即凶手是个男性，身高约七尺五寸**，**而 `Advice` 则是施加在符合老王所描述的嫌疑人的动作: 抓过来审问**。

为什么可以这样类比呢?

-   `Joint point` ： 爪哇的小县城里的百姓: 因为根据定义，`Joint point` 是所有可能被织入 `Advice` 的候选的点，在 Spring AOP中，则可以认为所有方法执行点都是 `Joint point`。 而在我们上面的例子中，命案发生在小县城中，按理说在此县城中的所有人都有可能是嫌疑人。
-   `Pointcut` ：男性，身高约七尺五寸: 我们知道，所有的方法（`Joint point`）都可以织入 `Advice`, 但是我们并不希望在所有方法上都织入 `Advice`，而 `Pointcut` 的作用就是提供一组规则来匹配`Joint point`，给满足规则的 `Joint point` 添加 `Advice`。同理，对于县令来说，他再昏庸，也知道不能把县城中的所有百姓都抓起来审问，而是根据凶手是个男性，身高约七尺五寸，把符合条件的人抓起来。在这里凶手是个男性，身高约七尺五寸就是一个修饰谓语，它限定了凶手的范围, 满足此修饰规则的百姓都是嫌疑人，都需要抓起来审问。
-   `Advice` ：抓过来审问，`Advice` 是一个动作，即一段 Java 代码，这段 Java 代码是作用于 `Pointcut` 所限定的那些 `Joint point` 上的。同理，对比到我们的例子中，抓过来审问 这个动作就是对作用于那些满足男性，身高约七尺五寸的爪哇的小县城里的百姓。
-   `Aspect` ：`Aspect` 是 `Pointcut` 与 `Advice` 的组合，因此在这里我们就可以类比: "根据老王的线索，凡是发现有身高七尺五寸的男性，都要抓过来审问" 这一整个动作可以被认为是一个 `Aspect`。

**最后是一个描述这些概念之间关系的图**：


<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e34143cf38dc41b0afcccbeda1378dac~tplv-k3u1fbpfcp-zoom-1.image">

> ## 三、其他的一些内容

`AOP`中的`Joinpoint`可以有多种类型：构造方法调用，字段的设置和获取，方法的调用，方法的执行，异常的处理执行，类的初始化。也就是说在`AOP`的概念中我们可以在上面的这些`Joinpoint`上织入我们自定义的`Advice`，但是在`Spring`中却没有实现上面所有的`joinpoint`，确切的说，`Spring`只支持方法执行类型的`Joinpoint`。

**Advice 的类型**

-   `before advice`, 在 join point 前被执行的 advice. 虽然 before advice 是在 join point 前被执行, 但是它并不能够阻止 join point 的执行, 除非发生了异常(即我们在 before advice 代码中, 不能人为地决定是否继续执行 join point 中的代码)
-   `after return advice`, 在一个 join point 正常返回后执行的 advice
-   `after throwing advice`, 当一个 join point 抛出异常后执行的 advice
-   `after(final) advice`, 无论一个 join point 是正常退出还是发生了异常, 都会被执行的 advice
-   `around advice`, 在 join point 前和 joint point 退出后都执行的 advice. 这个是最常用的 advice
-   `introduction`，introduction可以为原有的对象增加新的属性和方法

在`Spring`中，通过动态代理和动态字节码技术实现了`AOP`，这些内容，将在后续文章中进行讲解。



{{< admonition >}}
**👻 本人水平有限，如有建议，欢迎留言或邮件联系 👻** 
{{< /admonition >}}
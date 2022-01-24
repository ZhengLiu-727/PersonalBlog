---
title: "Comparable 和 Comparator"
date: 2022-01-23T23:34:56+08:00
lastmod: 2022-01-23T23:34:56+08:00
draft: false
hiddenFromHomePage: false

tags: [ "Java" ]
categories: 
  - "java"
---

在学习的过程中，总能在各种地方见到`Comparable`，`Comparator`，`compareTo`，`compare`，有时会有点乱，今天稍微总结一下

> ## Comparable

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
```

从源码可以看出，Comparable是一个接口，而且内部只有一个方法`compareTo`

Comparable可以认为是一个**内比较器**，实现了Comparable接口的类有一个特点，就是这些类是可以和自己比较的，至于具体和另一个实现了Comparable接口的类如何比较，则依赖compareTo方法的实现，compareTo方法也被称为自然比较方法。如果开发者add一个对象进入一个Collection，想要Collections的sort方法帮你自动进行排序的话，那么这个对象必须实现Comparable接口。

compareTo方法的返回值是int，有三种情况：

1. 比较者大于被比较者（也就是compareTo方法里面的对象），那么返回正整数

2. 比较者等于被比较者，那么返回0

3. 比较者小于被比较者，那么返回负整数

**例子：**

```java
import java.util.*;

/**
 * 省略了get set方法
 */
class Student implements Comparable<Student>{

    private int id;
    private int age;

    public Student(int id, int age) {
        this.id = id;
        this.age = age;
    }

    @Override
    public int compareTo(Student o) {
        return this.age - o.age;
    }

    public static void main(String[] args) {
        Student s1 = new Student(1, 25);
        Student s2 = new Student(2, 24);
        Student s3 = new Student(3, 23);

        List<Student> list = new ArrayList<>();
        list.add(s1);
        list.add(s2);
        list.add(s3);
        Collections.sort(list);
        for (int i = 0; i < list.size(); i++) {
            System.out.println("id:" + list.get(i).id + " age:"+ list.get(i).age);
        }

        System.out.println("=============");

        Student[] array = new Student[3];
        array[0] = s2;
        array[1] = s3;
        array[2] = s1;
        Arrays.sort(array);
        for (int i = 0; i < array.length; i++) {
            System.out.println("id:" + array[i].id + " age:"+ array[i].age);
        }
    }
}
```

**输出：**

```java
id:3 age:23
id:2 age:24
id:1 age:25
=============
id:3 age:23
id:2 age:24
id:1 age:25
```

> ## Comparator

```java
public interface Comparator<T> {
    int compare(T o1, T o2);

    boolean equals(Object obj);
}
```

Comparator 接口在1.8版本后加了很多方法，这里只显示最重要的两个，我们主要实现的就是这个`compare`方法

若一个类要实现Comparator接口：它一定要实现 `compareTo` 方法，但可以不实现 `equals` 方法。
为什么可以不实现 `equals` 方法呢？ 因为任何类，默认都是已经实现了 `equals` 的。 Java中的一切类都是继承于java.lang.Object，在Object.java中实现了 equals(Object obj) 方法，所以其它所有的类也相当于都实现了该方法。

Comparator可以认为是是一个外比较器，个人认为有两种情况可以使用实现Comparator接口的方式：

1. 一个对象不支持自己和自己比较（没有实现Comparable接口），但是又想对两个对象进行比较

2. 一个对象实现了Comparable接口，但是开发者认为compareTo方法中的比较方式并不是自己想要的那种比较方式

Comparator接口里面有一个compare方法，方法有两个参数T o1和T o2，是泛型的表示方式，分别表示待比较的两个对象，方法返回值和Comparable接口一样是int，有三种情况：

1. o1大于o2，返回正整数

2. o1等于o2，返回0

3. o1小于o3，返回负整数

**例子：**

```java
import java.util.*;

/**
 * @author liuzheng
 * 省略了get set方法
 * 这里就是上面所得第二种情况，尽管Student类实现了comparable，但我不满意，想用id排序
 */
class Student implements Comparable<Student>{

    private int id;
    private int age;

    public Student(int id, int age) {
        this.id = id;
        this.age = age;
    }

    @Override
    public int compareTo(Student o) {
        return this.age - o.age;
    }


    public static void main(String[] args) {
        Student s1 = new Student(1, 25);
        Student s2 = new Student(2, 24);
        Student s3 = new Student(3, 23);

        List<Student> list = new ArrayList<>();
        list.add(s3);
        list.add(s2);
        list.add(s1);
        Collections.sort(list, new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                return o1.id - o2.id;
            }
        });
        for (int i = 0; i < list.size(); i++) {
            System.out.println("id:" + list.get(i).id + " age:" + list.get(i).age);
        }
    }
}

```

**输出：**

```java
id:1 age:25
id:2 age:24
id:3 age:23
```

> ## 总结

1. 如果实现类没有实现 Comparable 接口，又想对两个类进行比较（或者实现类实现了 Comparable 接口，但是对 compareTo 方法内的比较算法不满意），那么可以实现Comparator接口，自定义一个比较器，写比较算法

2. 实现 Comparable 接口的方式比实现 Comparator 接口的耦合性要强一些，如果要修改比较算法，要修改 Comparable 接口的实现类，而实现 Comparator 的类是在外部进行比较的，不需要对实现类有任何修改。

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        processEscapes: true
      }
    });
</script>

<script src='https://cdn.jsdelivr.net/npm/mathjax@2.7.4/MathJax.js?config=TeX-AMS-MML_HTMLorMML' async></script>
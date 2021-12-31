---
title: "LeetCode 0042. Trapping Rain Water"
date: 2021-12-30T11:24:58+08:00
lastmod: 2021-12-30T11:24:58+08:00
draft: false
hiddenFromHomePage: true

tags: [ "LeetCode", "算法"]
categories: 
  - "LeetCode题解"

---
> ## 题目

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**

<img src="https://typora-us.oss-us-west-1.aliyuncs.com/rainwatertrap.png">

```java
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

</br>

**Example 2:**

```java
Input: height = [4,2,0,3,2,5]
Output: 9
```
</br>

 **Constraints:**

- `n == height.length`
- `1 <= n <= 2 * 10^4`
- `0 <= height[i] <= 10^5`


> ## 思路

其实关键就在于 `maxLeft` 和 `maxRight`

使用双指针的方法，每次更新一侧（哪侧更低更新哪侧）

> ## 方法

```java
class Solution {
    public int trap(int[] height) {
        if (height == null || height.length == 0) return 0;
        int left = 0, right = height.length - 1;
        int maxLeft = 0, maxRight = 0;
        int res = 0;
        while (left <= right) {
            if (maxLeft < maxRight) {
                if (maxLeft > height[left]) res += maxLeft - height[left];
                maxLeft = Math.max(maxLeft, height[left]);
                left++;
            } else {
                if (maxRight > height[right]) res += maxRight - height[right];
                maxRight = Math.max(maxRight, height[right]);
                right--;
            }
        }
        return res;
    }
}
```


**时间复杂度：**$ O(N) $

**空间复杂度：**$ O(1) $


</br>

{{< admonition >}}
**👻 本人水平有限，如有建议，欢迎留言或邮件联系 👻** 
{{< /admonition >}}

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        processEscapes: true
      }
    });
</script>

<script src='https://cdn.jsdelivr.net/npm/mathjax@2.7.4/MathJax.js?config=TeX-AMS-MML_HTMLorMML' async></script>
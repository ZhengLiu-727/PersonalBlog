---
title: "由 2 Sum 到 K Sum"
date: 2022-01-01T22:41:29+08:00
lastmod: 2022-01-01T22:41:29+08:00
draft: false
hiddenFromHomePage: false

tags: [ "算法" ]
categories: 
  - "算法模版"
---

> ## 题目

大家 2Sum，3Sum，4Sum 估计都已经倒背如流了，但我感觉面试应该不会考这么烂大街的题目了

如果考的话，K Sum也许还有点意思（虽然感觉可能性也很低🤣🤣🤣)

> ## 思路

K Sum 的精华就是把 K Sum 转化为 K-1 Sum，直到 2 Sum

在 K 大于 2 时，每次确定一个数字，然后拿到 K-1 Sum 的答案后，往开头加上刚刚确定的那个数

> ## 解法



```java
public class Solution {
    public static List<List<Integer>> kSum (int[] nums, int start, int k, int target) {
        int len = nums.length;
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (k == 2) { // two pointers
            int left = start, right = len - 1;
            while (left < right) {
                int sum = nums[left] + nums[right];
                if (sum < target) { // move left
                    left++;
                } else if (sum > target){ // move right
                    right--;
                } else {
                    List<Integer> path = new ArrayList<Integer>();
                    path.add(nums[left]);
                    path.add(nums[right]);
                    res.add(path);
                    while(left < right && nums[left] == nums[left + 1]) left++;
                    while(left < right && nums[right] == nums[right - 1]) right--;
                    left++;
                    right--;
                }
            }
        } else {
            for (int i = start; i < len - (k - 1); i++) {
                if (i > start && nums[i] == nums[i - 1]) continue;
                List<List<Integer>> temp = kSum(nums, i + 1, k - 1, target - nums[i]);
                for (List<Integer> t : temp) {
                    t.add(0, nums[i]);
                }
                res.addAll(temp);
            }
        }
        return res;
    }
  
    public static void main(String[] args) {
        int[] nums = new int[] {1,0,-1,0,-2,2};
        System.out.println(kSum(nums, 0, 4, 0));
    }
}

=======
输出：
[[-2, -1, 1, 2], [-2, 0, 0, 2], [-1, 0, 0, 1]]
```
</br>

**时间复杂度：**$ O(n ^ {k - 1}) $

**空间复杂度：**$ O(k) $，递归 k - 2 次

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
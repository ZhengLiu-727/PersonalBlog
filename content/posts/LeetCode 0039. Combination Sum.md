---
title: "LeetCode 0039. Combination Sum"
date: 2021-12-25T10:46:44+08:00
draft: false
hiddenFromHomePage: true

tags: [ "LeetCode", "算法" ]
categories: 
  - "LeetCode题解"
---
> ## 题目
Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is **guaranteed** that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

</br>

**Example 1:**

```java
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```
</br>

**Example 2:**

```java
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```
</br>

**Example 3:**

```java
Input: candidates = [2], target = 1
Output: []
```

</br>

**Constraints:**

- `1 <= candidates.length <= 30`
- `1 <= candidates[i] <= 200`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 500`



> ## 思路

典型的回溯法模版题，

用一个 `list` 记录当前的组合，用一个 `int sum` 记录组合中数字之和，`index`标记已经遍历到哪里，下一步可以选择从index到最后一个数中的任何一个



> ## 解法

```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        res = new ArrayList<>();
        backtracking(candidates, 0, new LinkedList<>(), 0, target);
        return res;
    }

    public void backtracking(int[] candidates, int index, LinkedList<Integer> cur, int sum, int target) {
        if (sum > target) {
            return;
        }
        if (sum == target) {
            // 这里注意，不能直接add，直接add永远都是这一个List，要复制一个新的List加进去
            res.add(new ArrayList(cur));
            return;
        }
        for (int i = index; i < candidates.length; i++) {
            cur.add(candidates[i]);
            backtracking(candidates, i, cur, sum + candidates[i], target);
            cur.removeLast();
        }
    }
}
```

**时间复杂度：**$ O(S) $，其中 $S$ 为所有可行解的长度之和。我们可以分析出时间复杂度取决于搜索树所有叶子节点的深度之和，即所有可行解的长度之和。在这题中，我们很难给出一个比较紧的上界，我们知道 $O(n \times 2^n) $是一个比较松的上界，即在这份代码中，$n$ 个位置每次考虑选或者不选，如果符合条件，就加入答案的时间代价。但是实际运行的时候，因为不可能所有的解都满足条件，递归的时候我们还会用 `sum > target` 进行剪枝，所以实际运行情况是远远小于这个上界的

**空间复杂度：**$O(target)$。除答案数组外，空间复杂度取决于递归的栈深度，在最差情况下需要递归 $O(\textit{target}) $层

</br>

{{< admonition >}}
**👻 本人水平有限，如有建议，欢迎留言或邮件联系 👻** 
{{< /admonition >}}
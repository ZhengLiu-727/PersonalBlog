---
title: "LeetCode 1609. Even Odd Tree"
date: 2021-12-25T10:46:44+08:00
draft: false

tags: [ "LeetCode", "算法"]
categories: 
  - "LeetCode题解"

---
> ## 题目

A binary tree is named **Even-Odd** if it meets the following conditions:

- The root of the binary tree is at level index `0`, its children are at level index `1`, their children are at level index `2`, etc.
- For every **even-indexed** level, all nodes at the level have **odd** integer values in **strictly increasing** order (from left to right).
- For every **odd-indexed** level, all nodes at the level have **even** integer values in **strictly decreasing** order (from left to right).

Given the `root` of a binary tree, *return* `true` *if the binary tree is **Even-Odd**, otherwise return* `false`*.*

<br/>

**Example 1:**


<img src="https://assets.leetcode.com/uploads/2020/09/15/sample_1_1966.png" width="65%" />

```java
Input: root = [1,10,4,3,null,7,9,12,8,6,null,null,2]
Output: true
Explanation: The node values on each level are:
Level 0: [1]
Level 1: [10,4]
Level 2: [3,7,9]
Level 3: [12,8,6,2]
Since levels 0 and 2 are all odd and increasing and levels 1 and 3 are all even and decreasing, the tree is Even-Odd.
```
<br/>

**Example 2:**

<img src="https://assets.leetcode.com/uploads/2020/09/15/sample_2_1966.png" width="65%"/>

```java
Input: root = [5,4,2,3,3,7]
Output: false
Explanation: The node values on each level are:
Level 0: [5]
Level 1: [4,2]
Level 2: [3,3,7]
Node values in level 2 must be in strictly increasing order, so the tree is not Even-Odd.
```
<br/>

**Example 3:**

<img src="https://assets.leetcode.com/uploads/2020/09/22/sample_1_333_1966.png" width="65%"/>


```java
Input: root = [5,9,1,3,5,7]
Output: false
Explanation: Node values in the level 1 should be even integers.
```
</br>

**Constraints:**

- The number of nodes in the tree is in the range `[1, 10^5]`.
- `1 <= Node.val <= 10^6`


</br>

> ## 思路

层序遍历的变种，多了几个判断条件：

- 根据层数的奇偶，判断每个节点值的奇偶
- 根据层数的奇偶，判断单调性，每次遍历一层开始时，设一个`Integer pre = null`

</br>

> ## 解法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isEvenOddTree(TreeNode root) {
        boolean isEven = true; // 层数的奇偶
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {
            int size = q.size();
            Integer pre = null;
            for (int i = 0; i < size; i++) {
                TreeNode cur = q.poll();
                if (isEven) { // 偶数层
                    if (cur.val % 2 == 0) return false;
                    if (pre != null && pre >= cur.val) return false;
                } else { // 奇数层
                    if (cur.val % 2 == 1) return false;
                    if (pre != null && pre <= cur.val) return false;
                }
                pre = cur.val;

                if (cur.left != null) q.offer(cur.left);
                if (cur.right != null) q.offer(cur.right);
            }
            isEven = !isEven;
        }
        return true;
    }
}
```

**时间复杂度：**$ O(N) $，$N$为树的节点和

**空间复杂度：**$ O(N) $

</br>

{{< admonition >}}
**👻 本人水平有限，如有建议，欢迎留言或邮件联系 👻** 
{{< /admonition >}}
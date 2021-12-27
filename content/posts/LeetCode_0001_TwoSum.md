---
title: "LeetCode 0001. TwoSum"
date: 2021-12-25T11:24:58+08:00
draft: false

tags: [ "LeetCode", "算法"]
categories: 
  - "LeetCode题解"
---
{{< admonition >}}
**👻 本人水平有限，如有建议，欢迎留言或邮件联系 👻** 
{{< /admonition >}}

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly\* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

**Example 1:**

```java
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**

```java
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```java
Input: nums = [3,3], target = 6
Output: [0,1]
```

**Constraints:**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **Only one valid answer exists.**

**Follow-up:** Can you come up with an algorithm that is less than `O(n2) `time complexity?

> **思路**

经典 two sum

> **解法**

```java
class Solution {
  public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    int[] res = new int[2];
    for (int i = 0; i < nums.length; i ++) {
      int complement = target - nums[i];
      if (map.containsKey(complement)) {
        res[0] = i;
        res[1] = map.get(complement);
        break;
      }
      map.put(nums[i], i);
    }
    return res;
  }
}
```


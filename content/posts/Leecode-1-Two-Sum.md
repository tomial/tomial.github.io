---
title: Leecode.1.Two Sum (两数之和)
date: 2019-08-06 18:03:48

tags:
- LeetCode

categories :
- 算法
---

## 题目描述：

> LeetCode第一题，两数之和
>
> Given an array of integers, return `indices` of the two numbers such that they add up to a specific target.
>
> You may assume that each input would have `exactly` one solution, and you may not use the same element twice.
>

**Example:**

> Given nums = [2, 7, 11, 15], target = 9,
>
> Because nums[0] + nums[1] = 2 + 7 = 9,
> return [0, 1].

## 解释：

很简单的题目，给出一个数和一个数组，求数组里面两个和为该数的下标。



## 代码：

``` java
//暴力法
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for(int i = 0; i < nums.length; i++) {
            for(int j = i + 1; j < nums.length; j++) {
                if(nums[i] + nums[j] == target) {
                    return new int[] { i, j };
                }
            }
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```



总结：这题虽然很简单，但是提交了很多次才过，因为之前用的是排序的方法，忽略了一个问题：**排序后无法获得原数组的下标**，导致虽然获得了正确的数，但是下标却和原数组不一致，故做题时**使用排序后若出现下标出错**的问题考虑**是否是排序**造成的。
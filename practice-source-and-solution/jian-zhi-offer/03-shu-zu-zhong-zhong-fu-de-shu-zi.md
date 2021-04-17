---
title: 剑指offer 03： 数组中重复的数字
date: '7/22/2020 18:28:54'
tags:
  - 剑指offer
categories:
  - 剑指offer
  - simple
---

# 03： 数组中重复的数字

## [题目描述](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

难度: **简单**

> 在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。
>
> **示例 1：**
>
> 输入：\[2, 3, 1, 0, 2, 5, 3\] 
>
> 输出：2 或 3
>
> **限制：**2 &lt;= n &lt;= 100000

此题和 [**LeetCode 287： Find the Duplicate Number**](../leetcode/287-find-the-duplicate-number.md) ****相似度很高，区别是这里重复的数字不止一个，而且重复的次数也可能不止一次，从此题可以引用过来的解法有三个：

* Solution - 1： Brute-force O\(n^2\)
* Solution - 2： Sorting O\(nlog\(n\)\)
* Solution - 5： Set O\(n\)

其余的解法依赖于只有一个数字重复一次的条件，这里再来介绍些其他的解法。

## **解法 - 4：** 使用临时数组 O\(n\)

**思路：**因为nums中的每个数的大小都在`0~n-1`之间，我们可以建立一个临时数组temp并把nums中数的值和临时数组temp建立映射关系：就是nums中元素的值是几，我们就把temp中对应的位置值加1，当temp某个位置的值大于1的时候，就表示出现了重复，直接返回即可。

```java
public int findRepeatNumber(int[] nums) {
    int[] temp = new int[nums.length];
    for (int i = 0; i < nums.length; i++) {
        temp[nums[i]]++;
        if (temp[nums[i]] > 1)
            return nums[i];
    }
    return -1;
}
```

* **时间复杂度 : O\(n\)**
* **空间复杂度 : O\(n\)** 因为temp

## 解法 - 5： 原地置换 O\(n\)

**思路：**和上面思路相似，我们还可以不使用临时数组，我们在遍历的时候把数组nums中的值放到对应的位置上，比如某个元素是5，我们就把他放到nums\[5\]中，每次放入的时候查看一下这个位置是否放入了正确的值，如果已经放入了正确的值，就说明重复了，直接返回即可。

```java
public int findRepeatNumber(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        if (i == nums[i]) //位置正确，先不用管
            continue;
        if (nums[i] == nums[nums[i]]) //出现了重复，直接返回
            return nums[i];
        //交换
        int temp = nums[nums[i]];
        nums[nums[i]] = nums[i];
        nums[i] = temp;
        //这里的i--是为了抵消掉上面的i++，
        //交换之后需要原地再比较
        i--;
    }
    return -1;
}
```

* **时间复杂度 : O\(n\)**
* **空间复杂度 : O\(1\)** 

## 总结

* 开个坑，打算先把剑指offer的题刷一遍，从基础开始。
* 本题提供的两种解法其实思路是一样的，但稍微改变下就能降低空间复杂度
* 此两种解法也适用于[**LeetCode 287： Find the Duplicate Number**](../leetcode/287-find-the-duplicate-number.md)\*\*\*\*

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


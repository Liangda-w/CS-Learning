---
title: 剑指offer 11： 旋转数组中的最小数字 Find Minimum in Rotated Sorted Array (Leetcode 153&154)
date: '08/14/2020 10:00:00'
tags:
  - 剑指offer
  - Binary Search
categories:
  - 剑指offer
  - simple
description: 'The same as LeetCode 153 & 154: Find Minimum in Rotated Sorted Array'
---

# 11： 旋转数组中的最小数字

## **Description**

Difficulty: **Simple**

> Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
>
> \(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`\).
>
> Find the minimum element.
>
> The array may contain duplicates.
>
> **Example 1:**
>
> ```java
> Input: [1,3,5]
> Output: 1
> ```
>
> **Example 2:**
>
> ```java
> Input: [2,2,2,0,1]
> Output: 0
> ```

## **Solution - 1：**  Brute Force

**思路：** 暴力解，根据旋转数组的特性，我们可以确定

* 在旋转了至少一位的情况下，最小的数字是第一个比前面的数要小的数。
* 如果没有旋转，则第一位数为最小数。

这个方法的问题是数据量过大的时候，效率过低。

```java
class Solution {
    public int minArray(int[] numbers) {
        for(int i=0; i<numbers.length-1;i++){
            if(numbers[i] > numbers[i+1]){
                return numbers[i+1];
            }
        }
        return numbers[0]; // the first number is the smallest one
    }
}
```

**时间复杂度：O\(n\)** 遍历数组 

**空间复杂度：O\(1\)**

## **Solution - 2：**  Binary Search

**思路：** 一看到排序数组的查找，第一个应该想到的是二分法。正常的二分查找代码是这样的：

```java
public int search(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (nums[mid] == target)
            return mid;
        if (target < nums[mid])
            right = mid - 1;
        else
            left = mid + 1;
    }
    return -1;
}
```

但这道题的数组在中间有一次旋转，所以不能直接用以前的那种二分法来查找，我们可以参照上面代码换种思路不断的缩小查找范围。照旋转数组的特性可以以最小数为界限将它分为左右两边，这样左边数组任一元素要大于或等于右边数组元素：

![](https://raw.githubusercontent.com/Liangda-w/images/master/offer11.png)

设两个边界为low和high，并找到中间的位置numbers\[mid\] 和 numbers\[high\]进行比较：

* 如果`numbers[mid] > numbers[high]`，说明numbers\[mid\]位于左边数组，而最小数肯定在它的右边，所以我们把low调为mid+1
* 如果`numbers[mid]  < numbers[high]`，说明numbers\[mid\]位于右边数组，而最小数可能在它的左边或者是它本身，所以我们把high调为mid
* 如果`numbers[mid]  = numbers[high]`，无法判断numbers\[mid\]位于哪边，所以我们把high调为high-1来缩小范围

当low = high的时候跳出循环，此时返回numbers\[low\] 或者 numbers\[high\]即可。

**注：**由于数组旋转的数字数量可能为零，即没有旋转，所以不能将mid和low进行对比，因为在numbers\[mid\] &gt; numbers\[low\] 的情况下

* 如果没有旋转，那么numbers\[mid\]应位于右边数组
* 如果有旋转，那么numbers\[mid\]位于左边数组

```java
class Solution {
    public int minArray(int[] numbers) {
        int low = 0, high = numbers.length - 1;
        while (low < high) {
            int mid = (low + high) / 2;
            if (numbers[mid] > numbers[high]) low = mid + 1;
            else if (numbers[mid] < numbers[high]) high = mid;
            else high--;
        }
        return numbers[low];
    }
}
```

**时间复杂度：O\(log\(n\)\)** 在特例情况下（例如 \[1,1,1,1\]），会退化到 O\(n\) **空间复杂度：O\(1\)**

### Leetcode 153

这道题还有一个版本是假设数组中不存在重复元素，那么就可以省略最后一个else

```java
public int findMin(int[] nums) {
    int left = 0, right = nums.length - 1;
    while (left < right) {
        int middle = (left + right) / 2;
        if (nums[middle] < nums[right]) {
            // middle可能是最小值
            right = middle;
        } else {
            // middle肯定不是最小值
            left = middle + 1;
        }
    }
    return nums[left];
}
```

## **Solution - 3：**  Array sort

**思路：** 排序查找，我们使用从小到大的顺序对数组进行升序排列，排序完之后直接返回数组的第一个元素即可。

```java
public int minArray(int[] numbers) {
    Arrays.sort(numbers);
    return numbers[0];
}
```

## Summary

* 一看到排序数组的查找，第一个应该想到的是二分法。还有待练习！
* 二分查找是一种效率较高的查找方法，其可将遍历法的线性级别时间复杂度降低至对数级别。但是要求线性表必须采用顺序存储结构，而且表中元素按关键字有序排列。 题目中给出的是半有序数组，虽然传统二分告诉我们二分只能用在有序数组中，但事实上，只要是可以减治的问题，仍然可以使用二分思想。

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


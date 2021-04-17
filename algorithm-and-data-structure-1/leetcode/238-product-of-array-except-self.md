---
title: Leetcode 238：Product of Array Except Self
date: '3/5/2021 18:28:54'
tags: null
categories:
  - Leetcode
  - Medium
---

# 238: Product of Array Except Self

## \*\*\*\*[**Description**](https://leetcode.com/problems/product-of-array-except-self/)\*\*\*\*

Difficulty: **Medium**

> Given an array `nums` of _n_ integers where _n_ &gt; 1, return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.
>
> **Example:**
>
> ```java
> Input:  [1,2,3,4]
> Output: [24,12,8,6]
> ```
>
> **Constraint:** It's guaranteed that the product of the elements of any prefix or suffix of the array \(including the whole array\) fits in a 32 bit integer.
>
> **Note:** Please solve it **without division** and in O\(_n_\).
>
> **Follow up:** Could you solve it with constant space complexity? \(The output array **does not** count as extra space for the purpose of space complexity analysis.\)

看似很简单的一道题，最暴力的解法就是同两个for loop算。有了O\(n\)的限制后可以考虑先把总的积求出来，再除以位置上不用考虑的数，但是题目也禁止了使用除法。这样只能考虑用乘法得到结果：

我们可以把每个i位置output上的结果拆分为

1. 在**i左边的所有数的乘积**
2. 在**i右边的所有数的乘积**

**的积**

示例：

```java
[1,2,3,4] = 1 * 24(2*12)
[1,2,3,4] = 1(1*1) * 12(3*4)
[1,2,3,4] = 2(1*2) * 4(1*4)
[1,2,3,4] = 6(2*3) * 1

L = [1, 1, 2, 6]
R = [24, 12, 4, 1]  
O = L*R = [24, 12, 8, 6]
```

## Solution 

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int len = nums.length;

        int[] L = new int[len];
        int[] R = new int[len];
        int[] out = new int[len];

        L[0] = 1; // for the first element, there's no num on the left side
        for(int i=1; i<len; i++){
            out[i] = out[i-1] * nums[i-1];
        }

         R[len-1] = 1; // for the last element, there's no num on the right side
        for(int i=len-2; i>=0; i--){
            R[i] = R[i+1] * nums[i+1];
        }

        for(int i=0; i<len; i++){
            out[i] = L[i] * R[i];
        }
        return out;
    }
}
```

但是用这个方法我们建了3个新的array，空间复杂度为O\(3N\)，为节省空间，我们可以只用一个out array，把L和R的计算合在一起：

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int len = nums.length;
        int[] out = new int[len];

        out[0] = 1; // now out is computing L
        for(int i=1; i<len; i++){
            out[i] = out[i-1] * nums[i-1];
        }

        int R = 1; // integer to save R value 
        for(int i=len-1; i>=0; i--){
            out[i] = out[i] * R;
            R *= nums[i];
        }

        return out;
    }
}
```

## Summary 

* 很巧妙的方法，看似无从下手的时候可以试着把output拆分从而寻找规律。

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


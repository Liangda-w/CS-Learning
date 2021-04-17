---
title: Leetcode 11：Container With Most Water
date: '3/6/2021 18:28:54'
categories:
  - Leetcode
  - Medium
---

# 11: Container With Most Water

## \*\*\*\*[**Description** ](https://leetcode.com/problems/container-with-most-water/)

Difficulty: **Medium**

> Given `n` non-negative integers `a1, a2, ..., an` , where each represents a point at coordinate `(i, ai)`. `n` vertical lines are drawn such that the two endpoints of the line `i` is at `(i, ai)` and `(i, 0)`. Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.
>
> **Notice** that you may not slant the container.
>
> **Example 1:**
>
> ![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)
>
> ```text
> Input: height = [1,8,6,2,5,4,8,3,7]
> Output: 49
> Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
> ```
>
> **Example 2:**
>
> ```text
> Input: height = [1,1]
> Output: 1
> ```
>
> **Example 3:**
>
> ```text
> Input: height = [4,3,2,1,4]
> Output: 16
> ```
>
> **Example 4:**
>
> ```text
> Input: height = [1,2,1]
> Output: 2
> ```
>
> **Constraints:**
>
> * `n == height.length`
> * `2 <= n <= 105`
> * `0 <= height[i] <= 104`

The volume of the container is width\*height

* width is the length of x aches
* height is the smaller one of the two lines

Thus, we can use two pointer starting from the most left and right side and move to the middle step by step to search for the maximal volume.

容器的体积等于底×高，底等于x轴的长度，高是两边柱子中短的一根的长度，因此这道题可以用双指针从最左最右开始一点点往中间走寻找最高值。

Example：

```java
[1,8,6,2,5,4,8,3,7]

8 * 1 = 8
7 * 7 = 49
6 * 3 = 18
5 * 8 = 40
3 * 4 = 12
2 * 5 = 10
1 * 2 = 2
```

## Solution - Two Pointer:

```java
class Solution {
    public int maxArea(int[] height) {
        int len = height.length;
        int width = len -1;

        int i=0; //first 
        int j=len-1; // last 
        int out = 0;
        int p1, p2;

        while (i<j){
            // it's important to set the updated value of p1 and p2 here
            p1 = height[i];
            p2 = height[j];
            out = Math.max(out, width * (p1>=p2 ? p2 : p1));

            // we change the position of shorter column and keep the longer one
            if(p1>p2){
                j -= 1;
                width--;
            } else if(p1<p2){
                i += 1;
                width--;
            } else { // if equal, we move both pointers
                j -= 1;
                i += 1;
                width -= 2;
            }            
        }        
        return out;             
    }
}
```

## Summary 

* pointer, pointer, pointer!

I'll keep on sharing some interesting tasks and solution each week, please feel free to leave a comment or pull request, if you have any questions or suggestions!\(●'◡'●\)


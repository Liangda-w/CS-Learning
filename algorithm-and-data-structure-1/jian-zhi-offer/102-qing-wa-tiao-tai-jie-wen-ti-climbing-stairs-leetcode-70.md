---
title: 剑指offer 10-2： 青蛙跳台阶问题 Climbing Stairs (Leetcode 70)
date: '08/12/2020 10:00:00'
tags:
  - 剑指offer
  - Dynamic Programming
categories:
  - 剑指offer
  - simple
description: 'The same as LeetCode 70: Climbing Stairs'
---

# 10-2： 青蛙跳台阶问题

## **Description** 

Difficulty: **Simple**

> You are climbing a stair case. It takes _n_ steps to reach to the top.
>
> Each time you can either climb **1 or 2** steps. In how many distinct ways can you climb to the top?
>
> **Example 1:**
>
> ```text
> Input: 2
> Output: 2
> Explanation: There are two ways to climb to the top.
> 1. 1 step + 1 step
> 2. 2 steps
> ```
>
> **Example 2:**
>
> ```text
> Input: 3
> Output: 3
> Explanation: There are three ways to climb to the top.
> 1. 1 step + 1 step + 1 step
> 2. 1 step + 2 steps
> 3. 2 steps + 1 step
> ```
>
> **Constraint：**
>
> ```text
> 0 <= n <= 45
> ```

乍一看很复杂，但是问题可以一步步拆分，要求climb\(n\)，我们可以选择在climb\(n-1\) 的时候选择上一个台阶或者在 climb\(n-2\) 的时候上两节台阶，如此拆分下去最后climb\(1\) = 1, climb\(2\) = 2。

没错，本质上这就是个Fibonacci数列，详细三种解法可以参考：[10-1. 斐波那契数列](101-fei-bo-na-qi-shu-lie-fibonacci-number-leetcode-509.md)。


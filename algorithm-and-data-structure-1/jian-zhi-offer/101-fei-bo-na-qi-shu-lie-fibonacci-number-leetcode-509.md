---
title: 剑指offer 10-1： 斐波那契数列 Fibonacci Number (Leetcode 509)
date: '08/10/2020 10:00:00'
tags:
  - 剑指offer
categories:
  - 剑指offer
  - simple
description: 'The same as LeetCode 509: Fibonacci Number'
---

# 10-1： 斐波那契数列

## **Description**

Difficulty: **Simple**

> The **Fibonacci numbers**, commonly denoted `F(n)` form a sequence, called the **Fibonacci sequence**, such that each number is the sum of the two preceding ones, starting from `0` and `1`. That is,
>
> ```java
> F(0) = 0,   F(1) = 1
> F(N) = F(N - 1) + F(N - 2), for N > 1.
> ```
>
> Given `N`, calculate `F(N)`.
>
> 答案需要**取模** 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
> Example:
>
> ```java
> Input: 4
> Output: 3
> Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.
> ```

一开始没明白取模是什么意思，后来看了下评论区：因为当数字越来越大后面在n = 45左右的时候会超过integer的上限，所以通过 `% 1000000007` 来表达。

## **Solution - 1：**  Iterative 动态规划

**思路：** 非递归，动态规划，按照式子一步步算。从计算效率、空间复杂度上看，本题最佳解法。

```java
class Solution {
    public int fib(int n) {
        if(n <= 1) return n;
        int f1 = 0;
        int f2 = 1;
        for(int i=2; i<=n;i++){
            int sum = (f1 + f2) % 1000000007;
            f1 = f2;
            f2 = sum;
        }
        return f2;
    }
}
```

**Time complexity:** O\(n\) 

**Space complexity:** O\(1\)

## **Solution - 2：**  Recursive

**思路：** 把 f\(n\) 问题的计算拆分成 f\(n−1\) 和 f\(n−2\)两个子问题的计算并递归，以 f\(0\) 和 f\(1\) 为终止条件。

**缺点**： 大量重复的递归计算，例如 f\(n\) 和 f\(n−1\) 两者向下递归需要各自计算 f\(n−2\) 的值，n&gt;41的时候就超时了。

```java
class Solution {
    public int fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1; 
        return (fib(n-1)% 1000000007 + fib(n-2)% 1000000007) % 1000000007;
    }
}
```

**Time complexity**: O\(2^n\) since T\(n\) = T\(n-1\) + T\(n-2\)is an exponential time **Space complexity:** O\(n\) space for recursive function call stack

## **Solution - 3：**  Recursive with HashMap

**思路：** 在递归法的基础上，新建一个长度为 n 的数组，用于在递归时存储 f\(0\) 至 f\(n\) 的数字值，重复遇到某数字则直接从数组取用，避免了重复的递归计算。除了HashMap也可以用array。

```java
class Solution {
   int constant = 1000000007;

    public int fib(int n) {
        return fib(n, new HashMap());
    }

    public int fib(int n, Map<Integer, Integer> map) {
        if (n < 2)
            return n;
        if (map.containsKey(n))
            return map.get(n);
        int first = fib(n - 1, map) % constant;
        map.put(n - 1, first);
        int second = fib(n - 2, map) % constant;
        map.put(n - 2, second);
        int res = (first + second) % constant;
        map.put(n, res);
        return res;
    }
}
```

**Time complexity**: O\(n\) calculate once for each number 

**Space complexity:** O\(n\) space for HashMap

## **Summary**

* 本题最优解其实是Iterative，recursive看似代码更少，但时间空间上不够效率，第三个解法一定程度上解决了这个问题。

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


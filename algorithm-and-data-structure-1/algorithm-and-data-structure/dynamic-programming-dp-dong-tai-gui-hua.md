# Dynamic Programming \(DP\) 动态规划

## Dynamic Programming 

The idea is very simple, If you have solved a problem with the given input, then save the result for future reference, so as to avoid solving the same problem again.. shortly _'Remember your Past'_. If the given problem can be broken up in to smaller sub-problems and these smaller subproblems are in turn divided in to still-smaller ones, and in this process, if you observe some over-lapping subproblems, then its a big hint for DP. A Dynamic Programming solution is based on the principal of **Mathematical Induction**.

### Top-Down & Bottom-Up

1. **Top-Down :** Start solving the given problem by **breaking it down**. If you see that the problem has been solved already, then just return the saved answer. If it has not been solved, solve it and save the answer. This is usually easy to think of and very intuitive. This is referred to as _**Memoization**_.
   * **Recursion** uses the top-down approach to solve the problem i.e. It begin with core\(main\) problem then breaks it into subproblems and solve these subproblems similarily. In this approach same subproblem can occur multiple times and consume more CPU cycle ,hence increase the time complexity. Whereas in Dynamic programming same subproblem will not be solved multiple times but the prior result will be used to optimise the solution. 
2. **Bottom-Up** **:** Analyze the problem and see the order in which the sub-problems are solved and start solving from the **trivial subproblem,** up towards the given problem. In this process, it is guaranteed that the subproblems are solved before solving the problem. This is referred to as **Dynamic Programming**
   * **Iterative**

Note that **divide and conquer** is slightly a different technique. In that, we divide the problem in to **non-overlapping** subproblems and solve them independently, like in **mergesort** and quick sort.

Complementary to Dynamic Programming are **Greedy Algorithms** which make a decision once and for all every time they need to make a choice, in such a way that it leads to a \*near-optimal solution.

动态规划是用一张可变的表格来存储运算结果，大多用于解决最优化问题（题目表述形如“使某个量最优”）。它通过用空间换时间来实现算法**时间复杂度的优化**。

### 分类

**分治 \(Divide And Conquer\)：最优子结构**

* 为了解决一个问题，把它分解成若干个与此问题相似的子问题。这样的“能分解”的性质就叫做最优子结构（又称无后效性）。很多问题都可以满足这个性质。

**动规 \(DP\)：最优子结构、重叠子问题**

* 动态规划是分治的特例。采用分治思想得到的子问题“不一定需要再次求解”，因为之前可能已经计算过相同的子问题了。这样的性质叫做重叠子问题。

**贪心 \(greedy algorithm\)：最优子结构、重叠子问题、贪心选择性质**

* 贪心比动态规划更特殊，它还需要问题满足另一个性质——贪心选择性质。每次都可以把原问题分解为一个子问题。

**动态规划是一种特殊的分治，而贪心是一种特殊的动态规划。**

### 实现思路

**递归形式 \(Recursion\)：**改分治。先进行判断，如果这个子问题已经处理过，那就直接把数组里储存了的值输出；否则就“计算结果”，最后储存答案。从所需结果出发不断回溯前一运算直到回到初值再递推得到所需结果----从**未知到已知**

**递推形式 \(Iterative\)**：找出一种可行的拓扑序列，从初值出发反复进行某一运算得到所需结果。-----从**已知到未知**

### Example: Minimum Steps to One

**Problem Statement:** On a positive integer, you can perform any one of the following 3 steps.

1. Subtract 1 from it. \( n = n - 1 \) 
2. If its divisible by 2, divide by 2. \( if n % 2 == 0 , then n = n / 2 \) , 
3. If its divisible by 3, divide by 3. \( if n % 3 == 0 , then n = n / 3 \). 

Now the question is, given a positive integer _n_, find the minimum number of steps that takes _n_ to 1

**Example:**

1. For n = 1 , output: 0  
2. For n = 4 , output: 2 \( 4 /2 = 2 /2 = 1 \)
3. For n = 7 , output: 3 \( 7 -1 = 6  /3 = 2  /2 = 1 \)

**Idea:** One can think of greedily choosing the step, which makes _n_ as low as possible and conitnue the same, till it reaches 1. If you observe carefully, the greedy strategy doesn't work here. Eg: Given _n_ = 10 , Greedy --&gt; 10 /2 = 5 -1 = 4 /2 = 2 /2 = 1 \( 4 steps \). But the optimal way is --&gt; 10 -1 = 9 /3 = 3 /3 = 1 \( 3 steps \). So, we need to try out all possible steps we can make for each possible value of _n_ we encounter and choose the minimum of these possibilities.

It all starts with **recursion**:

```java
F(n) =  1 + min{ F(n-1) , F(n/2) , F(n/3) } if (n>1) , 
else 0 ( F(1) = 0 ) .
```

Now that we have our recurrence equation, we can right way start coding the recursion. Wait.., does it have over-lapping subproblems ? YES. Is the optimal solution to a given input depends on the optimal solution of its subproblems ? Yes... Bingo ! its DP. So, we just store the solutions to the subproblems we solve and use them later on, as in memoization..

**Top-Down**

```java
int memo[n+1]; // we will initialize the elements to -1 ( -1 means, not solved it yet )

int getMinSteps ( int n ){
    if ( n == 1 ) return 0; // base case
    if( memo[n] != -1 ) return memo[n]; // we have solved it already

    int r = 1 + getMinSteps( n - 1 ); // '-1' step . 'r' will contain the optimal answer finally
    if( n%2 == 0 )  r = min( r , 1 + getMinSteps( n / 2 ) ) ; // '/2' step
    if( n%3 == 0 )  r = min( r , 1 + getMinSteps( n / 3 ) ) ; // '/3' step

    memo[n] = r ; // save the result. If you forget this step, then its same as plain recursion.
    return r;
}
```

or we start from bottom and move up till the given _n_, as in dp.

**Bottom-Up** 

```java
int getMinSteps ( int n ){
    int dp[n+1] , i;
    dp[1] = 0; // trivial case

    for( i = 2 ; i < = n ; i ++ ){
        dp[i] = 1 + dp[i-1];
        if(i%2==0) dp[i] = min( dp[i] , 1+ dp[i/2] );
        if(i%3==0) dp[i] = min( dp[i] , 1+ dp[i/3] );
    }
    return dp[n];
}
```

Both the approaches are fine. But one should also take care of the lot of over head involved in the function calls in Memoization, which may give StackOverFlow error or TLE rarely.

## **Summary**

* DP及其衍生是算法中很重要的的一节，这次总结总算是把递归和递推两个概念和我所学的 Rekursiv 和 Iterativ 连上了，一开始在网上看到这两个词还挺懵的。
* 递归节省代码长度，递推节省运行所需时间和空间，看需求选择。
* 递归的难点在于找到终止条件和路上的调用本身函数时的参数等，递推的难度在于如何找到对应问题的数学方程式。

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)

Reference: [Tutorial for Dynamic Programming ](https://www.codechef.com/wiki/tutorial-dynamic-programming)， [知乎：动态规划](https://www.zhihu.com/topic/19660018/intro)


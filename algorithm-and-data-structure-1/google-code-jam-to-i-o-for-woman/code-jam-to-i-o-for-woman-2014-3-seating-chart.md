# 2014 \(3\) Seating Chart

## Description 

[**C**](https://codingcompetitions.withgoogle.com/codejamio/round/0000000000050ed0/0000000000050df3)\*\*\*\*[**ode Jam to I/O for Woman 2014 \(3\) Seating Chart \(17 pts, 28 pts\)**](https://codingcompetitions.withgoogle.com/codejamio/round/0000000000050ed0/0000000000050df3)\*\*\*\*

> #### Problem \(Simplified\)
>
> There are **K** round tables at the dinner, numbered 1 through **K**. It is important to have exactly the same number of people sitting at each table. If that is impossible \(**N** is not divisible by **K**\), then the table with the most people must have at most one more person sitting at it than the table with the fewest people.
>
> Each of the **N** people will be assigned a unique number between 0 and **N** - 1. What matters is who is sitting next to whom, and not exactly where they're sitting. In other words, two arrangements, A and B, are considered different if there exists a pair of numbers, α and β, such that persons α and β are sitting next to each other at the same table in arrangement A, but they are not sitting next to each other in arrangement B.
>
> For example, if **N** is 5, and **K** is 2, we must have 3 people seated at one of the tables, and 2 people seated at the other table. Here is the list of all 10 of the possible arrangements:
>
> ```text
> [[0, 1, 2], [3, 4]]
> [[0, 1, 3], [2, 4]]
> [[0, 1, 4], [2, 3]]
> [[0, 2, 3], [1, 4]]
> [[0, 2, 4], [1, 3]]
> [[0, 3, 4], [1, 2]]
> [[1, 2, 3], [0, 4]]
> [[1, 2, 4], [0, 3]]
> [[1, 3, 4], [0, 2]]
> [[2, 3, 4], [0, 1]]
> ```
>
> All other arrangements are similar to one of the arrangements above and are not counted as different. In particular, all of the following arrangements are considered to be the same:
>
> ```text
> [[0, 1, 2], [3, 4]]
> [[2, 0, 1], [3, 4]]
> [[1, 2, 0], [4, 3]]
> [[0, 2, 1], [3, 4]]
> [[3, 4], [0, 2, 1]]
> ```
>
> This is because the following pairs of people \(and no other pairs\) are sitting next to each other in each of these 5 arrangements:
>
> ```text
> 0 and 1
> 0 and 2
> 1 and 2
> 3 and 4
> ```
>
> Another example is **N** = 5 and **K** = 3, which requires having two tables with two people each, and one table with a single person sitting at it. There are 15 possible arrangements in this case:
>
> ```text
> [[0, 1], [2, 3], [4]]
> [[0, 1], [2, 4], [3]]
> [[0, 1], [3, 4], [2]]
> [[0, 2], [1, 3], [4]]
> [[0, 2], [1, 4], [3]]
> [[0, 2], [3, 4], [1]]
> [[0, 3], [1, 2], [4]]
> [[0, 3], [1, 4], [2]]
> [[0, 3], [2, 4], [1]]
> [[0, 4], [1, 2], [3]]
> [[0, 4], [1, 3], [2]]
> [[0, 4], [2, 3], [1]]
> [[1, 2], [3, 4], [0]]
> [[1, 3], [2, 4], [0]]
> [[1, 4], [2, 3], [0]]
> ```
>
> In this final example, **N** = 5 and **K** = 1, which means that we only have a single table, seating all 5 guests. Here, the answer is 12:
>
> ```text
> [[0, 1, 2, 3, 4]]
> [[0, 1, 2, 4, 3]]
> [[0, 1, 3, 2, 4]]
> [[0, 1, 3, 4, 2]]
> [[0, 1, 4, 2, 3]]
> [[0, 1, 4, 3, 2]]
> [[0, 2, 1, 3, 4]]
> [[0, 2, 1, 4, 3]]
> [[0, 2, 3, 1, 4]]
> [[0, 2, 4, 1, 3]]
> [[0, 3, 1, 2, 4]]
> [[0, 3, 2, 1, 4]]
> ```
>
> #### Input
>
> The first line of the input gives the number of test cases, **T**. **T** lines, each one containing two integers, **N** and **K**.
>
> #### Output
>
> For each test case, output one line containing "Case \#x: y", where x is the test case number \(starting from 1\) and **y is the number of different possible seating arrangements.**

## Thinking Steps

### Different arrangements of N people at one table

If we want to place N people on one table and starting from a random sit:

1. for the first sit, we have N people to choose from, 
2. for the second sit, we have N-1 people to choose from, 
3. for the third sit, we have N-2 people to choose from

......

there will be `n*(n-1)*(n-2)...2*1 = n!` different combinations.

But beside this, we need to pay attention to this 2 specific cases:

1. `1 2 3 4 5` \(clock-wise\) presents the same as `5 4 3 2 1` \(anti-clockwise\) in this problem, we divide n! by 2.
2. `1 2 3 4 5` \(start counting from 1\) presents the same as `2 3 4 5 1` \(start counting from 2\) in this problem, we divide n! by n as we can count form n different positions.

Therefore there will be `n!/2/n = (n-1)!/2` different combinations if we want to place N people at one table and ensure in each combination at least one person has a different neighbor.

Note: this formula is only valid when `n >= 3`, if `n=2` or `n=1`, there will be only one arrangement.

### Different arrangements of N people at K table

#### Case 1: N can be divided by K

If we have N=12, K=3, we will have 4 people at each table and there are `(4-1)!/2 = 3!/2 = 3` combinations for each table.

Then we need to consider how we choose the people for each table. In the example above:

1. we first choose 4 from 12 people \(12 choose 4\)
2. we then choose 4 from 8 people \(8 choose 4\)
3. the last 4 people automatically form 1 group \(4 choose 4\)

Beside this, the order of each group should not be considered. For example the combination of `1234`, `5678` and `9 10 11 12` is the same as `5678`, `9 10 11 12` and `1234`. Therefore we need to divided the result by `3!` \(in general `K!`\)

Putting them together, the final combination for this example should be

$$
\frac{(12C4*3)*(8C4*3)*(4C4*3)}{3!} = \frac{(495*3)*(70*3)*(1*3)}{6} = 155925
$$

The general formula is

$$
\frac{(nCk*(n-1)!/2)*((n-k)Ck*(n-1)!/2)*...*(kCk*(n-1)!/2)}{K!}
$$

```java
// n can be divided by k    
public static long arrange_kind(int n, int k) {
    long res = 1;
    int left = n;
    int right = n / k; // number of people on each table

    while (left >= right) {
        res *= combi[left][right]; //leftCright
        if (right > 2) { 
            // different combination of right people on one table
            // fomular only valid for right>2, otherwise right=1
            res *= fak[right - 1] / 2; 
        }
        left -= right; 
    }

    return res / fak[k];
}
```

#### Case 2: N cannot be divided by K

For example N=13 and K=3, we will have 2 tables which contains 4 people and 1 table contains 5 people. In this case we can divide this into 2 problem using divide-and-conquer:

1. choose 5 from 13 people and count different arrangements of 5 people at 1 tables: 

   $$
   13C5*((5-1)!/2) = 1287*12 =15444
   $$

2. different arrangements of 8 people at 2 tables 

   $$
   \frac{(8C4*3)*(4C4*3)}{2!} = \frac{(70*3)*(1*3)}{2} = 315
   $$

So in summary we have `15444*315 = 4864860` different combinations.

```java
    if (n % k == 0) {
        ans = arrange_kind(n, k);
    } else {
        int groupA = n % k; // the number of group with 1 more person
        int groupB = k - n % k;
        // the totoal number of people which should be divided into groupA group
        int sumA = (n / k + 1) * groupA; 
        int sumB = (n / k) * groupB;

        a = arrange_kind(sumA, groupA);                
        b = arrange_kind(sumB, groupB);
        // you can also use [sumA] instead, they have the same result 
        // because nCk = nC(n-k)
        ans = combi[n][sumB] * a * b; 
    }
```

### Combinatoric - N choose K

In order to get the value of combinatoric `N choose K` quickly, we create a 2-dimensional array `combi` to store the values. `combi[n][k]` simply stores the value of `N choose K`.

```text
0C0
1C0 1C1
2C0 2C1 2C2
3C0 3C1 3C2 3C3
4C0 4C1 4C2 4C3 4C4
5C0 5C1 5C2 5C3 5C4 5C5
6C0 6C1 6C2 6C3 6C4 6C5 6C6
7C0 7C1 7C2 7C3 7C4 7C5 7C6 7C7
8C0 8C1 8C2 8C3 8C4 8C5 8C6 8C7 8C8
9C0 9C1 9C2 9C3 9C4 9C5 9C6 9C7 9C8 9C9
```

values:

```text
1 
1 1 
1 2 1 
1 3 3 1 
1 4 6 4 1 
1 5 10 10 5 1 
1 6 15 20 15 6 1 
1 7 21 35 35 21 7 1 
1 8 28 56 70 56 28 8 1 
1 9 36 84 126 126 84 36 9 1
```

The formula for `(N choose K)` is

$$
N! \over (N-K)!K!
$$

Therefore, `N choose K+1` is

$$
N \  choose  \ (K+1) = \frac {N!} {(N-(K+1)!(K+1)!} = \frac {N!} {(N-(K+1)!(K+1)!} \\ 
= \frac {N!} {(N-K-1)!(K+1)!} = \frac {N!} {\frac{(N-K)!} {N-K}K!(K+1)} \\
= \frac {N!} {(N-K)!K!* \frac {K+1} {N-K}}  = \frac {N!} {(N-K)!K!} * \frac {N-K} {K+1}\\
= N \  choose  \ K * \frac {N-K} {K+1}
$$

Beside this, we know for the simplest case: `nC0 = nCn = 1`.

Putting them together, we can generate a helper class \(the system out are for your validation\):

```java
// for the large dataset, n can be maximal 20
// results for n from 0 to 9 are shown above
public static int[][] combination() {
    int[][] combi = new int[21][21];
    for (int n = 0; n < 21; n++) {
        int nck = 1; // initialize with the first value nC0
        combi[n][0] = nck;
        // System.out.print(combi[n][0] + " ");
        for (int k = 0; k < n; k++) { // k couldn't be bigger than n
            combi[n][k + 1] = combi[n][k] * (n - k) / (k + 1); // the formula above
            // System.out.print(combi[n][k + 1] + " ");
        }
        // System.out.println();
    }
    return combi;
}
```

### Factorial

We need another helper method to calculate the factorial:

```java
// for the large dataset, n can be maximal 20
// results for n from 0 to 9 are shown above    
public static long[] factorial() {
    long[] fak = new long[21];
    fak[0] = 1; // 0! = 1
    fak[1] = 1; // 1! = 1
    for (int n = 2; n < 21; n++) {
        fak[n] = fak[n - 1] * n; // n! = n* (n-1) * (n-2) * ... * 1 = n* (n-1)!
    }
    return fak;
}
```

## Solution 

```java
import java.util.*;
import java.io.*;

public class Solution {
    static int[][] combi = combination();
    static long[] fak = factorial();

    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedReader(new InputStreamReader(System.in)));
        int t = in.nextInt();
        for (int i = 1; i <= t; ++i) {
            int n = in.nextInt();
            int k = in.nextInt();
            long ans = 0, a = 0, b = 0;

            if (n % k == 0) {
                ans = arrange_kind(n, k);
            } else {
                int groupA = n % k; // the number of group with 1 more person
                int groupB = k - n % k;
                int sumA = (n / k + 1) * groupA; 
                int sumB = (n / k) * groupB;

                a = arrange_kind(sumA, groupA);                
                b = arrange_kind(sumB, groupB);
                // you can also use [sumA] instead, they have the same result 
                // because nCk = nC(n-k)
                ans = combi[n][sumB] * a * b;
            }

            System.out.println("Case #" + i + ": " + ans);
        }
    }

    public static long arrange_kind(int n, int k) {
        long res = 1;
        int left = n;
        int right = n / k;

        while (left >= right) { // O(n), if k=1
            res *= combi[left][right];
            if (right > 3) {
                res *= fak[right - 1] / 2;
            }
            left -= right;
        }

        return res / fak[k];
    }

    public static int[][] combination() {
        int[][] combi = new int[21][21];
        for (int n = 0; n < 21; n++) { // O(n*k)
            int nck = 1; 
            combi[n][0] = nck;
            for (int k = 0; k < n; k++) { 
                combi[n][k + 1] = combi[n][k] * (n - k) / (k + 1); 
            }
        }
        return combi;
    }

    public static long[] factorial() {
        long[] fak = new long[21];
        fak[0] = 1;
        fak[1] = 1;
        for (int n = 2; n < 21; n++) { // O(n)
            fak[n] = fak[n - 1] * n;
        }
        return fak;
    }
}
```

* **Time complexity : O\(n\*k\)**
* **Space complexity : O\(k\)**

## Summary

* Quite difficult problem which requires good combinatoric knowledge.
* Use divide and conquer principle to handle the situation that n can't be divided by k.
* Combinatoric and Factorial are good helper class.

## Reference

1. [**StackOverflow: Combinatoric 'N choose R' in java math?**](https://stackoverflow.com/questions/2201113/combinatoric-n-choose-r-in-java-math)\*\*\*\*
2. \*\*\*\*[**CSDN: Code Jam to I/O for Women 2014 Seating Chart 排座组合数**](https://blog.csdn.net/qq_39004117/article/details/105204382)\*\*\*\*


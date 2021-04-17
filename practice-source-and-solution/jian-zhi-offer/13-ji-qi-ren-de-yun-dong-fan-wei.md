---
title: 剑指offer 13： 机器人的运动范围
date: '10/12/2020 10:00:00'
tags:
  - 剑指offer
categories:
  - 剑指offer
  - medium
---

# 13： 机器人的运动范围

## **Description** 

Difficulty: **Medium**

> 地上有一个m行n列的方格，从坐标 \[0,0\] 到坐标 \[m-1,n-1\] 。一个机器人从坐标 \[0, 0\] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 \[35, 37\] ，因为3+5+3+7=18。但它不能进入方格 \[35, 38\]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？
>
> 示例 1：
>
> ```text
> 输入：m = 2, n = 3, k = 1
> 输出：3
> ```
>
> 示例 2：
>
> ```text
> 输入：m = 3, n = 1, k = 0
> 输出：1
> ```

## **Solution - 1：**  Brute-force O\(n\*m\)

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        int[][] matrix = new int[m][n]; // matrix if the positon is already visited
        matrix[0][0] = 1; // init for start point (0,0)
        int count = 1;

        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(i==0 && j==0) continue; //skip the (0,0)  
                if(sum(i, j) <= k){ 
                    // if the position is reachable
                    if((i>=1 && matrix[i-1][j]==1) || (i<m-1 && matrix[i+1][j]==1) 
                    || (j>=1 && matrix[i][j-1]==1) || (j<n-1 && matrix[i][j+1]==1)){
                        matrix[i][j] = 1;
                        count++;
                    }
                }
            }
        }
        return count;
    }

    int sum (int x, int y){ // calculate the sum of the digit of the coordinates
        int sum = 0;
        while(x > 0){
            sum += x%10;
            x /= 10;
        }
        while(y > 0){
            sum += y%10;
            y /= 10;
        }
        return sum;               
    }
}
```

**时间复杂度：O\(n\*m\)** **空间复杂度：**

**O\(n\*m\)** 新建数组matrix的维度为 nm

## **Solution - 2：**  BFS O\(n\*m\)

本质上还是个DFS/BFS的问题

关于**Queue，FIFO**

|  | Throw exception | Return false |
| :--- | :--- | :--- |
| Insert | add\(e\) | offer\(e\) |
| Remove | remove\(\) | poll\(\) |
| Examine | element\(\) | peek\(\) |

```java
public int movingCount(int m, int n, int k) {
    //临时变量visited记录格子是否被访问过
    boolean[][] visited = new boolean[m][n];
    int res = 0;
    //创建一个队列，保存的是访问到的格子坐标，是个二维数组
    Queue<int[]> queue = new LinkedList<>();
    //从左上角坐标[0,0]点开始访问，add方法表示把坐标
    // 点加入到队列的队尾
    queue.add(new int[]{0, 0});
    while (queue.size() > 0) {
        //这里的poll()函数表示的是移除队列头部元素，因为队列
        //是先进先出，从尾部添加，从头部移除 => BFS
        int[] x = queue.poll();
        int i = x[0], j = x[1];
        //i >= m || j >= n是边界条件的判断，k < sum(i, j)判断当前格子坐标是否
        // 满足条件，visited[i][j]判断这个格子是否被访问过
        if (i >= m || j >= n || k < sum(i, j) || visited[i][j]) continue;
        //标注这个格子被访问过
        visited[i][j] = true;
        res++;
        //把当前格子右边格子的坐标加入到队列中
        queue.add(new int[]{i + 1, j});
        //把当前格子下边格子的坐标加入到队列中
        queue.add(new int[]{i, j + 1});
    }
    return res;
}

private int sum(int i, int j) {
    int sum = 0;
    while (i != 0) {
        sum += i % 10;
        i /= 10;
    }
    while (j != 0) {
        sum += j % 10;
        j /= 10;
    }
    return sum;
}
```

**时间复杂度：O\(n\*m\)** 最差情况下，机器人遍历矩阵所有单元 

**空间复杂度：O\(n\*m\)** 最差情况下， `visited` 内存储矩阵所有单元格的索引

## **Solution - 3：**  DFS O\(n\*m\)

关于**Queue，FIFO**

|  | Throw exception | Return false |
| :--- | :--- | :--- |
| Insert | add\(e\) | offer\(e\) |
| Remove | remove\(\) | poll\(\) |
| Examine | element\(\) | peek\(\) |

```java
public int movingCount(int m, int n, int k) {
    //临时变量visited记录格子是否被访问过
    boolean[][] visited = new boolean[m][n];
    return dfs(0, 0, m, n, k, visited);
}

public int dfs(int i, int j, int m, int n, int k, boolean[][] visited) {
    //i >= m || j >= n是边界条件的判断，k < sum(i, j)判断当前格子坐标是否
    // 满足条件，visited[i][j]判断这个格子是否被访问过
    if (i >= m || j >= n || k < sum(i, j) || visited[i][j])
        return 0;
    //标注这个格子被访问过
    visited[i][j] = true;
    //沿着当前格子的右边和下边继续访问
    return 1 + dfs(i + 1, j, m, n, k, visited) + dfs(i, j + 1, m, n, k, visited);
}

private int sum(int i, int j) {
    int sum = 0;
    while (i != 0) {
        sum += i % 10;
        i /= 10;
    }
    while (j != 0) {
        sum += j % 10;
        j /= 10;
    }
    return sum;
}
```

**时间复杂度：O\(n\*m\)** 最差情况下，机器人遍历矩阵所有单元 **空间复杂度：O\(n\*m\)** 最差情况下， `visited` 内存储矩阵所有单元格的索引

## Summary

* DFS/BFS重点！

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


---
title: Leetcode 1559： Detect Cycles in 2D Grid
date: '08/23/2020 10:00:00'
tags:
  - Leetcode
  - DFS
categories:
  - Leetcode
  - Hard
---

# 1559: Detect Cycles in 2D Grid

## \*\*\*\*[**Description**](https://leetcode.com/problems/detect-cycles-in-2d-grid/) 

Difficulty: **Hard**

> Given a 2D array of characters grid of size m x n, you need to find if there exists any cycle consisting of the same value in grid.
>
> A cycle is a path of **length 4 or more in the grid that starts and ends at the same cell**. From a given cell, you can move to one of the cells adjacent to it in one of the four directions \(up, down, left, or right\), if it has the same value of the current cell.
>
> Also, you cannot move to the cell that you visited in your last move. For example, the cycle \(1, 1\) -&gt; \(1, 2\) -&gt; \(1, 1\) is invalid because from \(1, 2\) we visited \(1, 1\) which was the last visited cell.
>
> Return true if any cycle of the same value exists in grid, otherwise, return false.
>
> **Example 1:**
>
> ```java
> Input: grid = [
>     ["a","a","a","a"],
>     ["a","b","b","a"],
>     ["a","b","b","a"],
>     ["a","a","a","a"]
> ]   
> Output: true
> Explanation: There are two valid cycles。
> ```
>
> **Example 2:**
>
> ```java
> Input: grid = [
>     ["c","c","c","a"],
>     ["c","d","c","c"],
>     ["c","c","e","c"],
>     ["f","c","c","c"]
> ]
> Output: true
> Explanation: There is only one valid cycle highlighted in the image below:
> ```
>
> **Example 3:**
>
> ```java
> Input: grid = [
>     ["a","b","b"],
>     ["b","z","b"],
>     ["b","b","a"]
> ]
> Output: false
> ```
>
> **Constraints:**
>
> m == grid.length n == grid\[i\].length 1 &lt;= m &lt;= 500 1 &lt;= n &lt;= 500 grid consists only of lowercase English letters.

## **Solution - 1：**  DFS Recursion

### Time Limit Exceeded \(1\)

和 [12. 矩阵中的路径](https://liangda-w.github.io/2020/08/22/offer-12/) 非常像，所以这里套用了大概思路，不过很多要注意的地方：

* 因为是找环，递归的终结点是回到了开始的那个点，所以额外标记了初始点为 '!'
* 因为上一点又出了很多问题，比如bfs函数并不知道是初始点，直接对比然后返回了false。所以用了额外一个参数z记录目前是在第几个点上（初始点k=1），这样也可以防止在第二个点上直接找到第一个点返回true。
* 除了初始点，其余走过的点也要标记 '.'
* 标记结束如果没找到的情况下还要记得改回去，不管是初始点还是其余走过的点

不过这个方法在70/74个测试用例的时候超时了，好可惜。

```java
class Solution {
    public boolean containsCycle(char[][] grid) {
        int x = grid.length;
        int y = grid[0].length;
        for(int i=0; i<x; i++){
            for(int j=0; j<y;j++){
                char c = grid[i][j];
                grid[i][j] = '!'; // mark the first char
                if(dfs(grid, c, i, j, 1)) return true;
                grid[i][j] = c; // set back
            }
        }
        return false;
    }

    public boolean dfs(char[][] grid, char c, int x, int y, int z){
         if(x<0 || x>grid.length-1 || y<0 || y>grid[0].length-1){
             return false; // outside of the border
         }

         if(grid[x][y] == '!' && z < 4 && z != 1){
             return false; // prevent from the second to the first one 
         } 

         if(grid[x][y] == '!' && z >= 4){
             return true; // find the cycle
         }

         if(grid[x][y] != c){
             if(z != 1) return false; // notice that the first one is also != c
         }

         char tmp = grid[x][y];
         if(z != 1) grid[x][y] = '.'; // mark the other char

         boolean b = dfs(grid, c, x+1, y, z+1) || dfs(grid, c, x-1, y, z+1) || dfs(grid, c, x, y-1, z+1) || dfs(grid, c, x, y+1, z+1);

         grid[x][y] = tmp; // set back
         return b;
    }
}
```

### Time Limit Exceeded \(2\)

看讨论区的时候又想到可以用另一个矩阵来记录走过的位置，一旦找到一个相距4以上一样的的就可以返回true，因为不一定初始的点就在圆里，而是后面形成的圆。按理应该会节省不少时间，但是竟然止步于68/74。

```java
class Solution {
    public boolean containsCycle(char[][] grid) {
        int x = grid.length;
        int y = grid[0].length;
        if(x == 0 || y == 0) return false;

        int[][] finished = new int[x][y];
        for(int i=0; i<x; i++){
            for(int j=0; j<y;j++){
                finished[i][j] = 1;
                if(dfs(grid, finished, grid[i][j], i, j, 1)) return true;
                finished = new int[x][y]; 
            }
        }
        return false;
    }

    public boolean dfs(char[][] grid, int[][] finished, char c, int x, int y, int z){
        if(x<0 || x>grid.length-1 || y<0 || y>grid[0].length-1 || grid[x][y] != c){
            return false;
        }

        if(finished[x][y] != 0){
            if(z - finished[x][y] >= 4) return true;
            if(z - finished[x][y] == 2) return false;
        }
        finished[x][y] = z;        
        boolean b =  dfs(grid, finished, c, x+1, y, z+1) ||  dfs(grid, finished, c, x, y-1, z+1) || dfs(grid, finished, c, x-1, y, z+1) || dfs(grid, finished, c, x, y+1, z+1);   
        finished[x][y] = 0; 
        return b;  
    }
}
```

### Time Limit Exceeded \(3\)

所以按照自己最初的想法做了一遍，代码不是很有效，还是止步于68/74。

```java
class Solution {
    public boolean containsCycle(char[][] grid) {
        int x = grid.length;
        int y = grid[0].length;
        if(x == 0 || y == 0) return false;

        int[][] finished = new int[x][y];
        for(int i=0; i<x; i++){
            for(int j=0; j<y;j++){
                finished[i][j] = 1; // the first
                // to the second one                
                if(dfs(grid, finished, grid[i][j], i, j, 2)) return true; 
                finished = new int[x][y]; 
            }
        }
        return false;
    }

    public boolean dfs(char[][] grid, int[][] finished, char c, int x, int y, int z){
        if( x!=0 && grid[x-1][y] == c){
            // if(grid[x-1][y] != c) return false; 
            // wrong, because we need to proof the other case below, never return false
            if(finished[x-1][y] != 0){
                if(z - finished[x-1][y] >= 4) return true;
                // if(z - finished[x-1][y] == 2) return false; the same here
            } else if(finished[x-1][y] == 0){
                finished[x-1][y] = z;
                if(dfs(grid, finished, c, x-1, y, z+1)) return true;
                finished[x-1][y] = 0;
            }          
        }

        if( x!=grid.length-1 && grid[x+1][y] == c ){
            if(finished[x+1][y] != 0){
                if(z - finished[x+1][y] >= 4) return true;
            } else if(finished[x+1][y] == 0){
                finished[x+1][y] = z;
                if(dfs(grid, finished, c, x+1, y, z+1)) return true;
                finished[x+1][y] = 0;
            }       
        }

        if( y!=0 && grid[x][y-1] == c){
            if(finished[x][y-1] != 0){
                if(z - finished[x][y-1] >= 4) return true;
            } else if(finished[x][y-1] == 0){
                finished[x][y-1] = z;
                if(dfs(grid, finished, c, x, y-1, z+1)) return true;
                finished[x][y-1] = 0;
            }
        }

        if( y!=grid[0].length-1 && grid[x][y+1] == c){
            if(finished[x][y+1] != 0){
                if(z - finished[x][y+1] >= 4) return true;
            } else if(finished[x][y+1] == 0){
                finished[x][y+1] = z;
                if(dfs(grid, finished, c, x, y+1, z+1)) return true;
                finished[x][y+1] = 0;
            }
        }
        return false;  
    }
}
```

### Accepted

参考解题里的思路

* 建立了一个2\*2的矩阵表示4个可能的坐标移动
* 参数里加入上一个走过的点的坐标，递归前先确认，防止立马往回跑。

至于为什么这个就不超时。。。应该是因为保证了调用递归函数的次数尽可能少，也就是提前筛掉不符合的情况，特别是next = last 的情况，毕竟是三分之一的概率。

```java
class Solution {
    int[][] direction = {{1,0},{-1,0},{0,1},{0,-1}}; // four possible position change
    public boolean containsCycle(char[][] grid) {
        int x = grid.length;
        int y = grid[0].length;
        if(x == 0 || y == 0) return false;

        int[][] visited = new int[x][y];
        for(int i=0; i<x; i++){
            for(int j=0; j<y;j++){
                if(visited[i][j] == 1) continue;
                if(dfs(grid, visited, grid[i][j], i, j, -1, -1)) return true;
            }
        }
        return false;
    }

    public boolean dfs(char[][] grid, int[][] visited, char c, int x, int y, int lx, int ly){
        if(visited[x][y] == 1) return true; // we find the cycle
        visited[x][y] = 1;
        for(int[]  d: direction){
            int nx = x + d[0]; // first number
            int ny = y + d[1]; // second number
            if(nx == lx && ny == ly) continue; // next = last
            if(nx < 0 || nx>grid.length-1 || ny<0 || ny>grid[0].length-1 || grid[nx][ny] != c) continue;
            if(dfs(grid, visited, c, nx, ny, x, y)) return true;
        }
        return false;  
    }
}
```

**时间复杂度：O\(3^\(nm\)\)** 最差情况下如果矩阵中的所有字符一样，需要遍历矩阵中的所有字符串。\(?\) **空间复杂度：O\(n\*m\)** 递归深度最坏情况下为 nm \(?\)

## Solution - 2 : Union Find

这类题目并查集也是很有效的解法，可惜没怎么用过不熟练：

* 利用并查集的思想，相同字母可以形成连通区域。
* 从左上角顶点开始，同时向右和向下搜索，若字母相同则合并。
* 合并时，若发现 x 和 y 的 parent 相同，即形成环。

```java
class Solution {                     
    public boolean containsCycle(char[][] grid) {
        int h = grid.length;
        int w = grid[0].length;
        DSU dsu = new DSU(w * h);
        for (int i = 0; i < h; ++i) {
            for (int j = 0; j < w; ++j) {
                char cur = grid[i][j];
                // 向右搜索
                if (j + 1 < w && cur == grid[i][j + 1]) {
                    if (dsu.union(i * w + j, i * w + j + 1)) {
                        return true;
                    }
                }
                // 向下搜索
                if (i + 1 < h && cur == grid[i + 1][j]) {
                    if (dsu.union(i * w + j, (i + 1) * w + j)) {
                        return true;
                    }
                }
            }
        }
        return false;
    }


    class DSU {
        int[] parent;

        public DSU(int N) {
            parent = new int[N];
            for (int i = 0; i < N; ++i) {
                parent[i] = i;
            }
        }

        public int find(int x) {
            if (parent[x] != x) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }

        /**
         * 若合并前，x和y的parent相同，则表示形成环，返回true。
         *
         * @param x
         * @param y
         * @return
         */
        public boolean union(int x, int y) {
            int parentX = find(x);
            int parentY = find(y);
            if (parentX == parentY) {
                return true;
            }
            if (parentX < parentY) {
                parent[parentY] = parentX;
            } else {
                parent[parentX] = parentY;
            }
            return false;
        }
    }
}
```

## Summary

* 第一次参加周赛，这道题本身其实遇到过类似的，但是超时什么的太难搞了吧。
* 递归的超时一般是因为调用递归函数次数过多，解决方案是提前设参数进行筛选。
* if\(...\) return true 能节省很多时间

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


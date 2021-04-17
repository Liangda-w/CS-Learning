---
title: 剑指offer 12： 矩阵中的路径 Word Search (Leetcode 79)
date: '08/22/2020 10:00:00'
tags:
  - 剑指offer
  - DFS
categories:
  - 剑指offer
  - medium
description: 'The same as LeetCode 79: Word Search'
---

# 12： 矩阵中的路径

## **Description** 

Difficulty: **Medium**

> Given a 2D board and a word, find if the word exists in the grid.
>
> The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.
>
> **Example:**
>
> ```java
> board =
> [
>   ['A','B','C','E'],
>   ['S','F','C','S'],
>   ['A','D','E','E']
> ]
>
> Given word = "ABCCED", return true.
> Given word = "SEE", return true.
> Given word = "ABCB", return false.
> ```

## **Solution - 1：**  DFS Recursion

这道因为二维数组感觉和 [Leetcode 130: Surrounded Regions](https://liangda-w.github.io/2020/06/21/Leetcode-130/) 很像，也可以用递归函数解决。因为一个数只能用一次我们可以建立一个二维数组finished来表示这个位置上的数是否已经用过（用过：1，没用过：0）。

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        if(word.length() == 0) return false;
        int len1 = board.length;
        int len2 = board[0].length;
        int[][] finished = new int[len1][len2]; 
        char w = word.charAt(0);

        for(int i = 0; i < len1; i++){
            for(int j = 0; j < len2; j++){
                if(board[i][j] == w){
                    finished[i][j] = 1;             
                    if (search(board,finished,word.substring(1), i, j)) return true;
                    // true, return directly; false, continue
                }
                // reset finished 
                finished = new int[len1][len2]; 
            }
        }
        return false;
    }

    public boolean search(char[][] board, int[][] finished, String word, int x, int y){
        if(word.length() == 0) return true;
        char w = word.charAt(0);

            if(x != 0 && board[x-1][y] == w && finished[x-1][y] != 1){
                finished[x-1][y] = 1;
                if(search(board,finished,word.substring(1), x-1, y)) return true;
                // true, return directly; false, continue, don't foeget to reset
                finished[x-1][y] = 0;
            }

            if(x != board.length-1 && board[x+1][y] == w && finished[x+1][y] != 1){
                finished[x+1][y] = 1;
                if(search(board,finished,word.substring(1), x+1, y)) return true;
                finished[x+1][y] = 0;
            }

            if(y != 0 && board[x][y-1] == w && finished[x][y-1] != 1){
                finished[x][y-1] = 1;
                if(search(board,finished,word.substring(1), x, y-1)) return true;
                finished[x][y-1] = 0;
            }

            if(y != board[0].length-1 && board[x][y+1] == w && finished[x][y+1] != 1){
                finished[x][y+1] = 1;
                if(search(board,finished,word.substring(1), x, y+1)) return true;
                finished[x][y+1] = 0;
            }

        return false;
    }
}
```

**时间复杂度：O\(3^k\)** 最差情况下，需要遍历矩阵中的所有相符长度的字符串。走过的位置无法回头，所有是3。 

**空间复杂度：O\(n\*m\)** 新建数组finished的维度为 nm，递归深度最坏情况下为 nm， 即k为mn

m,n 为矩阵的长宽，k为字符串word的长度。

其实还可以再把代码简化一些，比如

* 避免重复这里，可以不额外建finished，而是把走过的位置上的数设为特殊符号以标记，可以节省空间复杂度
* 把上下左右的搜查用\|\|连接起来，节省很多代码行数，但是要先确认x，y的值有没有超出边界。
* 不用substring，而是把word直接转化成char array

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        char[] words = word.toCharArray();
        for(int i = 0; i < board.length; i++) {
            for(int j = 0; j < board[0].length; j++) {
                if(dfs(board, words, i, j, 0)) return true;
            }
        }
        return false;
    }
    boolean dfs(char[][] board, char[] word, int i, int j, int k) {
        if(i >= board.length || i < 0 || j >= board[0].length || j < 0 || board[i][j] != word[k]) return false;
        if(k == word.length - 1) return true;
        char tmp = board[i][j];
        board[i][j] = '/';
        boolean res = dfs(board, word, i + 1, j, k + 1) || dfs(board, word, i - 1, j, k + 1) || 
                      dfs(board, word, i, j + 1, k + 1) || dfs(board, word, i , j - 1, k + 1);
        board[i][j] = tmp;
        return res;
    }
}
```

## Summary

* 思路都是一样的，感觉自己写出来的虽然对我而言是最好理解的，但代码还是不够简洁，有很多可以节省优化的地方。

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


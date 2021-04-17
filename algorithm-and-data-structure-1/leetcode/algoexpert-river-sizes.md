---
title: AlgoExpert：River Sizes
date: '3/7/2021 18:28:54'
tags:
  - dfs
categories:
  - Medium
---

# AlgoExpert: River Sizes

## **Description** 

Difficulty: **Medium**

> You're given a two-dimensional array \(a matrix\) of potentially unequal height and width containing only 0s and 1s. Each 0 represents land and each 1 represents part of a river. A river consists of any number of 1s that are either horizontally or vertically adjacent \(but not diagonally adjacent\). The number of adjacent 1s forming a river determine its size.
>
> Note that a river can twist. In other words, it doesn't have to be a straight vertical line or a straight horizontal line; it can be L-shaped, for example.
>
> Write a function that returns an array of the sizes of all rivers represented in the input matrix. The sizes don't need to be in any particular order.
>
> **Example:**
>
> ```java
> matrix:  [
>     [1,0,0,1,0],
>     [1,0,1,0,0],
>     [0,0,1,0,1],
>     [1,0,1,0,1],
>     [1,0,1,1,0],  
> ]
> output = [1,2,2,2,5]
> ```

## Solution - 1：DFS

```java
import java.util.*;

class Program {
  public static List<Integer> riverSizes(int[][] matrix) {
        ArrayList<Integer> result = new ArrayList<Integer>();

        for(int i = 0; i < matrix.length; i++){
            for(int j = 0; j < matrix[0].length; j++){
                if(matrix[i][j] == 1){
                      // when we visit a 1, mark it to 0
                    matrix[i][j] = 0;
                      // counter start from 1
                    result.add(dfs(matrix, i, j, 1));                     
                }
            }
        }

    return result;
  }

    public static int dfs (int[][] matrix, int i, int j, int counter){
        // go left
        if(j > 0 && matrix[i][j-1] == 1){
                matrix[i][j-1] = 0;
                    // don't return it, but reasign counter for the later usage
                 counter = dfs(matrix, i, j-1, ++counter);
        }

        // go right 
        if(j < matrix[0].length -1 && matrix[i][j+1] == 1){
                matrix[i][j+1] = 0;
                counter = dfs(matrix, i, j+1, ++counter);
        }

        // go up
        if(i > 0 && matrix[i-1][j] == 1){
                  matrix[i-1][j] = 0;
                counter = dfs(matrix, i-1, j, ++counter);
        }

        // go down 
        if(i < matrix.length -1 && matrix[i+1][j] == 1){
                matrix[i+1][j] = 0;
                counter = dfs(matrix, i+1, j, ++counter);
        }

        return counter;
    }
}
```

## Summary 

* DFS用过好几次，但是每道题的递归和return还是有所区别。

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


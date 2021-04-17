---
title: Leetcode 130： Surrounded Regions
date: '6/21/2020 18:28:54'
tags:
  - Leetcode
  - BFS
  - DFS
  - Union Find
categories:
  - Leetcode
  - Medium
---

# 130: Surrounded Regions

## \*\*\*\*[**Description** ](https://leetcode.com/problems/surrounded-regions/)

Difficulty: **Medium**

> Given a 2D board containing `'X'` and `'O'` **\(the letter O\)**, capture all regions surrounded by `'X'`. A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.
>
> **Example:**
>
> ```text
> X X X X
> X O O X
> X X O X
> X O X X
> ```
>
> After running your function, the board should be:
>
> ```text
> X X X X
> X X X X
> X X X X
> X O X X
> ```
>
> **Explanations:**
>
> Surrounded regions shouldn’t be on the border, which means that any `'O'` on the border of the board are not flipped to `'X'`. Any `'O'` that is not on the border and it is not connected to an `'O'` on the border will be flipped to `'X'`. Two cells are connected if they are adjacent cells connected horizontally or vertically.

如 `'X'` 模块四周均被 `'O'` 包围，则将 `'X'` 换成 `'O'`。

**特殊情况考虑：**

* `char[][] board` 可以是空的， 所以要先验证是否长度为0，如果是，则终止此方法 `return;`，Testcase其实只有一个 `[]` 要特殊考虑，所以理论上第二个for循环不需要：

```java
int a = board.length; 
if(a = 0) return;

int b = board[0].length;
for(int i=0; i<a; i++){
    if(board[i].length == 0 || board[i].length != b) return;
}
```

## Solution - 1 Recursive

**思路：**看到这题的第一反应是写一个递归函数 \(EN: recursive / DE: Rekursion\)，找到第一个 `'O'`，调用此递归函数：

* 如此`'O'`在边界上，不变
* 如上下左右均为 `'X'`，则换成 `'O'`，
* 如检测到了 `'X'`，则再次调用此递归函数，

然而仔细一想：因为每次都会检查上下左右，那相邻的`'O'`会陷入死循环， 除非能记住是从哪个`'O'`被调用的。-&gt; 解决方法：加二个参数记住被调用的`'O'`的坐标。

那么问题又来了：比如4个相邻的`'O'`呈正方形，那么同样会进入死循环，因为第四个`'O'`不认识第一个`'O'`。-&gt; 解决方法：加一个双重数组参数 `char[][] checked` 记住所有已经检测过的`'O'`的位置。

理论上应该是可行的，[详情参见大佬总结](https://leetcode.wang/leetcode-130-Surrounded-Regions.html)，但容易超时，此方法弃用。

## Solution - 2 DFS 

**思路：**既然问题是如何记住从哪被调用的，那么只要把被调用的`'O'`标记或换掉就可以了，因此换了一个思路，不再找应该被替换的`'O'`，而是不应该被替换\(即临边的`'O'`\)：

1. 找到临边界的`'O'`，先把它们变成其他任意的字母（我这里选了`'A'`）
2. 通过临边界的`'O'`找到所有与之相邻的`'O'`并把他们变成`'A'`，这里`change（...）`方法用了递归
3. 把剩余的`'O'`变成`'X'`
4. 最后再把所有`'A'`变回`'O'`

翻评论区发现这里面蕴藏了一个有名有姓的算法： **Depth first search / Tiefensuche / 深度优先搜索**

> The algorithm starts at the root node \(selecting some arbitrary node as the root node in the case of a graph\) and explores as far as possible along each branch before backtracking.

### Java 

```java
class Solution {
    public void solve(char[][] board) {
        int a = board.length; 
        if(a == 0){
            return;
        }

        int b = board[0].length;
        if(b == 0){
            return;
        }

        for(int i=0; i<a;i++){ //left top to left down
            if(board[i][0] == 'O'){
                board[i][0] = 'A';
                change(board, i, 0, a, b);
            }
        }

        for(int i=0; i<a;i++){ //right top to right down
            if(board[i][b-1] == 'O'){
                board[i][b-1] = 'A';
                change(board, i, b-1, a, b);
            }
        }  

        for(int j=0; j<b;j++){ //up left to up right
            if(board[0][j] == 'O'){
                board[0][j] = 'A';
                change(board, 0, j, a, b);
            }
        }

        for(int j=0; j<b;j++){ //down left to down right
            if(board[a-1][j] == 'O'){
                board[a-1][j] = 'A';
                change(board, a-1, j, a, b);
            }
        }

        for(int i=1; i<a-1; i++){ //change all 'O' left (means they are surround by 'X') to 'X'
            for(int j=1; j<b-1; j++){
                if(board[i][j] == 'O'){
                    board[i][j] = 'X';
                }
            }
        }

        for(int i=0; i<a; i++){ //change all 'A' to 'O'
            for(int j=0; j<b; j++){
                if(board[i][j] == 'A'){
                    board[i][j] = 'O';
                }
            }
        }
    }

    public void change(char[][] board, int i, int j, int a, int b){
        if(i-1>=0 && board[i-1][j] == 'O'){//Up
            board[i-1][j] = 'A';
            change(board, i-1, j, a, b);
        }

        if(i+1<a-1 && board[i+1][j] == 'O'){//down
            board[i+1][j] = 'A';
            change(board, i+1, j, a, b);
        }

        if(j+1<b-1 && board[i][j+1] == 'O' ){//right
            board[i][j+1] = 'A';
            change(board, i, j+1, a, b);
        }

        if(j-1>=0 && board[i][j-1] == 'O'){//left
            board[i][j-1] = 'A';
            change(board, i, j-1, a, b);
        }
     }
}
```

**测试结果：**内存占用有点多...

> Runtime: 3 ms （beats 27%） Memory Usage: 49.4 MB \(beats 6%\)

### C++

其实一开始是打算用C++做的，结果一看给的初始代码...被两个vector吓到了。

```text
class Solution {
    public:
    void solve(vector<vector<char>>& board) {

    }
};
```

后来看了评论区大佬的代码发现并没有任何区别 `vector<vector<char>>& board` = `char[][] board`，思路跟上面一样，而且跟我自己写的比简洁了太多，下面加了一些自己的注释（Java也可以如下简化）：

```java
class Solution {
    public:
    void solve(vector<vector<char>>& board) {
        if(board.empty())return;
        int row = board.size(),col = board[0].size();

        for(int i=0;i<col;i++)DFS(board,0,i,row,col),DFS(board,row-1,i,row,col);//first row and last row

        for(int i=0;i<row;i++)DFS(board,i,0,row,col),DFS(board,i,col-1,row,col); //first column and last column

        for(int i=0;i<row;i++)
            for(int j=0;j<col;j++)
                if(board[i][j]=='O')board[i][j]='X';
                else if(board[i][j]=='P')board[i][j]='O'; //together
    }

    void DFS(vector<vector<char>>& board,int r,int c,int rsize,int csize){
        if(r<0||c<0||r==rsize||c==csize||board[r][c]!='O')return; //together
        board[r][c] = 'P';
        DFS(board,r+1,c,rsize,csize);
        DFS(board,r,c+1,rsize,csize);
        DFS(board,r-1,c,rsize,csize);
        DFS(board,r,c-1,rsize,csize);
    }
};
```

## Solution - 3 BFS

与**DFS**对应，还有：**Breadth-first search （BFS） / Breitensuche / 宽度优先搜索算法。**

> It starts at the tree root \(or some arbitrary node of a graph, sometimes referred to as a 'search key'\[1\]\), and explores all of the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level.

两者的区别：

感觉一般DFS更常用一些（通过递归很容易挖深度），BFS就要借助 queue \(FIFO\)/deque/vector \(C++: Standard Template Library STL：Sequential Container）或者Queue/LinkedList/ArrayDeque（Java）。找到一个`'O'`后直接找上下左右并存在Container里面，下面是整合的C++ queue版本:

```java
void solve(vector<vector<char&board) { // the same as DFS
    int width = board.size();
    if (width == 0) //Add this to prevent run-time error!
        return;
    int length = board[0].size();
    if  (length == 0) // Add this to prevent run-time error!
        return;

    for (int i = 0; i < length; ++i){
        if (board[0][i] == 'O')
            bfsBoundary(board, 0, i);
        if (board[width-1][i] == 'O')
            bfsBoundary(board, width-1, i);
    }

    for (int i = 0; i < width; ++i){
        if (board[i][0] == 'O')
            bfsBoundary(board, i, 0);
        if (board[i][length-1] == 'O')
            bfsBoundary(board, i, length-1);
    }

    for (int i = 0; i < width; ++i){
        for (int j = 0; j < length; ++j){
            if (board[i][j] == 'O')
                board[i][j] = 'X';
            else if (board[i][j] == '#')
                board[i][j] = 'O';
        }
    }
}

void BFS(vector<vector<char>&board, int i, int j) {
    int m = board.size();
    int n = board[0].size();
    queue<pair<int, intq;
    q.push(make_pair(i, j));
    while (!q.empty()) {
        pair<int, intelem = q.front(); //oldest element
        q.pop(); //delete the oldest element
        i = elem.first;
        j = elem.second;
        if (i >= 0 && i < m && j >=0 && j < n && board[i][j] == 'O') {
            board[i][j] = '#';
            q.push(make_pair(i - 1, j)); //down
            q.push(make_pair(i + 1, j)); //up
            q.push(make_pair(i, j - 1)); //left
            q.push(make_pair(i, j + 1)); //right
        }
    }
}
```

## Solution - 4 Union Find

看评论区发现这道题的另一种解法是 **Union Find 并查集**，定义如下：

> 在计算机科学中，**并查集**是一种树型的数据结构，用于处理一些**不交集（Disjoint Sets）**的合并及查询问题。有一个**联合-查找算法（union-find algorithm）**定义了两个用于此数据结构的操作：
>
> * `Find`：确定元素属于哪一个子集。它可以被用来确定两个元素是否属于同一子集。
> * `Union`：将两个子集合并成同一个集合。
>
> 由于支持这两种操作，一个不相交集也常被称为联合-查找数据结构（union-find data structure）或合并-查找集合（merge-find set）。其他的重要方法，MakeSet，用于创建单元素集合。有了这些方法，许多经典的划分问题可以被解决。 为了更加精确的定义这些方法，需要定义如何表示集合。一种常用的策略是为每个集合选定一个固定的元素，称为代表，以表示整个集合。接着，`Find(x)` 返回 x 所属集合的代表，而 `Union` 使用两个集合的代表作为参数。

这道题我们就可以把`'O'`分为两大类，一类不需要被转化成`'O'`（即联通边界），另一类需要（即被全方位包围）。

首先我们把每个节点各作为一类，用它的行数和列数生成一个 id 标识该类。

```java
    int node(int i, int j) {
        return i * cols + j;
    }
```

然后遍历每个`'O'`节点

* 如果联通边界，就把它和 `dummyNode` 节点（一个在所有节点外的节点）合并
* 否则和它上下左右的`'O'`合并

最后就会把所有连通到边界的`'O'`和 `dummyNode` 合为了一类。

```java
public class Solution {
    int rows, cols;

    public void solve(char[][] board) {
        if(board == null || board.length == 0) return;

        rows = board.length;
        cols = board[0].length;

        UnionFind uf = new UnionFind(rows * cols + 1);//0,1,2...rows * cols(dummy)
        int dummyNode = rows * cols; //all outer O should connected to this dummy

        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(board[i][j] == 'O') {
                    if(i == 0 || i == rows-1 || j == 0 || j == cols-1) {
                        uf.union(node(i,j), dummyNode); //all outer O are connected to this dummy
                    }
                    else {
                        if(i 0 && board[i-1][j] == 'O')  uf.union(node(i,j), node(i-1,j)); //down
                        if(i < rows-1 && board[i+1][j] == 'O')  uf.union(node(i,j), node(i+1,j)); // up
                        if(j 0 && board[i][j-1] == 'O')  uf.union(node(i,j), node(i, j-1)); // left
                        if(j < cols-1 && board[i][j+1] == 'O')  uf.union(node(i,j), node(i, j+1)); //right
                    }
                }
            }
        }

        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(uf.isConnected(node(i,j), dummyNode)) { //if connected, no change
                    board[i][j] = 'O';
                }
                else { //change
                    board[i][j] = 'X';
                }
            }
        }
    }

    int node(int i, int j) {
        return i * cols + j; //id to identify each node
    }
}

class UnionFind {
    int [] parents;
    public UnionFind(int totalNodes) { //Constructor
        parents = new int[totalNodes];
        for(int i = 0; i < totalNodes; i++) {
            parents[i] = i;
        }
    }

    void union(int node1, int node2) {
        int root1 = find(node1);
        int root2 = find(node2);
        if(root1 != root2) {
            parents[root2] = root1; // only one representative
        }
    }

    int find(int node) { // find the representative
        while(parents[node] != node) {
            parents[node] = parents[parents[node]]; 
            node = parents[node]; 
        }
        return node;
    }

    boolean isConnected(int node1, int node2) {
        return find(node1) == find(node2);
    }
}
```

## Summary 

* 作为刷的第一题Leetcode（6.17当天的每日挑战，本来以为是每天一个新题，后来才发现都是原有题库里的...）意外得挺有趣，有时候思路稍微转化一下就是一个全新的解法（DFS / BFS）。 评论区的Union Find更是让我眼前一亮。
* 开始刷题之前纠结了一下该用什么语言，就我目前而言，可以大概掌握用来刷题的语言有Java和C/C++。本来想着要不两个用两个语言都做一遍，但发现没有这个必要，因为其实算法和思路才是关键，语言只是辅助，正所谓换汤不换药，况且两个语言之间相似度也很高。以后可能会专注于Java，毕竟学的时间长一点，当然遇到有趣的C++解题思路还是会一样分享的。（不过C++能实现的Java也一定可以？）
* 其实触及到的三个算法（DFS，BFS和Union Find）我去年在Algorithmen und Datenstruktur学过，但是因为是个理论课，并没有尝试着把理论转化为代码进行实践，也是看到了这题才恍然想起来是这么回事儿。所以实践和记录都很重要，学的知识真的是太容易忘了。

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


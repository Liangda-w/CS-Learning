---
title: 剑指offer 07： 重建二叉树 (Leetcode 105)
date: '7/25/2020 10:00:00'
tags:
  - 剑指offer
  - Binary Tree
  - HashMap
  - Stack
categories:
  - 剑指offer
  - medium
description: >-
  The same as LeetCode 105: Construct Binary Tree from Preorder and Inorder
  Traversal
---

# 07： 重建二叉树

## **Description** 

Difficulty: **Medium**

> Given preorder and inorder traversal of a tree, construct the binary tree.
>
> **Note:** You may assume that duplicates do not exist in the tree.
>
> For example, given
>
> ```java
> preorder = [3,9,20,15,7] // 前序遍历（VLR）: 首先访问根结点，然后遍历左子树，最后遍历右子树。
> inorder = [9,3,15,20,7] // 中序遍历（LDR）: 首先遍历左子树，然后访问根结点，最后遍历右子树。
> ```
>
> Return the following binary tree:
>
> ```java
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
```

## **Solution - 1：**  递归 + HashMap

**思路：**

> **前序遍历（VLR）:** 首先访问根结点，然后遍历左子树，最后遍历右子树。 **中序遍历（LDR）:** 首先遍历左子树，然后访问根结点，最后遍历右子树。

* 前序遍历的**首个元素**即为根节点 `root` 的值
* 在中序遍历中搜索到根节点 `root`  
  * 根节点**前面**的节点在**左子树**
  * 根节点**后面**的节点在**右子树**

根据子树特点，我们可以通过同样的方法对左（右）子树进行划分，**每轮可确认三个节点的关系** 。此递推性质让我们联想到用 **递归方法** 处理：

* 递推参数： 前序遍历中根节点的索引pre\_root、中序遍历左边界in\_left、中序遍历右边界in\_right。
* 终止条件： 当 in\_left &gt; in\_right ，子树中序遍历为空，说明已经越过叶子节点，此时返回 null。
* 递推工作： 1. **建立根节点root**： 值为前序遍历中索引为pre\_root的节点值。 2. 通过`dic.get(po[pre_root])`搜索根节点root在中序遍历的索引i： 为了提升搜索效率，本题解使用**哈希表HashMap**预存储中序遍历的值与索引的映射关系，每次搜索的时间复杂度为 O\(1\)。 3. 构建根节点root的左子树和右子树： 通过调用 recur\(\) 方法开启下一层递归。
  * **左子树**： 根节点索引为 pre\_root + 1 ，中序遍历的左右边界分别为 in\_left 和 i - 1。
  * **右子树**： 根节点索引为 **i - in\_left** + pre\_root + 1（即： **左子树长度**+根节点索引+ 1），中序遍历的左右边界分别为 i + 1 和  in\_right。
* 返回值： 返回 root，含义是当前递归层级建立的根节点 root 为上一递归层级的根节点的左或右子节点。

```java
class Solution {
    HashMap<Integer, Integer> dic = new HashMap<>(); // for inorder search
    int[] po; // for preorder
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || preorder.length == 0) {
            return null;
           }
        po = preorder;
        for(int i = 0; i < inorder.length; i++) 
            dic.put(inorder[i], i);
        return recur(0, 0, inorder.length - 1);
    }

    TreeNode recur(int pre_root, int in_left, int in_right) {
        if(in_left > in_right) return null; // 终止条件，子树中序遍历为空
        TreeNode root = new TreeNode(po[pre_root]); //root
        int i = dic.get(po[pre_root]); //index = dic.get(value)
        root.left = recur(pre_root + 1, in_left, i - 1);
        root.right = recur(pre_root + i - in_left + 1, i + 1, in_right);
        return root;
    }
}
```

**时间复杂度 O\(n\)：** n为树的节点数量。

* 初始化 HashMap 需遍历 inorder ，占用 O\(n\)；
* 递归共建立 n个节点，每层递归中的节点建立、搜索操作占用 O\(1\) ，因此递归占用 O\(N\)。\(最差情况为所有子树只有左节点，树退化为链表，此时递归深度 O\(n\) ；平均情况下递归深度 O\(log\(n\)\)。

**空间复杂度 O\(n\)**： HashMap 使用 O\(n\)额外空间；递归操作中系统需使用 O\(n\) 额外空间。

## **Solution - 2：** 迭代 Iteration + Stack

例如要重建的是如下二叉树。

```java
        3
       / \
      9  20
     /  /  \
    8  15   7
   / \
  5  10
 /
4
```

其前序遍历和中序遍历如下。

```java
preorder = [3,9,8,5,4,10,20,15,7]
inorder = [4,5,8,10,9,3,15,20,7]
```

**思路：** 用栈保存遍历过的节点。初始时令inorder的指针指向第一个元素，遍历preorder的数组：

* 如果preorder的元素不等于inorder的指针指向的元素，则preorder的元素为上一个节点的左子节点。
* 如果preorder的元素等于inorder的指针指向的元素，则正向遍历inorder的元素同时反向遍历preorder的元素，找到最后一次相等的元素，将preorder的下一个节点作为最后一次相等的元素的右子节点。其中，反向遍历preorder的元素可通过栈的弹出元素实现。

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || preorder.length == 0) {
            return null;
        }

        TreeNode root = new TreeNode(preorder[0]); // root
        int length = preorder.length;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root); // 创建一个栈，将根节点压入栈内
        int inorderIndex = 0; // 初始化中序遍历下标为 0
        for (int i = 1; i < length; i++) {
            int preorderVal = preorder[i];
            TreeNode node = stack.peek();
            if (node.val != inorder[inorderIndex]) {
                node.left = new TreeNode(preorderVal); // 当前元素作为其上一个元素的左子节点
                stack.push(node.left); // 并将当前元素压入栈内。
            } else {
                while (!stack.isEmpty() && stack.peek().val == inorder[inorderIndex]){
                    node = stack.pop();
                    inorderIndex++;
                }
                node.right = new TreeNode(preorderVal); // 当前元素为最后一次相等的元素的右子节点
                stack.push(node.right);
            }
        }
        return root;
    }
}
```

**时间复杂度O\(n\):** 前序遍历和后序遍历都被遍历。

**空间复杂度O\(n\):** 额外使用栈存储已经遍历过的节点。

## **Summary** 

* 接触到的第一道二叉树的题，还是很有挑战的，数据结构的特点需要弄懂
* 感觉二叉树的题肯定会涉及到递归
* 用HashMap来搜索特定数值的位置真的是太机智了！

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


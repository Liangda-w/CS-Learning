---
title: 剑指offer 06： 从尾到头打印链表
date: '7/24/2020 10:00:00'
tags:
  - 剑指offer
categories:
  - 剑指offer
  - simple
---

# 06： 从尾到头打印链表

## **Description** 

Difficulty: **Simple**

> 输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。
>
> **示例 ：**
>
> ```java
> 输入：head = [1,3,2]
> 输出：[2,3,1]
> ```

```java
/**
 \* Definition for singly-linked list.
 \* public class ListNode {
 \*   int val;
 \*   ListNode next;
 \*   ListNode(int x) { val = x; }
 \* }
 */
```

## **Solution - 1：**  Stack

**思路：** 栈的特点是**后进先出**，即最后压入栈的元素最先弹出。考虑到栈的这一特点，使用栈将链表元素顺序倒置。从链表的头节点开始，依次将每个节点压入栈内，然后依次弹出栈内的元素并存储到数组中。

```java
class Solution {
    public int[] reversePrint(ListNode head) {
        Stack<ListNode> stack = new Stack<ListNode>(); // 创建一个栈，用于存储链表的节点
        ListNode temp = head; // 创建一个指针，初始时指向链表的头节点
        while (temp != null) {
            stack.push(temp); // 将指针指向的节点压入栈内
            temp = temp.next;
        }
        int size = stack.size();
        int[] print = new int[size];
        for (int i = 0; i < size; i++) {
            print[i] = stack.pop().val; //从栈内弹出一个节点，将该节点的值存储
        }
        return print;
    }
}
```

同理也可以用LinkedList：

```java
class Solution {
    public int[] reversePrint(ListNode head) {
        LinkedList<Integer> stack = new LinkedList<Integer>();
        while(head != null) {
            stack.addLast(head.val);
            head = head.next;
        }
        int[] res = new int[stack.size()];
        for(int i = 0; i < res.length; i++)
            res[i] = stack.removeLast();
    return res;
    }
}
```

**时间复杂度：O\(n\)** 正向遍历一遍链表，然后从栈弹出全部节点，等于又反向遍历一遍链表。 

**空间复杂度：O\(n\)** 额外使用一个栈存储链表中的每个节点。

## **Solution - 2：**  递归

**思路：** 先走至链表末端，回溯时依次将节点值加入列表 ，这样就可以实现链表值的倒序输出。

```java
class Solution {
    ArrayList<Integer> tmp = new ArrayList<Integer>();
    public int[] reversePrint(ListNode head) {
        recur(head);
        int[] res = new int[tmp.size()];
        for(int i = 0; i < res.length; i++)
            res[i] = tmp.get(i); // 将列表 tmp 转化为数组 res 
        return res; 
    }
    void recur(ListNode head) {
        if(head == null) return; //递归终止条件：走过链表尾部节点
        recur(head.next); // 递推阶段
        tmp.add(head.val); // 回溯阶段：将当前节点值加入列表
    }
}
```

**时间复杂度 O\(n\)：** 遍历链表，递归 N 次。 **空间复杂度 O\(n\)：** 系统递归需要使用 O\(n\) 的栈空间。

## **Solution - 3**

**思路：** 不用饯也不用递归，只要知道链表的长度即可。

```java
class Solution {
    public static int[] reversePrint(ListNode head) {
        ListNode node = head;
        int count = 0; // 链表长度
        while (node != null) {
            ++count;
            node = node.next;
        }
        int[] nums = new int[count];
        node = head;
        for (int i = count - 1; i >= 0; --i) {
            nums[i] = node.val;
            node = node.next;
        }
        return nums;
    }
}
```

**时间复杂度 O\(n\)**：同样是要扫描两趟 

**空间复杂度 O\(1\)：** 不需要额外分配空间

## **Summary** 

* 写这题的时候发现java的各种数据结构和方法名称都忘的差不多了。。
* 递归本质上就是一个栈，先进后出。

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


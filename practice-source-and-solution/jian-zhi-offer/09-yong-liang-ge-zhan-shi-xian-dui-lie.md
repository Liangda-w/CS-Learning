---
title: 剑指offer 09： 用两个栈实现队列
date: '08/09/2020 10:00:00'
tags:
  - 剑指offer
categories:
  - 剑指offer
  - simple
---

# 09： 用两个栈实现队列

## **Description** 

Difficulty: **Simple**

> 用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。\(若队列中没有元素，deleteHead 操作返回 -1 \)
>
> 示例 ：
>
> 输入：\["CQueue","appendTail","deleteHead","deleteHead"\]，\[\[\],\[3\],\[\],\[\]\] 输出：\[null,null,3,-1\]
>
> 输入：\["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"\]，\[\[\],\[\],\[5\],\[2\],\[\],\[\]\] 输出：\[null,-1,null,null,5,2\]

输入的第一个括号里的是调用函数的名称，第二个是调用时的参数（只有append需要参数）。

而输出里只有delete会返回一个数。

## **Solution - 1：**  ArrayList

**思路：** 理论上一个队列ArrayList就可以实现，但题意是如何用双栈来实现它。

```java
class CQueue {
    ArrayList<Integer> list;
    public CQueue() {
        list = new ArrayList<Integer>();
    }

    public void appendTail(int value) {
        list.add(value);
    }

    public int deleteHead() {
        if(list.size() == 0){
            return -1;
        }
        int i = list.get(0);
        list.remove(0);
        return i;
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```

## **Solution - 2：**  Stack

**思路：** 然而此题的用意是要使用栈，而栈有着**先进后出**的特性，意味着`deleteHead`需要把栈翻个底朝天才能找到头，所以在我们翻找的时候可以定义第二个栈用来存后面的元素，而又因为**先进后出**的特性，在我们删完第一个元素把后面的元素从第二个栈挪回来的时候，可以保证元素原来的顺序（其实不需要挪回来，放在那不影响）。这也是双栈的意思。

* 加入队尾 appendTail\(\)函数： 将数字 val 加入栈 A 即可。
* 删除队首deleteHead\(\)函数： 有以下三种情况。
  1. 当栈 B 不为空： B中仍有已完成倒序的元素，因此直接返回 B 的栈顶元素。
  2. 否则，当 A 为空： 即两个栈都为空，无元素，因此返回 −1 。
  3. 否则： 将栈 A 元素全部转移至栈 B 中，实现元素倒序，并返回栈 B 的栈顶元素。

另外关于LinkedList和Stack的选择问题：

> 使用java的同学请注意，如果你使用Stack的方式来做这道题，会造成速度较慢； 原因是Stack继承了Vector接口，而Vector底层是一个Object\[\]数组，那么就要考虑空间扩容和移位的问题了。 可以使用LinkedList来做Stack的容器，因为LinkedList实现了Deque接口，所以Stack能做的事LinkedList都能做，其本身结构是个双向链表，扩容消耗少。 但是我的意思不是像100%代码那样直接使用一个LinkedList当做队列，那确实是快，但是不符题意。

```java
class CQueue {
    LinkedList<Integer> A, B;
    public CQueue() {
        A = new LinkedList<Integer>();
        B = new LinkedList<Integer>();
    }
    public void appendTail(int value) {
        A.addLast(value);
    }
    public int deleteHead() {
        if(!B.isEmpty()) return B.removeLast();
        if(A.isEmpty()) return -1;
        while(!A.isEmpty())
            B.addLast(A.removeLast());
        return B.removeLast();
    }
}
```

**时间复杂度**： `appendTail()` 为 O\(1\) ；`deleteHead()` 在 N 次队首元素删除操作中总共需完成 N 个元素的倒序，虽然看起来是 O\(n\) 的时间复杂度，但是仔细考虑下每个元素只会「至多被插入和弹出 B 一次」，因此均摊下来每个元素被删除的时间复杂度仍为O\(1\)。 **空间复杂度**： O\(N\) 最差情况下，栈 A 和 B 共保存 N 个元素。

## **Summary**

* 活用栈**先进后出**的特性

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


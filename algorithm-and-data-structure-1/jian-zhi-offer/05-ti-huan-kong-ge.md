---
title: 剑指offer 05： 替换空格
date: '7/23/2020 10:00:00'
tags:
  - 剑指offer
categories:
  - 剑指offer
  - simple
---

# 05： 替换空格

## **Description** 

Difficulty: **Simple**

> 请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。
>
> 示例 ：
>
> 输入：s = "We are happy." 输出："We%20are%20happy."

## **Solution - 1：**  replace

**思路：** 如题，直接用string里现成的`replace(old value, new value)`内置函数即可

```java
class Solution {
  public String replaceSpace(String s) {
     return s.replace(" ", "%20");
  }
}
```

## **Solution - 2：**  字符数组

**思路：** 由于java无法在原来的String上进行修改，我们可以建立一个新字符数组进行替换，由于每次替换从 1 个字符变成 3 个字符，新字符数组的长度为 `s` 的长度的 3 倍，这样可保证字符数组可以容纳所有替换后的字符。

```java
class Solution {
    public String replaceSpace(String s) {
        int length = s.length();
        char[] array = new char[length * 3];
        int size = 0;
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            if (c == ' ') { //替换
                array[size++] = '%';
                array[size++] = '2';
                array[size++] = '0';
            } else {
                array[size++] = c;
            }
        }
        String newStr = new String(array, 0, size); //从array的前size个字符创建新字符串
        return newStr;
    }
}
```

```java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder res = new StringBuilder();
        // an easier way to get each character
        for(Character c : s.toCharArray())
        {
            res.append(c == ' ' ? "%20" : c); 
        }
        return res.toString();
    }
}
```

**Space complexity : O\(3\*n\)** 

**Time complexity : O\(n\)**

## **Summary** 

* 使用内置replace函数进行替换是正常人第一的想法，但是肯定不是面试官要的。

总之希望自己能坚持下去，每周记录分享几道有趣的题和解法。也欢迎大家留言讨论补充\(●'◡'●\)


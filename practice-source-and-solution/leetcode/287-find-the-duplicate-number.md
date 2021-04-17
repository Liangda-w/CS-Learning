---
title: Leetcode 287： Find the Duplicate Number
date: '6/25/2020 18:28:54'
tags:
  - Leetcode
  - Floyd's Tortoise and Hare
  - Binary Search
  - Complexity Analysis
categories:
  - Leetcode
  - Medium
---

# 287: Find the Duplicate Number

## \*\*\*\*[**Description**](https://leetcode.com/problems/find-the-duplicate-number/) 

Difficulty: **Medium**

> Given an array nums containing n + 1 integers where each integer is between 1 and n \(inclusive\), prove that at least one duplicate number must exist. Assume that there is **only one duplicate** number, find the duplicate one.
>
> **Example:** `Input: [1,3,4,2,2]` `Output: 2`
>
> **Note:** 1. You **must not modify** the array \(assume the array is read only\). 2. You must use only constant, **O\(1\)** extra space. 3. Your runtime complexity should be less than **O\(n^2\)**. 4. There is only one duplicate number in the array, but it could be repeated more than once.

Find the only one duplicate number in an array, challenge is to reduce the time complexity.

## **Solution - 1： Brute-force O\(n^2\)** 

**Thought：**既然是压低复杂度，第一个想法是找能直接用的寻找方式，类似 `nums.contains[id]` ，但是这种方法无法剔除本身查找的数字，所以结果一直是`true`，（可行的方法是把数放进数字结构Set里，但是这样空间复杂度会过高，见Solution - 5）。

暴力解：遍历每一个数字的时候在剩下的数字里找是否有和当前数字相同的数，如果有，直接return。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        for(int i=0; i<nums.length;i++){
            for(int j=i+1; j<nums.length; j++){ // start to search from the next number
                if(nums[i] == nums[j]){
                    return nums[i];
                }
            }
        }
        return 0; //dummy return
    }
}
```

* **Time complexity : O\(n^2\)** two for loops
* **Space complexity : O\(1\)**

Certainly not a good solution:

![](https://raw.githubusercontent.com/Liangda-w/images/master/Leetcode-287.png)

## **Solution - 2： Sorting O\(n log\(n\)\)** 

**Thought:** If the array is sorted from smallest to the biggest, then we only need to check whether two adjacent numbers are identical.

如果数列是按大小排列的，那么只需要确认相邻的两个数是否相同。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        Arrays.sort(nums); // sort the array
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i-1]) {
                return nums[i];
            }
        }
        return -1;
    }
}
```

* **Time complexity : O\(n log\(n\)\)** because of `sort`
* **Space complexity : O\(1\) or O\(n\)** If we can sort in place, it will be O\(1\), but because of the first limitation, here it should be O\(n\)

## **Solution - 3： Binary Search O\(n log\(n\)\)**  <a id="solution-3"></a>

**思路：** 评论区一开始看到这个代码有点一头雾水，这不是只有在数列是按顺序排列的时候才成立嘛？后来发现这题目里很重要一点一直被我忽略了：

> Given an array nums containing **n + 1** integers where each integer is **between 1 and n** \(inclusive\)

* 正常来说是有`1-n`个数，如果x是`1-n`里任意一个数，那么小于等于x的数的数量应该正好是x，即`1-x`。
* 现在因为有一个数重复，如果重复的数字小于等于x，那么小于等于x的数的数量就是`x+1`; 如果重复的数字大于x，那么小于等于x的数的数量就还是x。
* 因此我们可以一步步缩小范围来找到重复的数。这里虽然数字不是按大小排列的，但是它们的index是，所以范围left/right和中间值mid可以通过index来表示。这也是为什么Binary Search在这里行得通：

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int left = 1;
        int right = nums.length - 1; // n
        while (left <= right) { //!!!
            int mid = left + (right - left)/2;
            // how many numbers are smaller than mid
            int count = 0;
            for (int a : nums) {                
                if (a <= mid) ++count; 
            }
            if (count <= mid) left = mid + 1; // duplicate is bigger than mid
            else right = mid - 1; // duplicate is smaller than mid
        }
        return left; //!!!
    }
}
```

* **Time complexity : O\(n log\(n\)\)**
* **Space complexity : O\(1\)**

## **Solution - 4： Bit Manipulation O\(n \* 32\)** 

**Thought：** Really interesting solution from discussion, it uses the fact that `Integer` has 32 Bits to compare: 

评论区很有意思的方法，利用了Integer有32 Bits来进行对比：

* a：在整数1-n里，有多少数的第i个bit是1？
* b：在数列nums里，有多少数的第i个bit是1？
* 如果b &gt; a，那么结果res的第i个bit是1。

```java
public int findDuplicate(int[] nums) {
    int n = nums.length-1, res = 0;
    for (int p = 0; p < 32; ++p) {
        int bit = (1 << p), a = 0, b = 0;
        for (int i = 0; i <= n; ++i) {
            if (i > 0 && (i & bit) > 0) ++a;
            if ((nums[i] & bit) > 0) ++b;
        }
        if (b > a) res += bit;
    }
    return res;
}
```

* **Time complexity : O\(n\*32\)** however  nlog\(n\) &lt; 32, so this solution is not faster than the two before.
* **Space complexity : O\(1\)**

## **Solution - 5： Set O\(n\)** 

**Thought:** we can put the numbers in a Set for the easy search. Before we put each number, we first check whether this number already exists in the Set. If exists, return it.

我们可以把数放在一个数字结构Set里用于查找。在我们一个一个把数字加入前先在Set里查找是否已经有这个数存在，如果存在，可以直接return。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        Set<Integer> seen = new HashSet<Integer>();
        for (int num : nums) {
            if (seen.contains(num)) { // lookup num
                return num;
            }
            seen.add(num); // insert
        }
        return -1;
    }
}
```

* **Time complexity : O\(n\)** because of for loop 
  * Set uses hash table, therefore add and search \(contains\) an element requires only constant O\(1\)
* **Space complexity : O\(n\)** the worst case is that the first and the last number are the same. In this case `seen` contains O\(n-1\) numbers. \(Exceed the limitation\)

## **\*Solution - 6： Floyd's Tortoise and Hare \(Cycle Detection\) O\(n\)** 

**思路：** 我们使用函数**f\(x\) = nums\[x\]**来建立一个数列：x，nums\[x\]，nums\[nums\[x\]\]，nums\[nums\[nums\[x\]\]\]...即每一个数的位置是前一个数的值，如果我们从x =nums\[0\]开始，又因为nums里有一个重复的数字，这必将形成一个带有循环的数列，如下：

![](https://raw.githubusercontent.com/Liangda-w/images/master/Leetcode_287_simple_cycle.png)

更复杂的例子 / 更大的圈：

![](https://raw.githubusercontent.com/Liangda-w/images/master/Leetcode_287_complex_cycle.png)

我们不难看出，进入循环的点就是我们要找的重复的数字（即例子里的1和9），问题是如何找到它？

**Floyd's algorithm：**包含两个阶段和两个变量 `tortoise`（龟）和 `hare`（兔）。

**阶段1**：兔子的跳跃速度是乌龟爬行速度的两倍，即 `hare = nums[nums[hare]]`, `tortoise = nums[tortoise]`。因为兔子的速度快，它会比乌龟更先进入循环，都进入循环后，在一个点兔子会和乌龟相遇。第一阶段结束，兔子胜利。 以第一个例子为准他们将相遇于点1：

* 兔子的路线为：2，3，6，1
* 乌龟的路线为：2，4，3，1

以第二个例子为准他们将相遇于点7：

* 兔子的路线为：2，1，3，8，9，5，6，7
* 乌龟的路线为：2，9，1，5，3，6，8，7

![](https://raw.githubusercontent.com/Liangda-w/images/master/Leetcode%20287_first_intersection.png)

因为兔子的速度是乌龟的两倍，所用时间相同，兔子跑的距离也是乌龟的两倍：2d\(tortoise\) = d\(hare\)，设从起始点到进入循环点的距离为F，循环的长度为C，相遇点距离进入循环点的距离为a，则2\(F+a\) = F+nC+a，n为任意整数。因此我们可以得出F+a = nC。

**阶段2**：我们再给乌龟一次机会并把兔子的速度变成和乌龟一样，即`hare = nums[hare]`, `tortoise = nums[tortoise]`。乌龟从起始点重新开始，而兔子从阶段1的相遇点开始。

![](https://raw.githubusercontent.com/Liangda-w/images/master/Leetcode_287_phase2.png)

以第一个例子为准他们将相遇于进入循环点1：

* 兔子的路线为：2，4，3，1
* 乌龟的路线为：1，5，6，1

以第二个例子为准他们将相遇于进入循环点9：

* 兔子的路线为：2，9
* 乌龟的路线为：7，9

为什么会这样呢？乌龟在F步之后会到达点F，而由于F = nC - a，兔子在F步之后也必定到达点F，所以这次兔子和乌龟会在进入循环的点相遇（即我们要找的重复的数）。

```java
class Solution {
  public int findDuplicate(int[] nums) {
    // Phrase 1: Find the intersection point of the two runners.
    int tortoise = nums[0];
    int hare = nums[0];
    do {
      tortoise = nums[tortoise];
      hare = nums[nums[hare]];
    } while (tortoise != hare);

    // Phrase 2: Find the "entrance" to the cycle.
    tortoise = nums[0];
    while (tortoise != hare) {
      tortoise = nums[tortoise];
      hare = nums[hare];
    }
    return hare;
  }
}
```

* **Time complexity : O\(n\)** 
* **Space complexity : O\(1\)**

## **Application** 

At last, the application of this task in the reality: 

> Proving that at least one duplicate must exist in nums is simple application of the **pigeonhole principle**. Here, each number in nums is a "pigeon" and each distinct number that can appear in nums is a "pigeonhole". Because there are n+1 numbers and n distinct possible numbers, the pigeonhole principle implies that at least one of the numbers is duplicated.
>
> 桌上有十个苹果，要把这十个苹果放到九个抽屉里，无论怎样放，我们会发现至少会有一个抽屉里面放不少于两个苹果。这一现象就是我们所说的“抽屉原理”。 抽屉原理的一般含义为：“如果每个抽屉代表一个集合，每一个苹果就可以代表一个元素，假如有n+1个元素放到n个集合中去，其中必定有一个集合里至少有两个元素。” 抽屉原理有时也被称为鸽巢原理。

## **Summary** 

* This is not a hard task, but there're many interesting solutions with different complexity.
* The last algorithm is a hard one.

I'll keep on sharing some interesting tasks and solution each week, please feel free to leave a comment or pull request, if you have any questions or suggestions!\(●'◡'●\)


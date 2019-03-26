---
layout: post
title: The 2nd Week Of ARTS:2-AddTwoNumbers
date: 2018-07-02
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm

1. [Description](https://leetcode.com/problems/add-two-numbers/description/)
- You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.
- Example
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.

2. Solution

```java
package com.silence.arts.leetcode.first;

import java.util.LinkedList;

/**
 * <br>
 * <b>功能：</b><br>
 * <b>作者：</b>@author Silence<br>
 * <b>日期：</b>2018-07-01 12:11<br>
 * <b>详细说明：</b>无<br>
 */
public class AddTwoNumbers_2 {
    public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        StringBuilder add1 = new StringBuilder();
        ListNode p = l1;
        while (p.next != null) {
            add1.append(p.val);
            p = p.next;
        }
        add1.append(p.val);

        StringBuilder add2 = new StringBuilder();
        ListNode pp = l2;
        while (pp.next != null) {
            add2.append(pp.val);
            pp = pp.next;
        }
        add2.append(pp.val);
        String num1 = add1.toString();
        String num2 = add2.toString();
        if (num1.length() < num2.length()) {
            for (int i = 0; i < num2.length() - num1.length(); i++) {
                add1.append(0);
            }
            num1 = add1.toString();
        }

        if (num2.length() < num1.length()) {
            for (int i = 0; i < num1.length() - num2.length(); i++) {
                add2.append(0);
            }
            num2 = add2.toString();
        }

        StringBuilder builder = new StringBuilder();
        int carry = 0;
        for (int i = 0; i < num1.length(); i++) {
            for (int j = 0; j < num2.length(); j++) {
                if (i == j) {
                    int val = Integer.parseInt(String.valueOf(num1.charAt(i)));
                    int val2 = Integer.parseInt(String.valueOf(num2.charAt(j)));
                    int sum = val + val2 + carry;
                    if (sum < 10) {
                        builder.append(sum);
                        carry = 0;
                    } else {
                        builder.append(String.valueOf(sum).substring(1));
                        carry = 1;
                    }
                }
            }
        }

        if (carry > 0) {
            builder.append(carry);
        }
        String result = builder.toString();
        LinkedList<ListNode> list = new LinkedList<>();
        for (int i = 0; i < result.length(); i++) {
            int val = Integer.parseInt(String.valueOf(result.charAt(i)));
            ListNode listNode = new ListNode(val);
            list.add(listNode);
        }
        for (int i = 0; i < list.size(); i++) {
            if (i + 1 < list.size()) {
                list.get(i).next = list.get(i + 1);
            } else {
                list.get(i).next = null;
            }
        }
        return list.get(0);
    }

    public static void main(String[] args) {
        ListNode l1 = new ListNode(2);
        ListNode l2 = new ListNode(4);
        l1.next = l2;
        ListNode l3 = new ListNode(3);
        l2.next = l3;
        l3.next = null;


        ListNode l4 = new ListNode(5);
        ListNode l5 = new ListNode(6);
        l4.next = l5;
        ListNode l6 = new ListNode(4);
        l5.next = l6;
        l6.next = null;
        ListNode ll = addTwoNumbers(l1, l4);
        System.out.println(ll.val);

    }

}

class ListNode {
    int val;
    ListNode next;
    ListNode(int x) {
        this.val = x;
    }
}
```

> Thinking
> 1. Assemble the values inside the two ListNodes into a string
> 2. Compile the two strings that are assembled, make the length of the two strings is the same
> 3. Traversing the characters of the two strings one at a time and carry
> 4. Convert the result to ListNodes
> 5. The obtained ListNode is composed of a LinkedList, and the first ListNode is output.

> Conclusion<br>
> - The idea is very simple, but the implementation of this algorithm is not very efficient, only be said to solve the problem, the effect is not very good, the redundancy of code writing.

> 思路
> 1. 将输入的两个ListNode内部的值组装成字符串
> 2. 将组装得到的两个字符串补齐，让其位数一样
> 3. 遍历让两个字符串的字符一次累加，进位
> 4. 将得到的字符串转换为ListNode
> 5. 将得到的ListNode组成LinkedList，输出第一个ListNode

> 总结
> - 思路很简单，但是这个算法的实现并不是很优雅，只能说解决了问题，效果不是很好，代码写的冗余。

3. More Efficient Solution!!!😂😂😂

```java
 ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
```
### Review
This week read the article [the-power-of-doing-nothing-at-all](https://medium.com/swlh/the-power-of-doing-nothing-at-all-73eeea488b8b). Through the follower aspects author wants to tell us sometimes we should shut ourselves down.
- Doing what matters vs. busy-bragging.
- The extreme busyness epidemic.
- The power of doing nothing at all.
- On implementing “nothing” time.

The author started with a story about an old crocodile and young crocodile. Through the story tells us that we should take a week off every year so that we have time to release ourselves to give your brain space to think and doing nothing.


### Tips

1. public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
- Object src : 原数组
- int srcPos : 从原数组的起始位置开始
- Object dest : 目标数组
- int destPos : 目标数组的开始起始位置
- int length  : 要copy的数组的长度

2. for (Map.Entry<String, String> entry : subMap.entrySet()) {}
- 对于map的遍历有多种方式，此种方式效率最高

### Share
[Azkaban任务流](https://zxsilence.cn/2018/07/%E4%BB%BB%E5%8A%A1%E8%B0%83%E5%BA%A6Azkaban(%E4%BA%8C)%E4%BB%BB%E5%8A%A1%E6%B5%81/)


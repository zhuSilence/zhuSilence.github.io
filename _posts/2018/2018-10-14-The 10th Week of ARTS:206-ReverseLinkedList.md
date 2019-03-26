---
layout: post
title: The 10th Week of ARTS:206-ReverseLinkedList
date: 2018-10-14
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
#### Reverse Linked List
1. [Description](https://leetcode.com/problems/reverse-linked-list/description/)
- Reverse a singly linked list.
Example:
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL

2. My Solution

```java
package com.silence.arts.leetcode.second;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-10-14 11:23<br>
 * <b>Desc：</b>无<br>
 */
public class ReverseLinkedList_206 {

    /**
     * 创建一个新的临时头结点 myHeader
     * 遍历依次将数据插入 myHeader 后面，提前保存 myHeader 的 next 指针
     * 注意避免指针丢失！！！
     *
     * @param head
     * @return
     */
    public static ListNode reverseList(ListNode head) {
        ListNode myHeader = new ListNode(0);
        while (head != null) {
            ListNode tmp = myHeader.next;
            myHeader.next = head;
            head = head.next;
            myHeader.next.next = tmp;
        }
        return myHeader.next;
    }


    public static void main(String[] args) {
        ListNode ls1 = new ListNode(1);
        ListNode ls2 = new ListNode(2);
        ls1.next = ls2;
        ListNode ls3 = new ListNode(3);
        ls2.next = ls3;
        ListNode ls4 = new ListNode(4);
        ls3.next = ls4;
        ListNode ls5 = new ListNode(5);
        ls4.next = ls5;
        ls5.next = null;
        ListNode listNode = reverseList(ls1);
        while (listNode != null) {
            System.out.println(listNode.val);
            listNode = listNode.next;
        }
    }
}


class ListNode {
    int val;
    ListNode next;

    ListNode(int x) {
        val = x;
    }
}
```

### Review
#### [HowToBeAProgrammer-Beginner-Learn to Debug](https://github.com/braydie/HowToBeAProgrammer/blob/master/en/1-Beginner/Personal-Skills/01-Learn-To-Debug.md)


### Tips
NO Tips

### Share
#### Sharing
- [个人博客Google AdSense接入](https://zxsilence.cn/2018/09/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2Google-AdSense%E6%8E%A5%E5%85%A5/)
- I don't know why there are no ads on the website while you use a browser to read the essay. But if you read the essay on your phone it will be ads on there.

#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [https://zxsilence.cn/](https://zxsilence.cn/)
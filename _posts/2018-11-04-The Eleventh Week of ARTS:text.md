---
layout: post
title: The Eleventh Week of ARTS:Sort List_148
date: 2018-11-04
tag: 算法
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm

#### Sort List
1. [Description](https://leetcode.com/problems/sort-list/description/)
Sort a linked list in O(n log n) time using constant space complexity.
Example 1:
Input: 4->2->1->3
Output: 1->2->3->4
Example 2:
Input: -1->5->3->4->0
Output: -1->0->3->4->5

2. My Solution

```java
package com.silence.arts.leetcode.list;

import java.util.ArrayList;
import java.util.List;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-10-21 00:23<br>
 * <b>Desc：</b>无<br>
 */
public class SortList_148 {

    public static ListNode sortList(ListNode head) {
        List<ListNode> list = new ArrayList<>();
        while (head != null) {
            list.add(head);
            head = head.next;
        }
        quickSort2(list, 0, list.size() - 1);
        ListNode node = new ListNode(0);
        for (int i = list.size() - 1; i >= 0; i--) {
            ListNode tmp = node.next;
            node.next = list.get(i);
            node.next.next = tmp;
        }
        return node.next;
    }

    /**
     * 快速排序迭代
     *
     * @param list
     * @param left
     * @param right
     */
    public static void quickSort2(List<ListNode> list, int left, int right) {
        if (left < right) {
            int position = position(list, left, right);
            quickSort2(list, left, position - 1);
            quickSort2(list, position + 1, right);
        }

    }

    /**
     * 找到中间位置
     *
     * @param list
     * @param left
     * @param right
     * @return
     */
    public static int position(List<ListNode> list, int left, int right) {
        int key = list.get(left).val;
        while (left < right) {
            while (right > left && list.get(right).val >= key) {
                right--;
            }
            list.get(left).val = list.get(right).val;

            while (left < right && list.get(left).val <= key) {
                left++;
            }
            list.get(right).val = list.get(left).val;
        }
        list.get(left).val = key;
        return left;
    }


    public static void main(String[] args) {
        ListNode ls1 = new ListNode(7);
        ListNode ls2 = new ListNode(3);
        ls1.next = ls2;
        ListNode ls3 = new ListNode(3);
        ls2.next = ls3;
        ListNode ls4 = new ListNode(2);
        ls3.next = ls4;
        ListNode ls5 = new ListNode(5);
        ls4.next = ls5;
        ls5.next = null;
        ListNode listNode = sortList(ls1);
        while (listNode != null) {
            System.out.println(listNode.val);
            listNode = listNode.next;
        }
//        ListNode listNode = ls1;
//        while (ls1 != null) {
//            if (null == ls1.next) {
//                System.out.println(ls1.val);
//            }
//            ls1 = ls1.next;
//        }
    }

}


class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```
> Conclusion
> Using quickSort solves it.

### Review
#### [How (properly) wasting time at work increases productivity](https://medium.com/swlh/how-wasting-time-at-work-properly-increases-productivity-76272d651ef0)
- Give your prefrontal cortex a breather
- Get your blood flowing
- Linger by the water cooler
- Go green
- Grab a bite… or surf the web
- Keep it work-related

> - The author talks about wasting time at work increases productivity from above these ideas
> - The main idea is that you should take a break while you work a very long time.  You should breath fresh air, stand up for a while, talk a few minutes with colleagues, walk into the park, browser some pages on the internet or do something work-related.
> - Actually It's very useful. When I work very long time I usually take a break to drink something water or see some views out of window or close eyes for a while to think about some detail.

### Tips
No tips

### Share
#### Sharing
[常用算法笔记——持续更新](http://zxsilence.cn/2018/11/%E5%B8%B8%E7%94%A8%E7%AE%97%E6%B3%95%E7%AC%94%E8%AE%B0/)


#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [http://zxsilence.cn/](http://zxsilence.cn/)
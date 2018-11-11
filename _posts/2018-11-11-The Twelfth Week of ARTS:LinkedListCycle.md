---
layout: post
title: The Twelfth Week of ARTS:LinkedListCycle_141
date: 2018-11-11
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
#### Sort List
1. [Description](https://leetcode.com/problems/linked-list-cycle/)
Given a linked list, determine if it has a cycle in it.

2. My Solution

```java
package com.silence.arts.leetcode.list;



/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-10-21 23:50<br>
 * <b>Desc：</b>无<br>
 */
public class LinkedListCycle_141 {

    /**
     * tow points
     * thinking about two runner are running on the track
     * fast one will catch up the slow one if there is a cycle
     * @param head
     * @return
     */
    public static boolean hasCycle(ListNode head) {
        if (null == head || null == head.next) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (slow != fast) {
            if (null == fast || null == fast.next) {
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }

    public static void main(String[] args) {

    }
}

```

> tow points

### Review
#### [Why Do All Websites Look the Same?](https://medium.com/s/story/on-the-visual-weariness-of-the-web-8af1c969ce73)
As a visual designer, the author thinks that all web pages on the internet looked the same.
> Today’s internet is bland. Everything looks the same: generic fonts, no layouts to speak of, interchangeable pages, and an absence of expressive visual language. Even micro-typography is a mess.

### Tips
1. JS `indexOf()`
    1. indexOf('') == 0

2. Java 序列化
    1. 实现 Serializable 接口，自动进行序列化；static 变量不会序列化；
    2. 实现 Externalizable 接口，需要手动序列化；static 变量会序列化；

3. `LinkedList` 底层双向链表
    1. `add`时直接将最后一个节点赋值为`last.next = newNode; newNode.prev = last`；
    2. `get`时，根据 `index` 与 `size >> 2`，大小，决定从头遍历还是从尾部遍历；

4. `HashSet`
    1. 不允许存放重复 value 的集合，底层用`HashMap` 实现；
    2. `add` 时基于`map.put(key, PRESNET) == null`
    3. `map` 的 `put` 方法返回旧值或者 `null`
5. 升级`jekyll`
    1. `bundle update jekyll` or `bundle update`

### Share
#### Sharing
[常用算法笔记——持续更新](http://zxsilence.cn/2018/11/%E5%B8%B8%E7%94%A8%E7%AE%97%E6%B3%95%E7%AC%94%E8%AE%B0/)

#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [http://zxsilence.cn/](http://zxsilence.cn/)
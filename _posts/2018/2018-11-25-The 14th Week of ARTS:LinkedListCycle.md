---
layout: post
title: The 14th Week of ARTS:LinkedListCycle_142
date: 2018-11-25
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
#### Linked List Cycle II
1. [Description](https://leetcode.com/problems/linked-list-cycle-ii/)

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.
Note: Do not modify the linked list.

2. My Solution

```java
package com.silence.arts.leetcode.list;


import java.util.HashSet;
import java.util.Set;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-11-25 15:50<br>
 * <b>Desc：</b>无<br>
 */
public class LinkedListCycle2_142 {

    /**
     * Definition for singly-linked list.
     * class ListNode {
     *     int val;
     *     ListNode next;
     *     ListNode(int x) {
     *         val = x;
     *         next = null;
     *     }
     * }
     */
    public class Solution {
        public ListNode detectCycle(ListNode head) {
            if (null == head) {
                return null;
            }
            Set<ListNode> set = new HashSet<>();
            ListNode tmp = head;
            while (tmp.next != null) {
                if (set.contains(tmp)) {
                    return tmp;
                }else {
                    set.add(tmp);
                }
                tmp = tmp.next;
            }
            return null;
        }
    }

    public static void main(String[] args) {

    }
}


```

> Extra spaces keep ListNode

### Review
#### [How to Design for the Modern Web](https://medium.com/s/silicon-satire/how-to-design-for-the-modern-web-52eaa926bae2)
- Let Users Know About Your Mobile Application
- Let Users Know About Cookies
- Let Users Know They Can Sign Up
- Block Users From Europe（**Do not agree**）
- Allow Users to Opt Out
- Advertise Your Application
- Always Bet on JavaScript

### Tips
#### Java CAS(Compare And Swap)比较交换
1. 原理
    1. 底层基于`Unsafe.compareAndSwap`实现，这是个`native`方法，调用系统原语进行操作，原语属于操作系统用语范畴，是由若干条指令组成的，用于完成某个功能的一个过程，并且原语的执行必须是连续的，在执行过程中不允许被中断，也就是说CAS是一条CPU的原子指令，不会造成所谓的数据不一致问题。
2. 基本思想——比较交互，赋值前先获取旧值，比较旧值是不是预期的值，是则用新值替换，不是则循环获取
```java
    //Unsafe类中的getAndAddInt方法
    public final int getAndAddInt(Object o, long offset, int delta) {
        int v;
        do {
            v = getIntVolatile(o, offset);
        } while (!compareAndSwapInt(o, offset, v, v + delta));
    return v;
```
3. 应用有并发包中的原子操作类(Atomic系列)
    - AtomicBoolean：原子更新布尔类型
    - AtomicInteger：原子更新整型
    - AtomicLong：原子更新长整型

```
package com.silence.arts.study;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

/**
 * @author silence
 */
public class AtomicIntegerCounter {
    private AtomicInteger ai = new AtomicInteger();
    private int i = 0;

    public static void main(String[] args) {
        final AtomicIntegerCounter cas = new AtomicIntegerCounter();
        List<Thread> ts = new ArrayList<Thread>();
        // 添加100个线程
        for (int j = 0; j < 100; j++) {
            ts.add(new Thread(() -> {
                // 执行100次计算，预期结果应该是10000
                for (int i = 0; i < 10000; i++) {
                    cas.count();
                    cas.safeCount();
                }
            }));
        }
        //开始执行
        for (Thread t : ts) {
            t.start();
        }
        // 等待所有线程执行完成
        for (Thread t : ts) {
            try {
                t.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("非线程安全计数结果：" + cas.i);
        System.out.println("线程安全计数结果：" + cas.ai.get());
    }

    /**
     * 使用CAS实现线程安全计数器
     */
    private void safeCount() {
        for (; ; ) {
            int i = ai.get();
            // 如果当前值 == 预期值，则以原子方式将该值设置为给定的更新值
            boolean suc = ai.compareAndSet(i, ++i);
            if (suc) {
                break;
            }
        }
    }

    /**
     * 非线程安全计数器
     */
    private void count() {
        i++;
    }
}
```


### Share
#### Sharing
[常用算法笔记——持续更新](http://zxsilence.cn/2018/11/%E5%B8%B8%E7%94%A8%E7%AE%97%E6%B3%95%E7%AC%94%E8%AE%B0/)

#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [http://zxsilence.cn/](http://zxsilence.cn/)
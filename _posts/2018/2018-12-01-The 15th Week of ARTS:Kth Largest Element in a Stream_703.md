---
layout: post
title: The 15th Week of ARTS:KthLargest Element in a Stream_703
date: 2018-12-01
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
#### KthLargest Element in a Stream
1. [Description](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

Design a class to find the kth largest element in a stream. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Your KthLargest class will have a constructor which accepts an integer k and an integer array nums, which contains initial elements from the stream. For each call to the method KthLargest.add, return the element representing the kth largest element in the stream.

2. My Solution

```java
package com.silence.arts.leetcode.stack;

import java.util.PriorityQueue;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-12-01 16:04<br>
 * <b>Desc：</b>无<br>
 */
public class KthLargest_703 {

    /**
     * PriorityQueue is kind of MINI Heap structure
     */
    private PriorityQueue<Integer> queue;
    private int k;

    public KthLargest_703(int k, int[] nums) {
        this.queue = new PriorityQueue<>(k);
        this.k = k;
        for (int num : nums) {
            add(num);
        }
    }

    /**
     * If the size < k that means we just need to add the element then maintain the Heap
     * else if the peek() < val we need to poll() the head of Heap and add the element then maintain the Heap
     * if the peek() > val we do nothing
     * return the peek()
     *
     * @param val
     * @return
     */
    public int add(int val) {
        if (queue.size() < k) {
            queue.offer(val);
        } else if (queue.peek() < val) {
            queue.poll();
            queue.offer(val);
        }
        return queue.peek();
    }
}


```

> Extra spaces keep ListNode

### Review
#### [I just got a developer job at Facebook. Here’s how I prepped for my interviews.](https://medium.freecodecamp.org/software-engineering-interviews-744380f4f2af)
##### Main ideas
- Algorithm Interviews
- Architecture Design Interviews
- Behavioral Interviews
- Culture Fit
- Pair Programming
- Finding and Patching Bugs
- Testing Domain Knowledge
- Understanding Operating Systems

##### Resources
1. Mock Interviews
- [interviewing.io (beta), Free](https://interviewing.io/)
- [Pramp, Free](https://www.pramp.com/#/)
- [CareerCup, Paid](https://careercup.com/interview)
2. Algorithms
- [Cracking the Code Interview, Book](https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/0984782850/ref=sr_1_1?s=books&ie=UTF8&qid=1506042558&sr=1-1&keywords=cracking+the+code+interview)
- [byte by byte, Website and YouTube](https://www.byte-by-byte.com/)
- [CS50, YouTube](https://www.youtube.com/user/cs50tv)
- [Interview Cake, Website](https://www.interviewcake.com/)
- [HackerRank, Website](https://www.hackerrank.com/)
- [LeetCode, Website](https://leetcode.com/)
3. Operating Systems
- [Operating System Concepts, Book](https://www.amazon.com/Operating-System-Concepts-Abraham-Silberschatz/dp/1118063333/ref=sr_1_1?s=books&ie=UTF8&qid=1506042402&sr=1-1&keywords=Operating+System+Concepts)
4. Architecture Design
- [Intro to Architecture and Systems, YouTube](https://www.youtube.com/watch?v=ZgdS0EUmn70)
5. Behavioural
- [Intro to Behavioural Interviews, YouTube](https://www.youtube.com/watch?v=PJKYqLP6MRE)


### Tips
#### Java CAS(Compare And Swap)比较交换
1. MySql数据库的 InnoDB 引擎的索引采用 B+树存储，之所以采用 B+树是为了减少磁盘的访问，提供查询效率
2. 主键索引保存的是整行数据，普通索引保存的是主键，索引普通索引的查询比主键搜索的查询多一次树的搜索


### Share
#### Sharing
[常用算法笔记——持续更新](https://zxsilence.cn/2018/11/%E5%B8%B8%E7%94%A8%E7%AE%97%E6%B3%95%E7%AC%94%E8%AE%B0/)

#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [https://zxsilence.cn/](https://zxsilence.cn/)
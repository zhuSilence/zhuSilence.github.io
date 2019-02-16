---
layout: post
title: The 16th Week of ARTS:Sliding Window Maximum_239
date: 2018-12-09
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
#### Sliding Window Maximum
1. [Description](https://leetcode.com/problems/sliding-window-maximum/)

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

2. My Solution

```java
package com.silence.arts.leetcode.stack;

import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;
import java.util.List;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2019-02-16 11:17<br>
 * <b>Desc：</b>无<br>
 */
public class SlidingWindowMaximum_239 {

    public static void main(String[] args) {
        int[] nums = new int[]{1, 3, -1, -3, 5, 3, 6, 7};

        int[] ints = maxSlidingWindow(nums, 3);
        for (int i = 0; i < ints.length; i++) {
            System.out.println(ints[i]);
        }
    }

    public static int[] maxSlidingWindow(int[] nums, int k) {
        if (null == nums || nums.length <= 0) return nums;
        List<Integer> result = new ArrayList<>(nums.length - k + 1);
        Deque<Integer> window = new ArrayDeque<>(k);
        for (int i = 0; i < nums.length; i++) {
            if (window.size() < k) {
                window.add(nums[i]);
            } else if(window.size() == k) {
                result.add(getQueueMax(window));
                window.pop();
                i--;
            }
        }
        if(window.size() == k) {
            result.add(getQueueMax(window));
        }
        int[] a = new int[result.size()];
        for (int i = 0; i < a.length; i++) {
            a[i] = result.get(i);
        }
        return a;
    }

    private static int getQueueMax(Deque<Integer> deque) {
        final int[] max = {deque.getFirst()};
        deque.forEach(d -> {
            if (d > max[0]) {
                max[0] = d;
            }
        });
        return max[0];
    }

}


```


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
[常用算法笔记——持续更新](http://zxsilence.cn/2018/11/%E5%B8%B8%E7%94%A8%E7%AE%97%E6%B3%95%E7%AC%94%E8%AE%B0/)

#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [http://zxsilence.cn/](http://zxsilence.cn/)
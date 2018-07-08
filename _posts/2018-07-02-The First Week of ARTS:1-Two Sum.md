---
layout: post
title: The First Week of ARTS:1-Two Sum
date: 2018-07-02
tag: ARTS
---

### Introduction
1. The tag `ARTS` is a project that launched by @Hao Chen at his WeiChat group which for discussing technologies in his Articles.

2. The meaning of `ARTS`:
   - A: Algorithm, solve an Algorithm each week at https://leetcode.com.
   - R: Review, review something important or meaningful for you in last week.
   - T: Technique, improve one or two Techniques that discovered in your daily work.
   - S: Share, share something you like or interested or something special for you. Not only technologies but also life or inspiration.


> The ARTS dosen't force you to use English to write, but it good for you to write articles in English.

## Let's do it!!!

### Algorithm: Two Sum
1. The description of Two Sum. https://leetcode.com/problems/two-sum/description/

> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
> You may assume that each input would have exactly one solution, and you may not use the same element twice.
> Example:
> Given nums = \[2, 7, 11, 15\], target = 9,
> Because nums\[0\] + nums\[1\] = 2 + 7 = 9,
> return \[0, 1\].

2. Solution

```java

package com.silence.leetcode.first;

import java.util.HashMap;
import java.util.Map;

/**
 * <br>
 * <b>功能：</b><br>
 * <b>作者：</b>@author Silence<br>
 * <b>日期：</b>2018-07-01 11:15<br>
 * <b>详细说明：</b>solve the problem two sum...<br>
 */
public class TwoSum {

    /**
     * solution 1, For the first time, just think about solve it.
     *
     * @param nums
     * @param target
     * @return
     */
    public int[] twoSum1(int[] nums, int target) {
        int[] ints = new int[2];
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < nums.length; j++) {
                if (nums[i] + nums[j] == target && i != j) {
                    ints[0] = i;
                    ints[1] = j;
                    break;
                }
            }
        }
        return ints;
    }


    /**
     * Reference other's solution, it's more beautiful, just copy down and record it.
     *
     * @param nums
     * @param target
     * @return
     */
    public int[] twoSum2(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }


    public static void main(String[] args) {
        TwoSum twoSum = new TwoSum();
        int[] tem = new int[]{3, 3};
        int[] result = twoSum.twoSum2(tem, 6);
        for (int i = 0; i < result.length; i++) {
            System.out.println(result[i]);
        }
    }

}

```
> It's an easy problem, but there is always a **efficient** solution.

### Review
Last week, we published a new version of our project. We used Distributed lock in our project.
But we found the performance of services became decrease when we finished.
So what's wrong with is??
After we discussing with each other, we found the problem, because we have 10 threads for each instance and we have 10 instances. Therefore, there was 100
threads to try to get the lock, but only one can get the lock, so others must be waiting. They can't consume the message While they are waiting.
Then we changed the strategy, adjust one thread for an instance. The problem was solved perfectly.

### Technique
For the first time, I didn't have any Technique to share.
1. Just put my personal website here:
    [personal website](http://zxsilence.me/)

2. The leetCode code is here:
    [ARTS Code](https://github.com/zhuSilence/ARTS)


### Share
1. Azkaban
    - Last week did a survey about Azkaban a distributed task dispatch system.
    - Azkaban was implemented at LinkedIn to solve the problem of Hadoop job dependencies. We had jobs that needed to run in order, from ETL jobs to data analytics products.
    -  [Azkaban](http://zxsilence.me/2018/06/%E4%BB%BB%E5%8A%A1%E8%B0%83%E5%BA%A6Azkaban/)

2. Md2All
    - A online website to style your Md.
    - [Md2All](http://md.aclickall.com/)

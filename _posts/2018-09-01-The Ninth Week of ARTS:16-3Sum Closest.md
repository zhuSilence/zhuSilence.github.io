---
layout: post
title: The Ninth Week of ARTS:16-3Sum Closest
date: 2018-09-01
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
#### 3Sum Closest
1. [Description](https://leetcode.com/problems/3sum-closest/description/)
- Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.
Example:
Given array nums = [-1, 2, 1, -4], and target = 1.
The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
2. My Solution

```java
package com.silence.arts.leetcode.second;

import java.util.Arrays;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-09-01 22:41<br>
 * <b>Desc：</b>无<br>
 */
public class SumClosest_16 {

    /**
     * 计算两数之间的最短距离
     * j和k的起始值需要变更，不能从0开始，可以减少算法时间复杂度
     * <p>
     * calculate the distance of two numbers, record the min-distance
     * careful the beginning of j and k can not be 0, it can reduce the Time complexity
     *
     * @param nums   input int array
     * @param target input
     * @return output
     */
    private static int threeSumClosest(int[] nums, int target) {

        long start = System.currentTimeMillis();
        Arrays.sort(nums);
        int min = Integer.MAX_VALUE;
        int minDistance = Integer.MAX_VALUE;
        int sum;
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    sum = nums[i] + nums[j] + nums[k];
                    if (Math.abs(sum - target) < minDistance) {
                        minDistance = Math.abs(sum - target);
                        min = sum;
                    }
                }
            }
        }
        System.out.println(System.currentTimeMillis() - start);
        return min;
    }

    public static void main(String[] args) {

        int[] nums = new int[]{-1, 2, 1, -4};
//        int[] nums = new int[]{1, 1, 1, 0};
//        System.out.println(Math.abs(-1));
        System.out.println(threeSumClosest(nums, 1));
    }
}


```

### Review
#### [The 3 Keys to Becoming Irresistible](https://medium.com/personal-growth/the-3-keys-to-becoming-irresistible-d2f689ea4bf1)
- Humility
- Curiosity
- Empathy

### Tips
- JS获取对象长度 `Object.keys(obj).length`
- `proxy_connect_timeout`：后端服务器连接的超时时间，发起握手等候响应超时时间
- `proxy_read_timeout`：连接成功后等候后端服务器响应时间，其实已经进入后端的排队之中等候处理（也可以说是后端服务器处理请求的时间）
- `proxy_send_timeout`：后端服务器数据回传时间，就是在规定时间之内后端服务器必须传完所有的数据

### Share
#### Sharing
[记一次线上Nginx响应超时问题](https://zxsilence.cn/2018/08/%E8%AE%B0%E4%B8%80%E6%AC%A1%E7%BA%BF%E4%B8%8ANginx%E5%93%8D%E5%BA%94%E8%B6%85%E6%97%B6%E9%97%AE%E9%A2%98/)

#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [https://zxsilence.cn/](https://zxsilence.cn/)
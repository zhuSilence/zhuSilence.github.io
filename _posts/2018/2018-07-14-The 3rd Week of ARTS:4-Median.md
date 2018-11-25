---
layout: post
title: The 3rd Week of ARTS:4-Median of Two Sorted Arrays
date: 2018-07-14
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm

1. [Description](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)
- There are two sorted arrays nums1 and nums2 of size m and n respectively.
- Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

    Example 1:
    nums1 = [1, 3]
    nums2 = [2]

    The median is 2.0
    Example 2:
    nums1 = [1, 2]
    nums2 = [3, 4]

    The median is (2 + 3)/2 = 2.5


2. Solution

```java
package com.silence.arts.leetcode.first;

import java.util.Arrays;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-07-02 22:23<br>
 * <b>Desc：</b>无<br>
 */
public class Median_4 {

    /**
     * T System.arraycopy(nums1, 0, sum, 0, nums1.length);
     * @param nums1
     * @param nums2
     * @return
     */
    public static double findMedianSortedArrays(int[] nums1, int[] nums2) {
        double median;
        int[] sum = new int[nums1.length + nums2.length];
        System.arraycopy(nums1, 0, sum, 0, nums1.length);
        System.arraycopy(nums2, 0, sum, nums1.length, nums2.length);

        Arrays.sort(sum);
        //偶数
        if (sum.length % 2 == 0) {
            int a = sum.length / 2 - 1;
            int b = a + 1;
            median = (sum[a] + sum[b]) / 2.0;
            //奇数
        } else {
            median = sum[sum.length / 2.0];
        }
        return median;
    }


    public static void main(String[] args) {
//        int[] a = new int[]{1,9,9,7};
        int[] a = new int[]{};
        int[] b = new int[]{2, 3};
//        int[] b = new int[]{5,3,4,9};
        System.out.println(findMedianSortedArrays(a, b));
    }
}

```

> Thinking
> 1. combine the two arrays as a new array
> 2. sort the new array
> 3. according to the length of the new array decides how to calculate the median
> 4. if the even length the median is (sum[a] + sum[b]) / 2.0
> 5. if the odd length the median is sum[array.length / 2.0]


> 思路
> 1. 将输入的两个整型数组组合成一个新的数组
> 2. 将新的数组进行排序
> 3. 根据新数组的长度进行中位数的计算
> 4. 如果新数组的长度是偶数，则中位数计算方式为(sum[a] + sum[b]) / 2.0
> 5. 如果新数组的长度是奇数，则中位数计算方式为sum[array.lenth / 2.0]


### Review
#### [What motivates more: positive or negative feedback?](https://medium.com/swlh/what-motivates-more-positive-or-negative-feedback-9364133bd58b)
This week I am going to talk about the article **What motivates more: positive or negative feedback?**
- Motivation is hard to summon and harder to keep alive.
> &emsp; Often, a sustained blast of it is all we need to kickstart a new career or complete a personal goal. And if we could harness it every day — well, we’d be unstoppable.
> &emsp; Unfortunately, it’s rarely that simple. Motivation is hard to summon and harder to keep alive. As a resource, it’s as finite as they come (see also: its ever-flakey brother, inspiration).

- It's also positive where there is a negative feedback.
> &emsp; Negative feedback also can be positive although it's always feeling painful.
> &emsp;[The Positive Power of Negative Thinking](https://www.amazon.com/Positive-Power-Negative-Thinking/dp/0465051391/)

- Regular, scheduled feedback motivates more
> &emsp; If skillful feedback is about paying attention, then it follows that giving it in small doses, focused on short time spans, is going to be more effective.

### Tips
1. Linux统计日志文件的行数
- `sed -n '$=' test.log`
2. 过滤指定列字段到新文件
- `cat test.log |awk  -F '&'  '{print $7}' > mac.log`
- awk -F 指定分隔符，$7 打印出指定列
3. 排序合并，按统计数据排序
- `sort mac.log |uniq -c |sort -rn`

4. vim和shell切换
- 在 Vi/Vim的命令模式下输入 :sh即可进入 Linux/Unix shell 环境。在要返回到 Vi/Vim 编辑环境时，输入 exit 命令即可。

### Share
前段时间项目中使用Consul作为服务器注册和发现的使用，结合Consul-template组件实现服务上下线后，nginx配置文件自动更新的功能。
具体实现见下文。
[Consul-template，nginx实现负载均衡和服务列表自动更新](http://zxsilence.cn/2018/04/Consul-template,-Nginx%E5%AE%9E%E7%8E%B0Thrift-Consul%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1/)


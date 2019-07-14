---
layout: post
title: 2019-02-02-The 17th of ARTS:Valid Anagram_242
date: 2019-02-02
tag: ARTS
---


### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
#### Valid Anagram
1. [Description](https://leetcode.com/problems/valid-anagram/)

Given two strings s and t , write a function to determine if t is an anagram of s.

2. My Solution

```java

package com.silence.arts.leetcode.map;

import java.util.HashMap;
import java.util.Map;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2019-02-02 21:37<br>
 * <b>Desc：</b>无<br>
 */
public class ValidAnagram_242 {

    public static boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        Map<String, Integer> sMap = new HashMap<>(32);
        Map<String, Integer> tMap = new HashMap<>(32);
        sToMap(s, sMap);

        sToMap(t, tMap);

        return sMap.equals(tMap);
    }

    private static void sToMap(String t, Map<String, Integer> sMap) {
        for (int i = 0; i < t.length(); i++) {
            String tmp = String.valueOf(t.charAt(i));
            if (!sMap.containsKey(tmp)) {
                sMap.put(tmp, 1);
            } else {
                sMap.put(tmp, sMap.get(tmp) + 1);
            }
        }
    }


    public static void main(String[] args) {

        String s1 = "anagram";
        String s2 = "anagram";
        System.out.println(isAnagram(s1, s2));
    }
}


```

>

### Review
#### [I just got a developer job at Facebook. Here’s how I prepped for my interviews.](https://medium.freecodecamp.org/software-engineering-interviews-744380f4f2af)



### Tips
#### Java CAS(Compare And Swap)比较交换


### Share
#### Sharing
[常用算法笔记——持续更新](https://zxsilence.cn/2018/11/%E5%B8%B8%E7%94%A8%E7%AE%97%E6%B3%95%E7%AC%94%E8%AE%B0/)

#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [https://zxsilence.cn/](https://zxsilence.cn/)
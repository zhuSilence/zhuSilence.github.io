---
layout: post
title: The 13th Week of ARTS:Valid Parentheses_20
date: 2018-11-18
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
1. [Description](https://leetcode.com/problems/valid-parentheses/)

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.

Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

2. My Solution

```java
package com.silence.arts.leetcode.list;


import java.util.*;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-11-25 16:49<br>
 * <b>Desc：</b>无<br>
 */
public class ValidParentheses_20 {

    /**
     *
     * @param s
     * @return
     */
    public static boolean isValid(String s) {
        if (null != s && s.length() > 0) {
            Map<String, String> map = new HashMap<>();
            map.put(")", "(");
            map.put("]", "[");
            map.put("}", "{");
            Stack<String> stack = new Stack<>();
            for (int i = 0; i < s.length(); i++) {
                String val = String.valueOf(s.charAt(i));
                //if (!map.containsKey(val)) {
                if (val.equals("(") || val.equals("[") || val.equals("{")) {
                    stack.push(val);
                } else if (stack.size() <= 0 || !map.get(val).equals(stack.pop())) {
                    return false;
                }
            }
            //stack needs empty
            return stack.size() <= 0;
        }
        return true;
    }

    public static void main(String[] args) {
        System.out.println(isValid(""));
    }
}


```

> tow points

### Review
No Review

### Tips
![MySQL实战](/images/posts/articles/2018-11-15/MySQL实战.png)

### Share
#### Sharing
[常用算法笔记——持续更新](http://zxsilence.cn/2018/11/%E5%B8%B8%E7%94%A8%E7%AE%97%E6%B3%95%E7%AC%94%E8%AE%B0/)

#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [http://zxsilence.cn/](http://zxsilence.cn/)
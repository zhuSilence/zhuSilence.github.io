---
layout: post
title: The 6th Week of ARTS:7-ReverseInteger And 9-PalindromeNumber
date: 2018-08-05
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
#### ReverseInteger

1. [Description](https://leetcode.com/problems/reverse-integer/description/)
> Given a 32-bit signed integer, reverse digits of an integer.
Example 1:
Input: 123
Output: 321
Example 2:
Input: -123
Output: -321
Example 3:
Input: 120
Output: 21


2. My Solution

```java
package com.silence.arts.leetcode.first;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-07-18 23:52<br>
 * <b>Desc：</b>无<br>
 */
public class ReverseInteger_7 {

    /**
     * 判断数字是否是负数，负数则截取第一位符号位
     * @param x
     * @return
     */
    public static int reverse(int x) {
        int resultNext;
        try {
            if (x < 0) {
                String string = String.valueOf(x);
                String prev = string.substring(0, 1);
                StringBuilder next = new StringBuilder(string.substring(1));
                resultNext = Integer.parseInt(prev + Integer.parseInt(next.reverse().toString()));
            }else {
                StringBuilder string = new StringBuilder(String.valueOf(x));
                resultNext = Integer.parseInt(string.reverse().toString());
            }
        }catch (Exception e) {
            resultNext = 0;
        }
        return resultNext;
    }

    public static void main(String[] args) {
        System.out.println(reverse(1534236469));
    }
}

```
> Thinking
> 1. Transfer int to String
> 2. Reverse the String and Splice minus sign if it has

---
> 思路
> 1. 将整型转换为字符串
> 2. 反转字符串并且组装符号位（如果为负数的话）


#### PalindromeNumber

1. [Description](https://leetcode.com/problems/palindrome-number/description/)
>Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.
Example 1:
Input: 121
Output: true
Example 2:
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
Example 3:
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

2. My Solution

```java
package com.silence.arts.leetcode.first;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-07-31 23:38<br>
 * <b>Desc：</b>无<br>
 */
public class PalindromeNumber_9 {

    /**
     * 转换为字符串，然后反转进行比较
     * 注意before.reverse() 会将before改变，不会生成新的内存对象
     *
     * @param x
     * @return
     */
    public static boolean isPalindrome(int x) {
        String b = String.valueOf(x);
        StringBuilder before = new StringBuilder(b);
        String after = before.reverse().toString();
        return b.equals(after);
    }


    public static void main(String[] args) {
        System.out.println(isPalindrome(-121));
    }
}

```
> Thinking
> 1. Transfer int to String;
> 2. Reverse the String and then compare it with original String.

---
> 思路
> 1. 将数字转换成字符串
> 2. 反转字符串与反转前的字符串进行比较

### Review
#### [10 Common Software Architectural Patterns in a nutshell](https://towardsdatascience.com/10-common-software-architectural-patterns-in-a-nutshell-a0b47a1e9013)

This author explains 10 different kinds of Architectural Patterns briefly. Here are they.
1. Layered pattern
2. Client-server pattern
3. Master-slave pattern
4. Pipe-filter pattern
5. Broker pattern
6. Peer-to-peer pattern
7. Event-bus pattern
8. Model-view-controller pattern
9. Blackboard pattern
10. Interpreter pattern
![Alt text](/images/posts/articles/2018-08-05/0.png)


### Tips

1. jQuery prop()
``` javascript
//this code is not working you should use below code.
//$('#abc').attr('checked', true);
//It's working!
$('#abc').prop('checked', true);
```

2. [GITHUBER - 开发者的新标签页](https://chrome.google.com/webstore/search/GITHUBER%20-%20%E5%BC%80%E5%8F%91%E8%80%85%E7%9A%84%E6%96%B0%E6%A0%87%E7%AD%BE%E9%A1%B5?hl=en-US)
- This is an extension of Chrome that will help you to find the good project on Github weekly. Something like this.
![Alt text](/images/posts/articles/2018-08-05/1.png)

3. [Process On](https://www.processon.com/i/56fdcf73e4b0fbb6c9630913)
- This is a very good Free online drawing website. It also can be a real-time collaboration website. You can use it for yourself also you can use it with your friends together.
![Alt text](/images/posts/articles/2018-08-05/2.png)

### Share
#### Sharing
Last week was a busy week. I didn't do anything extra but work. Nothing else to share.
#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [http://zxsilence.cn/](http://zxsilence.cn/)
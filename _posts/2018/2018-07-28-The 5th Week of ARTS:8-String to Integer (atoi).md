---
layout: post
title: The 5th Week of ARTS:8-String to Integer (atoi)
date: 2018-07-28
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
1. [Description](https://leetcode.com/problems/string-to-integer-atoi/description/)
- Implement atoi which converts a string to an integer.
- The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.
- The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.
- If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.
- If no valid conversion could be performed, a zero value is returned.

2. My Solution

```java
package com.silence.arts.leetcode.first;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-07-19 23:13<br>
 * <b>Desc：</b>无<br>
 */
public class StringToInteger_8 {
    /**
     * ASCII码
     * 43 --> +
     * 45 --> -
     * 48 --> 0
     * 57 --> 9
     * @param str
     * @return
     */
    public static int myAtoi(String str) {
        str = str.trim();
        if (str.length() > 0) {
            char first = str.charAt(0);
            StringBuilder builder = new StringBuilder();
            if ((first == 43) || (first == 45)) {
                if (first == 45) {
                    builder.append(first);
                }
                str = str.substring(1);
            }
            boolean flag = true;
            for (int i = 0; i < str.length(); i++) {
                if (str.charAt(i) >= 48 && str.charAt(i) <= 57) {
                    builder.append(str.charAt(i));
                    flag = false;
                } else {
                    break;
                }
            }
            if (flag) {
                return 0;
            }
            try {
                if (first != 45) {
                    if (Long.valueOf(builder.toString()).compareTo((long) Integer.MAX_VALUE) > 0) {
                        return Integer.MAX_VALUE;
                    }
                }else {
                    if (Long.valueOf(builder.toString()).compareTo((long) Integer.MIN_VALUE) < 0) {
                        return Integer.MIN_VALUE;
                    }
                }
            } catch (Exception e) {
                if (first != 45) {
                    return Integer.MAX_VALUE;
                }else {
                    return Integer.MIN_VALUE;
                }
            }
            return Integer.valueOf(builder.toString());
        }
        return 0;
    }

    public static void main(String[] args) {
        System.out.println(myAtoi("+9223372036854775809"));
    }
}

```

> Thinking
> 1. According to ASCII checking the first Character if it equals '-' or '+';
> 2. Traversing the rest Characters of string, if it between 0-9 then added to append;
> 3. Cast string to Integer and compare it to Integer.MAX_VALUE and Integer.MIN_VALUE

---
> 思路
> 1. 检查第一位字符，根据ASCII码判断是否是符号
> 2. 遍历剩下的字符串，如果在0-9之间就append
> 3. 强转比较，如果异常则根据第一位字符进行最大值或者最小值输出

### Review
#### [Why should you learn Go?](https://medium.com/exploring-code/why-should-you-learn-go-f607681fad65)
- Hardware limitations
> So, if we cannot rely on the hardware improvements, the only way to go is more efficient software to increase the performance. But sadly, modern programming languages are not much efficient.

- Go has goroutines !!
- Go runs directly on underlying hardware.
- Code written in Go is easy to maintain.
- Go is backed by Google.

According to the Author, told us about the limitations of the hardware. Because of the limitations of the hardware, we need more efficient programming languages to develop our software. Go, which is the exact language that we expected. Go is almost as efficient as C/C++, while keeping the code syntax simple as Ruby, Python, and other languages.

So, I want to learn Go in my free time.

### Tips
#### Set website to Https.
1. Nginx needs to compiled with module `--with-http_ssl_module`;
2. You need a Certificate, you can get it from Tencent Cloud for one year for free;
3. Add following setting to nginx.conf
```powershell
server {
        listen       443 ssl;
        server_name  www.zxsilence.cn;

    #ssl on;
        ssl_certificate      your-Certificate.crt;
        ssl_certificate_key  your-Certificate.key;
        ssl_session_timeout  5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;
        location / {
            proxy_pass http://xxx;
        }
    }
```
> **Pay Attention**
> After that you need to restart nginx. you need `nginx -s stop` and `nginx`,
> Remember that `nginx -s reload` does not work!!!

### Share
This week I decide to publish my ARTS to medium.com. Here are my ARTS list:
- [The First Week Of ARTS:1-Two Sum](https://medium.com/@zhuxiang134/the-first-week-of-arts-1-two-sum-edd5dfb4244e)
- [The Second Week Of ARTS:2-AddTwoNumbers](https://medium.com/@zhuxiang134/the-second-week-of-arts-2-addtwonumbers-f173dcec094b)
- [The Third Week of ARTS:4-Median of Two Sorted Arrays](https://medium.com/@zhuxiang134/the-third-week-of-arts-4-median-of-two-sorted-arrays-2d47cc24c639)
- [The Fourth Week of ARTS:5-Longest Palindromic Substring](https://medium.com/@zhuxiang134/the-fourth-week-of-arts-5-longest-palindromic-substring-2eec41cf8c24)
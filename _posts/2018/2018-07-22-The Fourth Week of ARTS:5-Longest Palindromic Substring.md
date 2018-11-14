---
layout: post
title: The Fourth Week of ARTS:5-Longest Palindromic Substring
date: 2018-07-22
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
1. [Description](https://leetcode.com/problems/longest-palindromic-substring/description/)
- Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
> Example 1:
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
> Example 2:
Input: "cbbd"
Output: "bb"

2. Solution Brute Force

```java

package com.silence.arts.leetcode.first;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-07-12 22:50<br>
 * <b>Desc：</b>无<br>
 */
public class LongestPalindrome_5 {

    /**
     * Brute Force
     * 1. 找出所有的子串依次遍历判断是否是回文字符串
     * 2. 如果是回文串，并且长度比len大，则更新结果
     *
     * 1. find all substrings and find the longest Palindromic substring
     * 2. while k == end (odd numbers) or k + 1 == end (even numbers) means the substring is Palindromic substring
      *
     * @param s
     * @return
     */
    public static String longestPalindrome(String s) {
        String result = s.substring(0, 1);
        int len = 0;
        long start = System.currentTimeMillis();
        for (int i = 0; i < s.length(); i++) {
            for (int j = i + 1; j <= s.length(); j++) {
                String substring = s.substring(i, j);
                for (int k = 0; k < substring.length(); k++) {
                    int end = substring.length() - k - 1;
                    if (substring.length() >= len && substring.charAt(k) == substring.charAt(end)) {
                        if (((k == end) || (k + 1 == end))) {
                            if (substring.length() > len) {
                                len = Math.max(len, substring.length());
                                result = substring;
                            }
                        }
                    }else {
                        break;
                    }
                }
            }
        }
        System.out.println((System.currentTimeMillis() - start));
        return result;
    }


    public static void main(String[] args) {
        System.out.println(longestPalindrome("vdaaadd"));
    }

}

```

> Think
> 1. Firstly, find out all of substrings
> 2. Secondly, Traversing the Characters of substring from both side to compare if they are same, such as substring[begin]  == substring[end]
> 3. if so,  begin += 1, end -= 1 until begin == end or begin = end -1

---
> 思路
> 1. 先找出所有的子串
> 2. 遍历所有子串，从子串的两边开始遍历，如果`substring[begin] == substring[end]`则`begin += 1,end -= 1`直到`begin==end`，或者`begin + 1 end`时结束，再判断substring的长度是否大于len，

### Review
[10 Tricks to Appear Smart During Meetings](https://medium.com/conquering-corporate-america/10-tricks-to-appear-smart-during-meetings-27b489a39d1a)

Here are 10 tricks:
- Draw a Venn diagram
- Translate percentage metrics into fractions
- Encourage everyone to “take a step back”
- Nod continuously while pretending to take notes
- Repeat the last thing the engineer said, but very very slowly
- Ask “Will this scale?” no matter what it is
- Pace around the room
- Ask the presenter to go back a slide
- Step out for a phone call
- Make fun of yourself

To be honest, I don't agree with the author about the tricks not all of them. It's very impolite! Why do you need to pretend to take notes? Just take notes.! There are always we can learn something from others. Perhaps it's because we have different cultures.

### Tips
1. vim
- 命令模式 u撤回上一步操作
- vim ~/..vimrc  设置个性化操作 如：set nu设置显示行号
2. nginx配置https后需要重启nginx, **reload无效**

### Share
Lately, I moved my website to Tencent Cloud,  so the way to deploy website is changed. Before then I just need to push my code to the github.io repository and wait a second, github will help me to publish my website. But now I must log in my Tencent Cloud terminal to pull the code and restart Jekyll. It's more complicated.

After that, I found Github webhook which can request a custom API. According to this, I can develop an interface which called by Github webhook to execute a shell.

To achieve that I need two things:
1. API server
2. A shell file

**You can read the following article for more details.**
 [基于Github的Webhook实现发布代码后网站自动部署](http://zxsilence.cn/2018/07/%E5%9F%BA%E4%BA%8EGithub%E7%9A%84Webhook%E5%AE%9E%E7%8E%B0%E5%8F%91%E5%B8%83%E4%BB%A3%E7%A0%81%E5%90%8E%E7%BD%91%E7%AB%99%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2/)
---
title: LeetCode解题报告（318)-- 535. Encode and Decode TinyURL
tags:
  - LeetCode
mathjax: true
date: 2021-03-16 12:06:46

---

## Problem

> Note: This is a companion problem to the [System Design](https://leetcode.com/discuss/interview-question/system-design/) problem: [Design TinyURL](https://leetcode.com/discuss/interview-question/124658/Design-a-URL-Shortener-(-TinyURL-)-System/).

TinyURL is a URL shortening service where you enter a URL such as `https://leetcode.com/problems/design-tinyurl` and it returns a short URL such as `http://tinyurl.com/4e9iAk`.

Design the `encode` and `decode` methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

<!-- more -->

------

## Analysis

&emsp;&emsp;题目要求我们实现某种URL的encode和decode机制。解决方法大致上有两种，第一种是使用一种严谨的编码方法，能够编码和解码，同时能保证唯一性，这个是比较难的；第二种是直接建立映射关系，这个在打题的时候是比较实用的。

&emsp;&emsp;如何建立映射关系呢，根据题目给出的例子，短的URL后面是`4e9iAk`六位由数字和英文字母组成的字符串，所以我们的目的就是生成这样一个字符串。对于每个长的URL，我们生产一个六位的随机数字字母串表示，然后使用两个hash map存放两者的对应关系。

&emsp;&emsp;这里需要解决唯一性的问题，即相同的长URL应该encode成相同的短URL，而不同的长URL必须encode成不同的短URL。第一个好解决，因为我们存放了映射关系，每遇到一个新的长URL，就在当前的hash map中找一下是否存在，如果存在则使用已有的映射关系。第二个其实也不难，如果遇到一个新的长URL，生成的字符串已经存在，那么我们重新生成便是，直至与过去的没有重复即可。

------

## Solution

&emsp;&emsp;主要用到的数据结构是使用两个hash map存放长、短URL之间的对应关系。

------

## Code

```c++
class Solution {
public:

    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        if (long2short.count(longUrl)) {
            return shortPrefix + long2short[longUrl];
        }
        int idx = 0;
        string randString;
        for (int i = 0; i < 6; i++) {
            randString.push_back(dict[rand() % 62]);
        }
        while (short2long.count(randString)) {
            randString[idx] = dict[rand() % 62];
            idx = (idx + 1) % 5;
        }
        short2long[randString] = longUrl;
        long2short[longUrl] = randString;
        return shortPrefix + randString;
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        string randString = shortUrl.substr(shortUrl.find_last_of("/") + 1);
        return short2long.count(randString) ? short2long[randString]: shortUrl;
    }
private:
    string dict = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    string shortPrefix = "https://tinyurl.com/";
    unordered_map<string, string> short2long, long2short;
    
};

// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));
```

------

## Summary

&emsp;&emsp;这道题是自由度比较大的设计类题目，思路不唯一。我这里介绍的解法是比较快速能够解决题目的方法，当然不会是比较好的编码方法。这道题目的分享到这里，感谢你的支持！

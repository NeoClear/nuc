---
title: LeetCode 1371 Find the Longest Substring Containing Vowels in Even Counts
date: 2020-03-11 13:32:22
categories:
leetcode
tags:
leetcode
hash
sliding window
---

A medium problem related to sliding window & hashing. Doing some bit operations, might be tested in interviews

<!--more-->

```c++
class Solution {
public:
    int findTheLongestSubstring(string s) {
        const char vowels[] = "aeiou";
        vector<int> image(1 << 5, INT_MAX);
        image[0] = -1;
        int ans = 0;
        int state = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int k = 0; k < 5; k++)
                if (s[i] == vowels[k])
                    state ^= 1 << k;
            if (image[state] == INT_MAX)
                image[state] = i;
            ans = max(ans, i - image[state]);
        }
        return ans;
    }
};
```
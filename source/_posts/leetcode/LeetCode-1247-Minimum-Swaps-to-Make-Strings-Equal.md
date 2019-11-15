---
title: LeetCode 1247 Minimum Swaps to Make Strings Equal
date: 2019-11-03 12:49:43
tags:
- leetcode
- greedy
---

一道简单但较难实现的贪心题。
首先提取两个string不同的部分。
令diff为两个string的x的数量的差。
令pair为两个string不同的pair的数量。
当前context下有两个操作：
1. swap，使diff += 2或diff -= 2，同时pair -= 2。需要swap一次。
2. swap，使diff不变，但pair -= 2。需要swap两次。

这样，题目分为两个情况：
在pair == 2时：
假如diff == 2，操作一次结束。
假如diff == 0，操作两次结束。

```c++
class Solution {
public:
    int cntx(string s) {
        int cnt = 0;
        for (const auto& c : s)
            if (c == 'x')
                cnt++;
        return cnt;
    }
    int minimumSwap(string s1, string s2) {
        if ((cntx(s1) + cntx(s2)) & 0x1)
            return -1;
        string ss1, ss2;
        for (int i = 0; i < s1.size(); i++) {
            if (s1[i] != s2[i]) {
                ss1.push_back(s1[i]);
                ss2.push_back(s2[i]);
            }
        }
        return solve(ss1, ss2);
    }
    int diff(string s1, string s2) {
        int cnt = 0;
        for (int i = 0; i < s1.size(); i++)
            if (s1[i] != s2[i])
                cnt++;
        return cnt;
    }
    int solve(string s1, string s2) {
        int pairs = s1.size() / 2; // 3
        int dif = (abs(cntx(s1) - cntx(s2)) / 2) & 0x1; // 3
        return (pairs + dif) & 0x1 ? pairs + 1 : pairs;
    }
};
```
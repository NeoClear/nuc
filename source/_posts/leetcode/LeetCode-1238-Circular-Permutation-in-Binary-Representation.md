---
title: LeetCode 1238 Circular Permutation in Binary Representation
date: 2019-11-04 21:09:08
tags:
- leetcode
- bit operation
- math
---

这道题和曾经的一道题非常相似。
给定n，生成n位数的数组，每个相邻的数只有一个bit不同。
从0开始，每次开始计算新的一位，就将计算完成的数组倒序排列，每个数组最高位添加1。
因为计算出的数组相邻数只差1bit，所以可以直接使用数组得到新的数组。

```c++
class Solution {
public:
    vector<int> circularPermutation(int n, int start) {
        if (n == 0)
            return {};
        vector<int> ans = {0, 1};
        for (int offset = 1; offset < n; offset++) {
            for (int it = ans.size() - 1; it >= 0; it--) {
                ans.push_back(ans[it] | (1 << offset));
            }
        }
        vector<int> ret;
        for (auto it = ans.begin(); it != ans.end(); it++) {
            if (*it == start) {
                ret = vector<int>(it, ans.end());
                for (auto i = ans.begin(); i != it; i++)
                    ret.push_back(*i);
            }
        }
        return ret;
    }
};
```
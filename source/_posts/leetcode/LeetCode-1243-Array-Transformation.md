---
title: LeetCode 1243 Array Transformation
date: 2019-11-04 12:51:40
tags:
- leetcode
- brute force
---

非常简单的模拟题。。。
似乎只能模拟，不能用高端算法解决。

<!--more-->

```c++
class Solution {
public:
    vector<int> transformArray(vector<int> arr) {
        vector<int> prev;
        vector<int> cur(arr.begin(), arr.end());
        do {
            prev = vector<int>(cur.begin(), cur.end());
            for (int i = 1; i < arr.size() - 1; i++) {
                if (prev[i] > prev[i - 1] && prev[i] > prev[i + 1])
                    cur[i]--;
                else if (prev[i] < prev[i - 1] && prev[i] < prev[i + 1])
                    cur[i]++;
            }
        } while (prev != cur);
        return cur;
    }
};
```
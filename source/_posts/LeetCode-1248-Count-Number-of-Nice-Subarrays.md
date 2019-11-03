---
title: LeetCode 1248 Count Number of Nice Subarrays
date: 2019-11-03 12:45:13
tags:
- leetcode
- sliding window
- subarray
---

这道题是非常简单的组合题。
因为是subarray，而不是subsequence，使得难度大大下降。
将每个偶奇数组合，记录每个group的组数。再排列组合计算总数。

```c++
class Solution {
public:
    int numberOfSubarrays(vector<int>& nums, int k) {
        vector<int> group = {1};
        // group codes
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] & 0x1)
                group.push_back(1);
            else
                group.back()++;
        }
        int cnt = 0;
        // use product rule to calc each subset with k odd numbers
        for (int i = k; i < group.size(); i++)
            cnt += group[i - k] * group[i];
        return cnt;
    }
};
```
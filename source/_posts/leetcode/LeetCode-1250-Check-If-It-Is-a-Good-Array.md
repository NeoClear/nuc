---
title: LeetCode 1250 Check If It Is a Good Array
date: 2019-11-03 11:46:47
tags:
- math
- leetcode
- chinese remainder theorem
---

这道题使用中国剩余定理
假如x % t == 0 && y % t == 0， 那么：
ax + by % t == 0

使用gcd寻找所有数的最大共约数，假如不为1，则无法使用变换得到1
假如为1，则可以通过变换得到1。

```c++
class Solution {
public:
    int gcd(int a, int b) {
        if (b == 0)
            return a;
        return gcd(b, a % b);
    }
    bool isGoodArray(vector<int>& nums) {
        int remainder = nums.front();
        for (const auto& it : nums)
            remainder = gcd(remainder, it);
        return remainder == 1;
    }
};
```
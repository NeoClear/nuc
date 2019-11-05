---
title: LeetCode 1239 Maximum Length of a Concatenated String with Unique Characters
date: 2019-11-04 22:02:10
tags:
- bit
- backtracing
---

又是一道bit manipulattion的题目，这周的出题人很喜欢bit啊。。。（我也比较喜欢bit
因为字母只有26个，所以可以将一个字符的出现视作对应bit上的1。
再进行搜索，尝试每个可能（数据规模允许

有一个技巧：if num1 + num2 == num1 | num2，证明两个数的bit不重叠，反之亦然。

```c++
class Solution {
public:
    vector<int> bits;
    vector<int> length;
    int max_len = 0;
    void solve(int state, int id, int len) {
        if (id == bits.size())
            max_len = max(max_len, len);
        for (int nt = id; nt < bits.size(); nt++) {
            if (bits[nt] + state == (bits[nt] | state)) {
                solve(state | bits[nt], nt + 1, len + length[nt]);
            } else {
                max_len = max(max_len, len);
            }
        }
    }
    bool duplicate(string s) {
        int flag = 0;
        for (const auto& c : s) {
            if (flag == (flag | (1 << (c - 'a'))))
                return true;
            flag |= 1 << (c - 'a');
        }
        return false;
    }
    int maxLength(vector<string>& arr) {
        for (const auto& s : arr) {
            if (duplicate(s))
                continue;
            int state = 0;
            for (const auto& c : s) {
                state |= (1 << (c - 'a'));
            }
            bits.push_back(state);
            length.push_back(s.length());
        }
        solve(0, 0, 0);
        return max_len;
    }
};

```
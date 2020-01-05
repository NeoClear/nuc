---
title: leetcode weekly contest 170
date: 2020-01-05 01:49:23
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

这周的题还行吧，不过因为一些愚蠢的原因，最后一题在比赛结束后10分钟才写出来。。。（没错老年人debug的实力就是这么低

<!--more-->

## Decrypt String from Alphabet to Integer Mapping

Just brute force

```python
class Solution:
    def freqAlphabets(self, s: str) -> str:
        ans = ""
        i = len(s) - 1
        while i >= 0:
            if s[i] == "#":
                ans += chr(int(s[i - 2: i]) + ord("a") - 1)
                i -= 3
            else:
                ans += chr(int(s[i]) + ord("a") - 1)
                i -= 1
        return ans[::-1]
```

## XOR Queries of a Subarray

There is one characteristics about xor operation. First, it supports commutative operation. a ^ b = b ^ a
Moreover, the xor of two same numbers is 0. a ^ a = 0
Thus, in order to get xor[i: j], we can just
    xor[i: j]
  = xor[i: j] ^ xor[0: i - 1] ^ xor[0: i - 1]
  = xor[0: j] ^ xor[0: i - 1]
it is similar to prefix sum. We calculate xor[0: k], for every k, then calc xor[0: j] ^ xor[0: i - 1] to get xor[i: j]

```c++
class Solution {
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {
        vector<int> value = vector<int>(arr.size());
        vector<int> ans;
        for (int i = 0; i < arr.size(); i++) {
            value[i] = (i == 0 ? 0 : value[i - 1]) ^ arr[i];
        }
        for (const auto& it : queries) {
            int l = it[0];
            int r = it[1];
            int cur = value[r] ^ (l == 0 ? 0 : value[l - 1]);
            ans.push_back(cur);
        }
        return ans;
    }
};
```

## Get Watched Videos by Your Friends

Simple bfs problem

```c++
class Solution {
public:
    vector<string> watchedVideosByFriends(vector<vector<string>>& watchedVideos, vector<vector<int>>& friends, int id, int level) {
        unordered_map<string, int> count;
        unordered_set<int> visited;
        queue<int> q;
        q.push(id);
        visited.insert(id);
        for (int i = 0; i < level && !q.empty(); i++) {
            int size = q.size();
            while (size--) {
                int cur = q.front(); q.pop();
                for (const auto& dest : friends[cur]) {
                    if (visited.count(dest))
                        continue;
                    q.push(dest);
                    visited.insert(dest);
                }
            }
        }
        while (!q.empty()) {
            int cur = q.front(); q.pop();
            for (const auto& it : watchedVideos[cur])
                count[it]++;
        }
        vector<pair<int, string>> ans;
        for (const auto& [k, v] : count) {
            ans.push_back({v, k});
        }
        sort(ans.begin(), ans.end());
        vector<string> ret;
        for (const auto& [k, v] : ans)
            ret.push_back(v);
        return ret;
    }
};
```

## Minimum Insertion Steps to Make a String Palindrome

A medium dp with memonization problem. For every string input, the return value must be the same integer
Pay attention to border conditions
If the front & back are the same, just return the value of the string strip the front & back
If the front & back are not the same, return the minimum of the string strip the front and the string strip the back. Plus one to represent one operation

```c++
class Solution {
public:
    unordered_map<int, int> cache;
    int tag(int a, int b) { return (a << 16) | b; }
    int solve(string& s, int l, int r) {
        if (l >= r)
            return 0;
        if (cache.count(tag(l, r)))
            return cache[tag(l, r)];
        if (s[l] == s[r])
            return cache[tag(l, r)] = solve(s, l + 1, r - 1);
        return cache[tag(l, r)] = min(solve(s, l, r - 1),
                                      solve(s, l + 1, r)) + 1;
        
    }
    int minInsertions(string s) {
        return solve(s, 0, s.length() - 1);
    }
};
```
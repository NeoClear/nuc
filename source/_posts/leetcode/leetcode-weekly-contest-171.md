---
title: leetcode weekly contest 171
date: 2020-01-13 02:11:57
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

The last prob is still trickey

<!--more-->

## Convert Integer to the Sum of Two No-Zero Integers

Very simple

```c++
class Solution {
public:
    bool solve(int n) {
        while (n > 0) {
            if (n % 10 == 0)
                return false;
            n /= 10;
        }
        return true;
    }
    vector<int> getNoZeroIntegers(int n) {
        for (int i = 1; i < n; i++) {
            if (solve(i) && solve(n - i))
                return {i, n - i};
        }
        return {};
    }
};
```

## Minimum Flips to Make a OR b Equal to c

Bit operation

```c++
class Solution {
public:
    int minFlips(int a, int b, int c) {
        int cnt = 0;
        for (int i = 0; i < 32; i++) {
            if ((c >> i) & 0x1) {
                cnt += 1 - (((a >> i) & 0x1) | ((b >> i) & 0x1));
            } else {
                cnt += ((a >> i) & 0x1) + ((b >> i) & 0x1);
            }
        }
        return cnt;
    }
};
```

## Number of Operations to Make Network Connected

Union find problem
Find number of distinct islands, and check if cables are enough

```c++
class Solution {
public:
    vector<int> parent;
    int find(int x) {
        if (parent[x] == x)
            return x;
        return parent[x] = find(parent[x]);
    }
    void merge(int x, int y) {
        parent[find(x)] = find(y);
    }
    int makeConnected(int n, vector<vector<int>>& connections) {
        int ans = 0;
        parent = vector<int>(n);
        for (int i = 0; i < n; i++)
            parent[i] = i;
        for (const auto& it : connections) {
            int x = it.front();
            int y = it.back();
            merge(x, y);
        }
        unordered_set<int> group;
        for (int i = 0; i < n; i++) {
            group.insert(find(i));
        }
        return connections.size() >= n - 1 ? group.size() - 1 : -1;
    }
};
```

## Minimum Distance to Type a Word Using Two Fingers

Difficult

dp(i, l, r) means up to i-th step, with left hand on l & right hand on r, the number of distance that could finish the word from i to the end

```c++
class Solution {
public:
    int minimumDistance(string word) {
        constexpr int kRest = 26;
        const int n = word.length();
        vector<vector<vector<int>>> mem(n, vector<vector<int>>(27, vector<int>(27, -1)));
        auto dis = [](int a, int b) {
            if (a == kRest) return 0;
            return abs(a / 6 - b / 6) + abs(a % 6 + b % 6);
        };
        function<int(int, int, int)> dp = [&](int i, int l, int r) {
            if (i == n) return 0;
            if (mem[i][j][k] != -1) return mem[i][j][k];
            int d = word[i] - 'A';
            return mem[i][l][r] = min(dp(i + 1, d, r) + dis(l, d),
                                      dp(i + 1, l, d) + dis(r, d));
        };
        return dp(0, kRest, kRest);
    }
};
```
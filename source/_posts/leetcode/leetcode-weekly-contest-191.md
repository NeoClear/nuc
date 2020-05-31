---
title: leetcode weekly contest 191
date: 2020-05-31 02:36:34
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

Easy but wired week

<!--more-->

## Maximum Product of Two Elements in an Array

Easy

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        reverse(nums.begin(), nums.end());
        return (nums[0] - 1) * (nums[1] - 1);
    }
};
```

## Maximum Area of a Piece of Cake After Horizontal and Vertical Cuts

Simple

```c++
typedef long long ll;

class Solution {
public:
    int maxArea(int h, int w, vector<int>& horizontalCuts, vector<int>& verticalCuts) {
        constexpr ll mod = 1000000007;
        horizontalCuts.push_back(h);
        verticalCuts.push_back(w);
        sort(horizontalCuts.begin(), horizontalCuts.end());
        sort(verticalCuts.begin(), verticalCuts.end());
        ll mh = 0, mw = 0;
        ll pre = 0;
        for (const auto& it : horizontalCuts) {
            mh = max(mh, it - pre);
            pre = it;
        }
        pre = 0;
        for (const auto& it : verticalCuts) {
            mw = max(mw, it - pre);
            pre = it;
        }
        return (mh * mw) % mod;
    }
};
```

## Reorder Routes to Make All Paths Lead to the City Zero

Simple tree traversal

```c++
class Solution {
public:
    int minReorder(int n, vector<vector<int>>& connections) {
        unordered_map<int, vector<pair<int, bool>>> graph;
        for (const auto& e : connections) {
            int from = e[0], to = e[1];
            graph[from].push_back({to, true});
            graph[to].push_back({from, false});
        }
        int ans = 0;
        function<void(int, int)> solve = [&](int cur, int pre) {
            for (const auto& dest : graph[cur]) {
                if (dest.first == pre)
                    continue;
                if (dest.second)
                    ans++;
                solve(dest.first, cur);
            }
        };
        solve(0, -1);
        return ans;
    }
};
```

## Probability of a Two Boxes Having The Same Number of Distinct Balls

Wired prob. If you use long long it would overflow, but it works for double

```c++
typedef long long ll;

class Solution {
public:
    double fact(double n) {
        if (n <= 1)
            return 1;
        return n * fact(n - 1);
    }
    double C(double n, double k) {
        return fact(n) / (fact(k) * fact(n - k));
    }
    double solve(ll stage, ll remain, ll balance, vector<int>& balls) {
        if (remain < 0)
            return 0;
        if (stage == balls.size()) {
            return (remain == 0 && balance == 0) ? 1 : 0;
        }
        double ans = 0;
        for (ll i = 0; i <= balls[stage]; i++) {
            if (i == 0)
                ans += C(balls[stage], i) * solve(stage + 1, remain, balance + 1, balls);
            else if (i == balls[stage])
                ans += C(balls[stage], i) * solve(stage + 1, remain - i, balance - 1, balls);
            else
                ans += C(balls[stage], i) * solve(stage + 1, remain - i, balance, balls);
        }
        return ans;
    }
public:
    double getProbability(vector<int> balls) {
        ll sum = accumulate(balls.begin(), balls.end(), 0);
        double a = solve(0, sum / 2, 0, balls);
        return (double)a / (double)C(sum, sum / 2);
    }
};
```
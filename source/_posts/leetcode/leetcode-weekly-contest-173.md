---
title: leetcode weekly contest 173
date: 2020-01-27 00:22:09
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

The first time that I finish all problems neatly within time limit. Well done!
The second prob is actually the most difficult one (some tricks required)

<!--more-->

The first one is very easy if you could understand the mechanism behind it
If empty, return 0
If palindrome, return 1
The rest would be 2 because you can get all a's then all b's

## Remove Palindromic Subsequences
```python
class Solution:
    def removePalindromeSub(self, s: str) -> int:
        if len(s) == 0:
            return 0
        if s == s[::-1]:
            return 1
        return 2
```

## Filter Restaurants by Vegan-Friendly, Price and Distance

Looks difficult, but its idea is simple, just brute force

```c++
#include <iostream>
#include <vector>

using namespace std;

class Solution {
public:
    vector<int> filterRestaurants(vector<vector<int>>& restaurants, int veganFriendly, int maxPrice, int maxDistance) {
        vector<pair<int, int>> ans;
        // id, rating, veg, price, dis
        for (const auto& it : restaurants) {
            int id = it[0];
            int rating = it[1];
            int veg = it[2];
            int price = it[3];
            int dis = it[4];
            if (price > maxPrice || dis > maxDistance) { continue; }
            if (veganFriendly == 1 && veg == 0) { continue; }
            ans.push_back({id, rating});
        }
        sort(ans.begin(), ans.end(), [](pair<int, int>& a, pair<int, int>& b) {
            if (a.second == b.second)
                return a.first > b.first;
            return a.second > b.second;
        });
        vector<int> ret;
        for (const auto& it : ans) { ret.push_back(it.first); }
        return ret;
    }
};
```

## Find the City With the Smallest Number of Neighbors at a Threshold Distance

Perform one source closest path to every point. One trick: You should mark a point as visited when it pops from the priority_queue, while for simple queue, you add it when you add it to the queue

```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <unordered_set>
#include <queue>

using namespace std;

class Solution {
public:
    unordered_map<int, vector<pair<int, int>>> graph;
    int bfs(int i, int threshold) {
        unordered_set<int> visited;
        priority_queue<pair<int, int>> q;   // remain, dest
        q.push({threshold, i});
        while (!q.empty()) {
            int cur = q.top().second;
            int dis = q.top().first;
            q.pop();
            if (visited.count(cur)) { continue; }
            visited.insert(cur);
            for (const auto& dest : graph[cur]) {
                int dest_id = dest.first;
                int remain = dis - dest.second;
                if (remain < 0) { continue; }
                q.push({remain, dest_id});
            }
        }
        return visited.size() - 1;
    }
    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        vector<int> reach(n);
        for (const auto& e : edges) {
            int from = e[0];
            int to = e[1];
            int dis = e[2];
            graph[from].push_back({to, dis});
            graph[to].push_back({from, dis});
        }
        for (int i = 0; i < n; i++) { reach[i] = bfs(i, distanceThreshold); }
        pair<int, int> ans = {-1, 200};
        for (int i = 0; i < n; i++) {
            cout << reach[i] << endl;
            if (reach[i] <= ans.second) { ans = {i, reach[i]}; }
        }
        return ans.first;
    }
};
```

## Minimum Difficulty of a Job Schedule

Just a dp with memonization. Simpler than his brothers

```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <unordered_set>
#include <queue>
#include <algorithm>

using namespace std;

class Solution {
public:
    int tag(int a, int b) { return (a << 16) | b; }
    unordered_map<int, int> cache;
    int solve(int item, int day, vector<int>& jobDifficulty) {
        if (cache.count(tag(item, day))) { return cache[tag(item, day)]; }
        if (day == 1) {
            int diff = 0;
            for (int i = 0; i < item; i++) { diff = max(diff, jobDifficulty[i]); }
            return cache[tag(item, day)] = diff;
        }
        int diff = 0;
        int ans = INT_MAX / 2;
        for (int i = item - 1; i >= day - 1; i--) {
            diff = max(diff, jobDifficulty[i]);
            ans = min(ans, solve(i, day - 1, jobDifficulty) + diff);
        }
        return cache[tag(item, day)] = ans;
    }
    int minDifficulty(vector<int>& jobDifficulty, int d) {
        if (jobDifficulty.size() < d) { return -1; }
        return solve(jobDifficulty.size(), d, jobDifficulty);
    }
};
```

---
title: leetcode weekly contest 179
date: 2020-03-08 03:40:06
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

Easy week

<!--more-->

## Generate a String With Characters That Have Odd Counts

Simple

```c++
class Solution {
public:
    string generateTheString(int n) {
        if (n & 0b1)
            return string(n, 'x');
        return string(n - 1, 'x') + "y";
    }
};
```

## Bulb Switcher III

Sliding window?

```c++
class Solution {
public:
    int numTimesAllBlue(vector<int>& light) {
        int last = 0;
        int cnt = 0;
        int ans = 0;
        for (const auto& it : light) {
            last = max(last, it);
            cnt++;
            if (last == cnt)
                ans++;
        }
        return ans;
    }
};
```

## Time Needed to Inform All Employees

BFS

```c++
class Solution {
public:
    void solve(int id, int time, int& ans, unordered_map<int, vector<int>>& g, vector<int>& informTime) {
        ans = max(ans, time);
        for (const auto& it : g[id])
            solve(it, time + informTime[id], ans, g, informTime);
    };
    int numOfMinutes(int n, int headID, vector<int> manager, vector<int> informTime) {
        unordered_map<int, vector<int>> g;
        for (int i = 0; i < n; i++)
            g[manager[i]].push_back(i);
        int ans = 0;
        solve(headID, 0, ans, g, informTime);
        return ans;
    }
};
```

A better solution from bottom up

```c++
class Solution {
public:
    int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) {
        unordered_map<int, int> cache;
        function<int(int)> solve = [&](int cur) {
            if (cur == headID)
                return 0;
            if (cache.count(cur))
                return cache[cur];
            return cache[cur] = informTime[manager[cur]] + solve(manager[cur]);
        };
        int ans = 0;
        for (int i = 0; i < n; i++)
            ans = max(ans, solve(i));
        return ans;
    }
};
```

## Frog Position After T Seconds

BFS

```c++
class Solution {
private:
    unordered_map<int, vector<int>> tree;
public:
    double frogPosition(int n, vector<vector<int>> edges, int t, int target) {
        for (const auto& it : edges) {
            tree[it.front()].push_back(it.back());
            tree[it.back()].push_back(it.front());
        }
        // {id, prob, time}
        unordered_set<int> visited;
        queue<tuple<int, double, int>> q;
        q.push({1, 1.0, 0});
        visited.insert(1);
        while (!q.empty()) {
            int id;
            double prob;
            int time;
            tie(id, prob, time) = q.front();
            q.pop();
            if (id == target && t == time) {
                return prob;
            }
            if (time > t)
                break;
            int num_dest = tree[id].size() - (id == 1 ? 0 : 1);
            for (const auto& dest : tree[id]) {
                if (visited.count(dest))
                    continue;
                visited.insert(dest);
                // cout << dest << ", " << prob / num_dest << endl;
                q.push({dest, prob / num_dest, time + 1});
            }
            if (num_dest == 0)
                q.push({id, prob, time + 1});
        }
        return 0.0;
    }
};
```

Cleaner version

```c++
class Solution {
public:
    double frogPosition(int n, vector<vector<int>>& edges, int t, int target) {
        unordered_map<int, vector<int>> graph;
        for (const auto& it : edges) {
            graph[it.front()].push_back(it.back());
            graph[it.back()].push_back(it.front());
        }
        // [node, prob]
        unordered_set<int> visited;
        queue<pair<int, double>> q;
        q.push({1, 1.0});
        visited.insert(1);
        int steps = 0;
        while (!q.empty() && steps <= t) {
            int size = q.size();
            while (size--) {
                int cur;
                double prob;
                tie(cur, prob) = q.front();
                q.pop();
                if (cur == target && steps == t)
                    return prob;
                int num_dest = graph[cur].size() - (cur == 1 ? 0 : 1);
                if (num_dest == 0 && cur == target)
                    return prob;
                for (const auto& dest : graph[cur]) {
                    if (visited.count(dest))
                        continue;
                    visited.insert(dest);
                    q.push({dest, prob / num_dest});
                }
            }
            steps++;
        }
        return 0.0;
    }
};
```

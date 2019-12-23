---
title: LeetCode 1245 Tree Diameter
date: 2019-11-04 15:32:56
tags:
- leetcode
- tree
- dfs
- graph
---

还算简单的题目。
使用dfs遍历graph，函数返回这个节点能返回的最长路径的点数。
再dfs中使用返回值计算最长路径并更新，最后得出答案。

<!--more-->

```c++
class Solution {
public:
    unordered_map<int, vector<int>> graph;
    unordered_set<int> visited;
    int ans = 0;
    int get_max(vector<int>& v) {
        int cnt = 0;
        for (const auto& it : v)
            cnt = max(cnt, it);
        return cnt;
    }
    int sum_max2(vector<int> v) {
        sort(v.begin(), v.end());
        int acc = 2;
        int cnt = 0;
        for (auto it = v.rbegin(); it != v.rend() && acc; it++, acc--)
            cnt += *it;
        return cnt;
    }
    int solve(int id) {
        visited.insert(id);
        vector<int> go;
        for (const auto& dest : graph[id])
            if (!visited.count(dest))
                go.push_back(solve(dest));
        ans = max(ans, sum_max2(go) + 1);
        return get_max(go) + 1;
    }

    int treeDiameter(vector<vector<int>> edges) {
        for (const auto& e : edges) {
            graph[e[0]].push_back(e[1]);
            graph[e[1]].push_back(e[0]);
        }
        solve(0);
        return ans - 1;
    }
};
```

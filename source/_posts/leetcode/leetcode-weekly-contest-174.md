---
title: leetcode weekly contest 174
date: 2020-02-02 00:42:47
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

Made too many incorrect attempts. Easy anyway, 1000 people finished all probs

<!--more-->

## The K Weakest Rows in a Matrix

Count and sort

```c++
class Solution {
public:
    
    vector<int> kWeakestRows(vector<vector<int>>& mat, int k) {
        map<pair<int, int>, int> graph;
        for (int i = 0; i < mat.size(); i++) {
            int val = accumulate(mat[i].begin(), mat[i].end(), 0);
            graph[{val, i}] = i;
        }
        int id = 0;
        vector<int> ans;
        for (const auto& it : graph) {
            if (id >= k) { break; }
            ans.push_back(it.second);
            id++;
        }
        return ans;
    }
};
```

## Reduce Array Size to The Half

Count and sort

```c++
class Solution {
public:
    int minSetSize(vector<int>& arr) {
        int N = arr.size();
        if (N == 0) { return 0; }
        unordered_map<int, int> count;
        for (const auto& it : arr) { count[it]++; }
        vector<int> vals;
        for (const auto& it : count) {
            vals.push_back(it.second);
        }
        int ans = 0;
        int cnt = 0;
        sort(vals.begin(), vals.end());
        for (auto it = vals.rbegin(); it != vals.rend(); it++) {
            cnt += *it;
            ans++;
            if (cnt * 2 >= N) { return ans; }
        }
        return vals.size();
    }
};
```

## Maximum Product of Splitted Binary Tree

Tree traversal

```c++
class Solution {
public:
    unordered_map<TreeNode*, int> sums;
    int calc(TreeNode* node) {
        int val = 0;
        if (node->left)
            val += calc(node->left);
        if (node->right)
            val += calc(node->right);
        return sums[node] = val + node->val;
    }
    int maxProduct(TreeNode* root) {
        constexpr int mod = 1000000007;
        if (root == NULL) { return 0; }
        calc(root);
        long long ans = 0;
        long long total = sums[root];
        for (const auto& it : sums) {
            long long cur = it.second;
            ans = max(ans, (total - cur) * cur);
        }
        return ans % mod;
    }
};
```

## Jump Game V

dp with memonization

```c++
class Solution {
public:
    unordered_map<int, int> cache;  // cache[i] is the maximum number of indices can visit
    int solve(int id, int d, vector<int>& arr) {
        if (cache.count(id)) { return cache[id]; }
        int ans = 0;
        for (int i = id - 1; i >= 0 && arr[i] < arr[id] && id - i <= d; i--)
            ans = max(ans, solve(i, d, arr));
        for (int i = id + 1; i < arr.size() && arr[i] < arr[id] && i - id <= d; i++)
            ans = max(ans, solve(i, d, arr));
        return cache[id] = ans + 1;
    }
    int maxJumps(vector<int>& arr, int d) {
        int ans = 0;
        for (int i = 0; i < arr.size(); i++)
            ans = max(ans, solve(i, d, arr));
        return ans;
    }
};
```
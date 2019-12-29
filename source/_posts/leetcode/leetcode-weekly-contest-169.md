---
title: leetcode weekly contest 169
date: 2019-12-29 00:38:15
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

这周的contest有点怪啊，前三题简单得离谱，最后一题却非常难（可以做但是不知如何优化、剪枝）

<!--more-->

## Find N Unique Integers Sum up to Zero

Very simple problem (people who need explanation for this problem must be monkey)

```python
from typing import List

class Solution:
    def sumZero(self, n: int) -> List[int]:
        ans = []
        for i in range(n // 2):
            ans.append(i * 2 + 1)
            ans.append(-(i * 2 + 1))
        if n % 2 == 1:
            ans.append(0)
        return ans

```

## All Elements in Two Binary Search Trees

Very simple binary tree traverse problem
Can be improved because both trees are binary search trees, they are sorted themselves, so it is feasible to get rid of the sort at the end

```c++
#include <vector>
#include <queue>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    vector<int> getAllElements(TreeNode* root1, TreeNode* root2) {
        vector<int> ans;
        queue<TreeNode *> q;
        if (root1)
            q.push(root1);
        if (root2)
            q.push(root2);
        while (!q.empty()) {
            auto node = q.front(); q.pop();
            ans.push_back(node->val);
            if (node->left)
                q.push(node->left);
            if (node->right)
                q.push(node->right);
        }
        sort(ans.begin(), ans.end());
        return ans;
    }
};
```

## Jump Game III

A simple bfs problem

Just search the line to see if it can obtain 0 at some point

```c++
#include <vector>
#include <iostream>
#include <unordered_set>
#include <queue>

using namespace std;

class Solution {
public:
    bool canReach(vector<int>& arr, int start) {
        vector<int> dirs = {1, -1};
        int size = arr.size();
        unordered_set<int> visited;
        queue<int> q;
        q.push(start);
        visited.insert(start);
        while (!q.empty()) {
            auto cur = q.front(); q.pop();
            if (arr[cur] == 0)
                return true;
            for (const auto& it : dirs) {
                int dest = cur + arr[cur] * it;
                if (dest >= size || dest < 0)
                    continue;
                if (!visited.count(dest)) {
                    q.push(dest);
                    visited.insert(dest);
                }
            }
        }
        return false;
    }
};
```

## Verbal Arithmetic Puzzle

The last prob is toooooooo difficult. It is not difficult to have the solution, but it is really difficult to optimize it such that it could pass the test

Waiting for optimized solution
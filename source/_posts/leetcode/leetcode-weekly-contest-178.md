---
title: leetcode weekly contest 178
date: 2020-03-07 21:18:35
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

The last prob is easier than expected

<!--more-->

## How Many Numbers Are Smaller Than the Current Number

Sort

```c++
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        vector<int> rank(nums.begin(), nums.end());
        sort(rank.begin(), rank.end());
        unordered_map<int, int> mm;
        for (int i = 0; i < rank.size(); i++) {
            if (!mm.count(rank[i]))
                mm[rank[i]] = i;
        }
        vector<int> ans;
        for (const auto& it : nums) {
            ans.push_back(mm[it]);
        }
        return ans;
    }
};
```

## Rank Teams by Votes

Special sorting

```c++
class Solution {
public:
    string rankTeams(vector<string>& votes) {
        int N = votes[0].size();
        vector<unordered_map<char, int>> count(N);
        for (const auto& line : votes) {
            for (int i = 0; i < N; i++) {
                char ch = line[i];
                count[i][ch]++;
            }
        }
        string ans(votes[0].begin(), votes[0].end());
        sort(ans.begin(), ans.end(), [&](char& a, char& b) {
            for (int i = 0; i < N; i++) {
                if (count[i][a] == count[i][b])
                    continue;
                return count[i][a] > count[i][b];
            }
            return a < b;
        });
        return ans;
    }
};
```

## Linked List in Binary Tree

Simple traversal

```c++
class Solution {
public:
    bool isSubPath(ListNode* head, TreeNode* root) {
        vector<int> lst;
        while (head) {
            lst.push_back(head->val);
            head = head->next;
        }
        vector<int> tree;
        bool solved = false;
        auto match = [&]() {
            for (auto l = lst.rbegin(), t = tree.rbegin(); l != lst.rend(); l++, t++) {
                if (*l != *t)
                    return;
            }
            solved = true;
        };
        function<void(TreeNode*)> solve = [&](TreeNode* cur) {
            if (cur == NULL)
                return;
            if (solved)
                return;
            tree.push_back(cur->val);
            if (tree.size() >= lst.size())
                match();
            solve(cur->left);
            solve(cur->right);
            tree.pop_back();
        };
        solve(root);
        return solved;
    }
};
```

## Minimum Cost to Make at Least One Valid Path in a Grid

Dijstra

```c++
class Solution {
public:
    int minCost(vector<vector<int>>& grid) {
        int M = grid.size(), N = grid[0].size();
        vector<pair<int, int>> dirs = {
            {0, 0},
            {0, 1},
            {0, -1},
            {1, 0},
            {-1, 0}
        };
        auto gen = [](int x, int y) {
            return (x << 16) | y;
        };
        auto getX = [](int addr) {
            return addr >> 16;
        };
        auto getY = [](int addr) {
            return addr & 0xffff;
        };
        // -cost, id
        priority_queue<pair<int, int>> q;
        unordered_set<int> visited;
        q.push({0,  0});
        while (!q.empty()) {
            int addr, cost;
            tie(cost, addr) = q.top();
            q.pop();
            int x = getX(addr);
            int y = getY(addr);
            cost = -cost;
            // cout << "cost: " << cost << ", index: (" << x << ", " << y << ")" << endl;
            if (x == M - 1 && y == N - 1)
                return cost;
            if (visited.count(addr))
                continue;
            visited.insert(addr);
            for (int i = 1; i <= 4; i++) {
                int tx = x + dirs[i].first;
                int ty = y + dirs[i].second;
                if (tx < 0 || tx >= M ||
                    ty < 0 || ty >= N)
                    continue;
                if (grid[x][y] == i)
                    q.push({-cost, gen(tx, ty)});
                else
                    q.push({-cost - 1, gen(tx, ty)});
            }
        }
        return INT_MAX;
    }
};
```
---
title: leetcode weekly contest 172
date: 2020-01-19 01:33:07
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

This week I started one hour after the contest began. I should have finish all problems if I started normally

<!--more-->

## Maximum 69 Number

Very simple

```python
class Solution:
    def maximum69Number (self, num: int) -> int:
        s = str(num)
        ans = ""
        flag = True
        for c in s:
            if c == "6" and flag:
                flag = False
                ans += "9"
            else:
                ans += c
        return int(ans)
```

## Print Words Vertically

Please know that spaces at the end of each word will be deleted

```python
class Solution:
    def printVertically(self, s: str) -> List[str]:
        words = list(s.split())
        m = 0
        for w in words:
            m = max(m, len(w))
        ret = []
        for i in range(len(words)):
            words[i] += (m - len(words[i])) * " "
        for i in range(m):
            ret.append("")
        for i in range(len(words)):
            for j in range(m):
                ret[j] += words[i][j]
        for i in range(len(ret)):
            ret[i] = ret[i].rstrip()
        return ret
```

## Delete Leaves With a Given Value

Simple traversal problem

```c++
class Solution {
public:
    int t;
    void solve(TreeNode* par, TreeNode* cur, int& target) {
        if (cur->left)
            solve(cur, cur->left, target);
        if (cur->right)
            solve(cur, cur->right, target);
        if (cur->left == nullptr && cur->right == nullptr && cur->val == target) {
            if (par->left == cur)
                par->left = nullptr;
            if (par->right == cur)
                par->right = nullptr;
        }
    }
    TreeNode* removeLeafNodes(TreeNode* root, int target) {
        TreeNode* head = new TreeNode(0);
        head->left = root;
        solve(head, root, target);
        return head->left;
    }
};
```

## Minimum Number of Taps to Open to Water a Garden

A **not that difficult search with memonization prob**

solve(i, n, cover) indicated the minimum number of covers starts with i to the end of the garden
results are cached

```c++
class Solution {
public:
    unordered_map<int, int> cache;
    int solve(int i, int& n, map<int, int>& cover) {
        if (i >= n) { return 0; }
        if (cache.count(i)) { return cache[i]; }
        auto it = cover.upper_bound(i);
        if (it == cover.begin()) { return -1; }
        int ans = INT_MAX / 2;
        do {
            it = prev(it);
            if (it->second > i) { ans = min(ans, solve(it->second, n, cover)); }
        } while (it != cover.begin());
        if (ans == INT_MAX / 2)
            ans = -1;
        return cache[i] = ans == -1 ? -1 : ans + 1;
    }
    int minTaps(int n, vector<int>& ranges) {
        map<int, int> cover;
        for (int i = 0; i < ranges.size(); i++) { cover[i - ranges[i]] = i + ranges[i]; }
        return solve(0, n, cover);
    }
};
```

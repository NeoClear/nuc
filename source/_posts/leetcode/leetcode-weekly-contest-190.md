---
title: leetcode weekly contest 190
date: 2020-05-26 17:29:05
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

Really easy week

<!--more-->

## Check If a Word Occurs As a Prefix of Any Word in a Sentence

Split the string and use find method

```python
class Solution:
    def isPrefixOfWord(self, sentence: str, searchWord: str) -> int:
        dic = list(sentence.split())
        for i in range(len(dic)):
            if dic[i].find(searchWord) == 0:
                return i + 1
        return -1
```

## Maximum Number of Vowels in a Substring of Given Length

Slidign window

```c++
class Solution {
private:
    bool isVowel(char c) {
        return c == 'a' || c == 'e' || c == 'i' ||
               c == 'o' || c == 'u';
    }
public:
    int maxVowels(string s, int k) {
        int N = s.length();
        int ans = 0;
        int acc = 0;
        int iter = min(k - 1, N - 1);
        for (int i = 0; i <= iter; i++) {
            acc += isVowel(s[i]) ? 1 : 0;
            ans = max(ans, acc);
        }
        while (iter != N - 1) {
            iter++;
            if (isVowel(s[iter]))
                acc++;
            if (isVowel(s[iter - k]))
                acc--;
            ans = max(ans, acc);
        }
        return ans;
    }
};
```

## Pseudo-Palindromic Paths in a Binary Tree

Bit operation and recursion

```c++
class Solution {
private:
    void insert(int& state, int pos) {
        state ^= (1 << pos);
    }
    bool isPalindromic(int state) {
        if (state == 0)
            return true;
        while (!(state & 0x1))
            state >>= 1;
        return state == 1;
    }
public:
    int pseudoPalindromicPaths(TreeNode* root, int state = 0) {
        insert(state, root->val);
        if (root->left == nullptr && root->right == nullptr)
            return isPalindromic(state) ? 1 : 0;
        int acc = 0;
        if (root->left)
            acc += pseudoPalindromicPaths(root->left, state);
        if (root->right)
            acc += pseudoPalindromicPaths(root->right, state);
        return acc;
    }
};
```

## Max Dot Product of Two Subsequences

The simpliest dp I have ever seen
let dp\[i\]\[j\] be the largest dot product of the first i num in nums1 and first j num in nums2
Transfer equation: dp\[i\]\[j\] = max(dp\[i - 1\]\[j - 1\] + nums1[i] * nums2[j], dp\[i - 1\]\[j\], dp\[i\]\[j - 1\])

```c++
class Solution {
public:
    int maxDotProduct(vector<int>& nums1, vector<int>& nums2) {
        int N = nums1.size(), M = nums2.size();
        int dp[N][M];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                dp[i][j] = nums1[i] * nums2[j];
                if (i && j)
                    dp[i][j] += max(dp[i - 1][j - 1], 0);
                if (i)
                    dp[i][j] = max(dp[i][j], dp[i - 1][j]);
                if (j)
                    dp[i][j] = max(dp[i][j], dp[i][j - 1]);
            }
        }
        return dp[N - 1][M - 1];
    }
};
```
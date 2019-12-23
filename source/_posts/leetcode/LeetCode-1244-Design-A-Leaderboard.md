---
title: LeetCode 1244 Design A Leaderboard
date: 2019-11-04 14:52:10
tags:
- leetcode
- data structure
- map
- set
---

就是一道普通的数据结构题。
类似map，只是通过value排序。
所以使用map记录multiset的iterator，通过multiset记录value。

<!--more-->

```c++
class Leaderboard {
public:
    unordered_map<int, multiset<int, std::greater<int>>::iterator> board;
    multiset<int, std::greater<int>> scores;

    Leaderboard() {

    }

    void addScore(int playerId, int score) {
        if (!board.count(playerId)) {
            board[playerId] = scores.insert(0);
        }
        auto old = board[playerId];
        board[playerId] = scores.insert(*old + score);
        scores.erase(old);
    }

    int top(int K) {
        int cnt = 0;
        for (auto it = scores.begin(); it != scores.end(), K--; it++, K)
            cnt += *it;
        return cnt;
    }

    void reset(int playerId) {
        auto old = board[playerId];
        board[playerId] = scores.insert(0);
        scores.erase(old);
    }
};
```

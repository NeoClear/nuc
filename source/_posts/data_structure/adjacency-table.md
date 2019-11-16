---
title: adjacency table
date: 2019-11-16 17:58:40
tags:
- adjacency table
- graph
---

传统的邻接表。比用vector和map实现效率更高。
vertex[i]记录与点i相邻的点的第一个Edge边。通过遍历获得所有相邻的点。
需要注意的是边的index从1开始，因为0代表着遍历的结束。

```c++
#include <bits/stdc++.h>

#define SIZE 100010
typedef long long ll;

using namespace std;

struct Edge {
    ll to, w, next;
} edges[SIZE];

ll top = 0;
ll vertex[SIZE];

inline void insert(ll from, ll to, ll w) { edges[++top] = (Edge){to, w, vertex[from]}; vertex[from] = top; }

inline void init() { memset(vertex, 0, sizeof(vertex)); }

int main() {
    init();
    return 0;
}
```
---
title: LeetCode 1293 Shortest Path in a Grid with Obstacles Elimination
date: 2019-12-19 12:26:13
tags:
- leetcode
- bfs
- queue
---

这道题乍一看挺复杂的，不能直接套用bfs的模板做题。但其实理解了题目之后就非常简单了。。。（说到底就是智商不够）。

<!--more-->

```c++
class Solution {
public:
    int shortestPath(vector<vector<int>>& grid, int k) {
        const vector<int> dirs = {1, 0, -1, 0, 1};
        const int n = grid.size();
        const int m = grid[0].size();
        vector<vector<int>> visited(n, vector<int>(m, INT_MAX));
        queue<tuple<int, int, int>> q;  // (x, y, o)
        int steps = 0;
        q.push({0, 0, 0});
        visited[0][0] = 0;
        while (!q.empty()) {
            int size = q.size();
            while (size--) {
                int x, y, o;
                tie(x, y, o) = q.front(); q.pop();
                if (x == n - 1 && y == m - 1)
                    return steps;
                for (int i = 0; i < 4; i++) {
                    int tx = x + dirs[i];
                    int ty = y + dirs[i + 1];
                    if (tx < 0 || ty < 0 ||
                        tx >= n || ty >= m)
                        continue;
                    int to = o + grid[tx][ty];
                    if (to >= visited[tx][ty] || to > k)
                        continue;
                    visited[tx][ty] = to;
                    q.push({tx, ty, to});
                }
            }
            steps++;
        }
        return -1;
    }
};
```

这里我们使用了传统的bfs方法遍历整个graph，但是我们加入了一个visited二维数组来储存到达每个点穿过的最少obstacles的数量。因为bfs是一层一层进行的，所以后被遍历到的点使用的步数一定比之前的大。假如穿过的obstacles数量还更大，那么便没有利用的价值了。所以接下来遍历的时候，只将obstacle数量小于当前数量的走法进队列。最后在队列出口处加一段判断终点的代码。（完美，要是在比赛的时候能想出来就好了）。
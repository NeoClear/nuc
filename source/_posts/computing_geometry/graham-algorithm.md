---
title: graham algorithm
date: 2019-11-15 00:50:03
tags:
- graham
- convex hull
- geometry
---

非常传统的凸包算法。
通过排序和栈计算凸包。
值得注意的是，一开始使用c++的高级特性，无法过后面的测试点。
但是改用了最传统的数组后，成功通过测试。
这就是NOI的怨念🐎？

graham算法详情参考算法导论计算几何章节

```c++
#include <bits/stdc++.h>

#define SIZE 10010
#define eps 1e-10

using namespace std;

int dcmp(double d) {
    if (abs(d) < eps)
        return 0;
    return d < 0 ? -1 : 1;
}

struct point {
    double x, y;
};

int N;
struct point pos[SIZE];

double cross(point a, point b, point base) {
    return (a.x - base.x) * (b.y - base.y) - (b.x - base.x) * (a.y - base.y);
}

double dis(point a, point b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

struct point convex[SIZE];
int top = 0;

int main() {
    freopen("in.txt", "r", stdin);
    scanf("%d", &N);
    point origin = {0, 0};
    for (int i = 0; i < N; i++) {
        scanf("%lf%lf", &pos[i].x, &pos[i].y);
    }
    sort(pos, pos + N, [](point& a, point& b) {
        return dcmp(a.y - b.y) ? a.y < b.y : a.x < b.x;
    });
    point base = pos[0];
    sort(pos + 1, pos + N, [&base, &origin](point& a, point& b) {
        double c = cross(a, b, base);
        return dcmp(c) ? c > 0 : dis(a, origin) < dis(b, origin);
    });
    convex[0] = pos[0];
    convex[1] = pos[1];
    convex[2] = pos[2];
    top = 3;
    for (int i = 3; i < N; i++) {
        while (top > 1 && dcmp(cross(convex[top - 1], pos[i], convex[top - 2])) <= 0)
            top--;
        convex[top] = pos[i];
        top++;
    }
    // for (int i = 0; i < top; i++)
    //  cout << convex[i].x << ", " << convex[i].y << endl;
    convex[top] = convex[0];
    double ans = 0;
    for (int i = 0; i < top; i++)
        ans += dis(convex[i], convex[i + 1]);
    printf("%.2lf\n", ans);
}
```
---
title: segment tree
date: 2019-11-09 21:40:45
tags:
- segment tree
- data structure
---

线段树，常见却又复杂。
其基本原理较为简单，但实现中有这种各样的细节需要注意。

```c++
struct segment_node {
    int left;
    int right;
    int weight;
    int lazy;
};

struct segment_node tree[size * 4 + 1];
```
其中lazy值为此区间的子区间的延迟更新值

第一步是建树过程。
数组大小为N * 4 + 1。最好再加点量（
```c++
// build segment tree
// param: k -> index of segment node in the array
void build(int left, int right, int k) {
    tree[k].left = left;
    tree[k].right = right;
    tree[k].lazy = 0;
    if (left == right) {
        cin >> tree[k].weight;
        return;
    }
    int mid = (left + right) / 2;
    build(left, mid, k * 2);
    build(mid + 1, right, k * 2 + 1);
    tree[k].weight = tree[k * 2].weight + tree[k * 2 + 1].weight;
}
```

线段树有两种基本操作：区间更新和求区间和。其他操作皆可由求和和更新得到（比如point更新

区间更新
注意在update区间时需要同时更新weight和lazy。lazy是此区间的子区间的延迟更新，不包含当前区间的延迟更新。
```c++
void update(int i, int j, int addition, int k) {
    if (tree[k].left >= i && tree[k].right <= j) {
        tree[k].weight += addition * (tree[k].right - tree[k].left + 1);
        tree[k].lazy += addition;
        return;
    }
    if (tree[k].lazy)
        down(k);
    int mid = (tree[k].left + tree[k].right) / 2;
    if (i <= mid)
        update(i, mid, addition, k * 2);
    if (j > mid)
        update(mid + 1, j, addition, k * 2 + 1);
    tree[k].weight = tree[k * 2].weight + tree[k * 2 + 1].weight;
}
```

区间求和
原理和update类似
```c++
// get the sum of the range
// param: i, j represents interval [i, j]
int sum(int i, int j, int k) {
    if (tree[k].left >= i && tree[k].right <= j)
        return tree[k].weight;
    if (tree[k].lazy)
        down(k);
    int ans = 0;
    int mid = (tree[k].left + tree[k].right) / 2;
    if (i <= mid)
        ans += sum(i, j, k * 2);
    if (j > mid)
        ans += sum(i, j, k * 2 + 1);
    return ans;
}
```

当然，最重要的还是我们的down函数了。down函数使得延迟更新成为可能。
```c++
void down(int k) {
    tree[k * 2].lazy += tree[k].lazy;
    tree[k * 2 + 1].lazy += tree[k].lazy;
    tree[k * 2].weight += tree[k].lazy * (tree[k * 2].right - tree[k * 2].left + 1);
    tree[k * 2 + 1].weight += tree[k].lazy * (tree[k * 2 + 1].right - tree[k * 2 + 1].left + 1);
    tree[k].lazy = 0;
}
```
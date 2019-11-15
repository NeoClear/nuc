---
title: binary indexed tree
date: 2019-11-09 23:48:55
tags:
- binary indexed tree
- fenwick tree
- data structure
---

binary indexed tree，又称fenwick tree，树状数组。是一种动态维护数组前缀和的数据结构。其代码极为简洁。
但是理解概念较难。一般直接背代码。

```c++
class fwt {
private:
    vector<int> sums_;
    // return the lowbit of an integer
    static inline int lowbit(int x) { return x & (-x); }

public:
    // the index of fwt start at 1. Thus, we initialize as n + 1
    fwt(int n): sums_(n + 1, 0) {}
    // id in update increases until exceeds the size of fwt
    void update(int id, int delta) {
        while (id < sums_.size()) {
            sums_[id] += delta;
            id += lowbit(id);
        }
    }
    // id in query decreases until less or equal to 0
    int query(int id) const {
        int ans = 0;
        while (id > 0) {
            ans += sums_[id];
            id -= lowbit(id);
        }
        return ans;
    }
};
```
真的没啥好解释的。。。
注意update中id增加
而query中id减少
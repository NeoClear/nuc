---
title: heap
date: 2019-11-17 11:54:12
tags:
- heap
---

非常简单的数据结构。
唯一复杂的部分是down函数。

```c++
#include <bits/stdc++.h>

typedef long long ll;

using namespace std;

class heap {
public:
    vector<ll> dat;

    heap(): dat(1) {}

    ll up(ll k) {
        if (dat[k / 2] > dat[k]) { swap(dat[k / 2], dat[k]); return 1; }
        return 0;
    }

    bool has_left(ll k) { return k * 2 <= size(); }

    bool has_right(ll k) { return k * 2 + 1 <= size(); }

    ll down(ll k) {
        // return 0 -> no change
        // return -1 -> swap left son
        // return 1 -> swap right son
        if (has_right(k)) {
            if (dat[k] <= min(dat[k * 2], dat[k * 2 + 1])) { return 0; }
            else {
                if (dat[k * 2] < dat[k * 2 + 1]) { swap(dat[k], dat[k * 2]); return -1; }
                else { swap(dat[k], dat[k * 2 + 1]); return 1; }
            }
        } else if (has_left(k) && dat[k * 2] < dat[k]) { swap(dat[k], dat[k * 2]); return -1; }
        return 0;
    }

    void push(ll val) {
        dat.push_back(val);
        ll k = dat.size() - 1;
        while (k != 1 && up(k)) { k /= 2; }
    }

    ll front() { return dat[1]; }

    void pop() {
        swap(dat[1], dat[size()]);
        dat.pop_back();
        ll op, k = 1;
        while ((op = down(k)) != 0) { k = (op == -1 ? 0 : 1) + k * 2; }
    }

    ll size() { return dat.size() - 1; }
};

int main() {
    heap ins;
    int N;
    cin >> N;
    while (N--) { int n; cin >> n; ins.push(n); }
    while (ins.size()) {
        cout << ins.front();
        ins.pop();
    }
    return 0;
}
```
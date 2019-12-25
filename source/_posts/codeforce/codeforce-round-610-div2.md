---
title: codeforce round 610 div2
date: 2019-12-24 16:19:08
categories:
- codeforce
tags:
- codeforce
---

Start the journey on codeforce now

<!--more-->

## A. Temporarily Unavailable

This is a very simple problem, about the geometry

```c++
// c++
#include <iostream>
#include <cstdio>
#include <cctype>
#include <algorithm>
typedef long long ll;
using namespace std;
ll read() {
    ll ans = 0, f = 1; char ch = getchar();
    while (!isdigit(ch)) { if (ch == '-') f = -1; ch = getchar(); }
    while (isdigit(ch)) { ans = ans * 10 + ch - '0'; ch = getchar(); }
    return ans * f;
}
int main() {
    // freopen("in.txt", "r", stdin);
    ll N = read();
    ll a, b, c, r;
    for (ll i  = 0; i < N; i++) {
        a = read(); b = read(); c = read(); r = read();
        ll left = min(a, b), right = max(a, b);
        ll leftr = max(left, min(right, c - r));
        ll rightr = min(right, max(left, c + r));
        printf("%lld\n", (leftr - left) + (right - rightr));
    }
}
```

```python
N = int(input())
for i in range(N):
    a, b, c, r = map(int, input().split())
    left = min(a, b)
    right = max(a, b)
    leftr = max(left, c - r)
    rightr = min(right, c + r)
    print(min((leftr - left) + (right - rightr), right - left))
```

## B1. K for the Price of One (Easy Version)

In this problem K is set as 2. Thus, you can just use 2 array to store even and odd sums to get the answer

```c++
#include <iostream>
#include <cstdio>
#include <cctype>
#include <algorithm>
#include <cstring>

typedef long long ll;

using namespace std;

ll read() {
    ll ans = 0, f = 1; char ch = getchar();
    while (!isdigit(ch)) { if (ch == '-') f = -1; ch = getchar(); }
    while (isdigit(ch)) { ans = ans * 10 + ch - '0'; ch = getchar(); }
    return ans * f;
}

ll store[200010];

int main() {
    // freopen("in.txt", "r", stdin);
    ll N = read();
    for (ll i = 0; i < N; i++) {
        ll n = read(), p = read(), k = read();
        for (ll j = 0; j < n; j++) { store[j] = read(); }
        sort(store, store + n);
        ll j = 0;
        ll acc[2];
        memset(acc, 0, sizeof(acc));
        for (j = 0; j < n; j++) {
            acc[j % 2] += store[j];
            if (acc[j % 2] > p)
                break;
        }
        printf("%lld\n", j);
    }
    return 0;
}
```

## B2. K for the Price of One (Hard Version)

It is the harder version of B1. You can just see it as a very simple one dimensional dp problem

```c++
#include <iostream>
#include <cstdio>
#include <cctype>
#include <algorithm>
#include <cstring>

#define SIZE 200010
#define inf 0xffffffff

typedef long long ll;

using namespace std;

ll read() {
    ll ans = 0, f = 1; char ch = getchar();
    while (!isdigit(ch)) { if (ch == '-') f = -1; ch = getchar(); }
    while (isdigit(ch)) { ans = ans * 10 + ch - '0'; ch = getchar(); }
    return ans * f;
}

ll item[SIZE];
ll acc[SIZE];

int main() {
    // freopen("in.txt", "r", stdin);
    ll N = read();
    for (ll i = 0; i < N; i++) {
        ll n = read(), p = read(), k = read();
        for (ll t = 0; t < n; t++) { item[t] = read(); }
        sort(item, item + n);
        memset(acc, 0, sizeof(acc));
        ll j;
        ll flag = -1;
        bool go = true;
        for (j = 0; j < n; j++) {
            ll acc1 = (j < 0 ? 0 : acc[j - 1]) + item[j];
            ll acc2 = j < k - 1 ? inf : (j < k ? 0 : acc[j - k]) + item[j];
            acc[j] = min(acc1, acc2);
            // cout << acc[j] << endl;
            if (acc[j] <= p) {
                flag = j;
            }
        }
        printf("%lld\n", flag + 1);
    }
    return 0;
}
```
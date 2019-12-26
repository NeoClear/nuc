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

## C. Petya and Exam

The code is actually not mine. (I failed to solve it using my own method)

It sort problems first, then iterate these problems to get the answer
First it calculate the remaining easy probs & hard probs, and add it to the current solved problems
If there are times remained, we update the answer

```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <algorithm>
#include <cstring>

using namespace std;
 
int m;
int n, t, a[2];
pair<int, int> p[200005];
 
int main(){
	scanf("%d", &m);
	while(m--) {
		scanf("%d%d%d%d", &n, &t, &a[0], &a[1]);
		for(int i=1;i<=n;++i) scanf("%d", &p[i].second);
		for(int i=1;i<=n;++i) scanf("%d", &p[i].first);
		int b[2] = {};
		for(int i=1;i<=n;++i) b[p[i].second]++;
		int c[2] = {};
		sort(p+1, p+n+1);
		int s = 0;
		int ans = 0;
		for(int i=1;i<=n;++i) {
			int k = min(b[0]-c[0], (p[i].first - s - 1)/a[0]);
			int l = min(b[1]-c[1], (p[i].first - s - a[0]*k - 1)/a[1]);
			k = max(0, k);
			l = max(0, l);
			if(p[i].first > s) ans = max(ans, k+l+i-1);
			s += a[p[i].second];
			c[p[i].second]++;
			if(s > t) break;
			if(i==n) ans = n;
		}
		printf("%d\n", ans);
	}
}
```


## D. Enchanted Artifact

This is a new kind pf problem. In traditional problems, the input is always fixed. However, for interactive problems, the input actually reflects our output. We output queries to stdout, and get results from stdin. Although this problem is new, it is not difficult

We can just use two queries to determine the number of as and bs in the spell. Next, we use n-1 iterations to determine the character at each index. Since the query limit is n + 2, we cannot query the last index. However, we can determine the character at the last index by examine the final value of **diff** (a variable describes the current remaining difference)

```c++
#include <iostream>
#include <cstring>
#include <cstdio>
#include <cctype>
#include <algorithm>
#include <cstdlib>

#define N 300

typedef long long ll;

using namespace std;

ll interact(string w) {
    cout << w << endl;
    int diff;
    cin >> diff;
    if (diff)
        return diff;
    exit(0);
}

// diff_a == 300 - a
// diff_b == 300 - b

int main() {
    ll a_num, b_num, n;
    // calculate number of as and bs in the spells
    a_num = N - interact(string(300, 'a'));
    b_num = N - interact(string(300, 'b'));
    n = a_num + b_num;

    string s(n, 'a');
    ll diff = b_num;

    for (ll i = 0; i < n - 1; i++) {
        s[i] = 'b';
        if (interact(s) > diff)
            s[i] = 'a';
        else
            diff--;
    }
    if (diff)
        s.back() = 'b';
    interact(s);
    return 0;
}
```
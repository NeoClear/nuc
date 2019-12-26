---
title: c++ silver bullet
date: 2019-12-19 12:40:23
categories:
- silver bullet
tags:
- c++
- silver bullet
---

## std

翻转一个vector

<!--more-->

```c++
vector<int> t = {1, 2, 3, 4, 5};
reverse(t.begin(), t.end());
for (const auto& it : t)
    cout << it << " ";
// it will print 5 4 3 2 1
```

remove函数
先放源码
```c++
template <class ForwardIterator, class T>
  ForwardIterator remove (ForwardIterator first, ForwardIterator last, const T& val)
{
  ForwardIterator result = first;
  while (first!=last) {
    if (!(*first == val)) {
      *result = move(*first);
      ++result;
    }
    ++first;
  }
  return result;
}
```

大意就是将区间内与val相同的value放到区间的最后，并返回第一个值为val的地址。

tie函数
更加方便将tuple的值直接assign到多个变量。

```c++
queue<tuple<int, int, int>> q;
int tx, ty, to;
tie(tx, ty, to) = q.front(); q.pop();
```

## string
how to get a slice of a string?
use substr method call.
```c++
s.substr(i, length)
```

i indicates the index of first character and length stands for the length of the slice.

## Useful Functions

### memset

```c++
#include <cstring>
memset(addr, value, sizeof(addr));
```

### isdigit

```c++
#include <cctype>
if(isdigit(ch)) {}
```

### sort

```c++
#include <algorithm>
sort(dat, dat + n);
```

## Calculation

### Not (!)

If you apply **!** to an integer, then 0 would be 1, and others would be 0

### array & structure initialization

If you created an array like

```c++
int t[SIZE];
```

Then elements in t are random values. However, if you use this

```c++
int t[SIZE] = {};
```

These elements would be initialized as 0

However, you can only do this at its generation. If you created it then assign it, it would thow an compile error

```c++
// Wrong!!!
int t[SIZE];
t = {};
```

But structures are different, you can assign it after it is defined

```c++
struct str {
  int a, b;
};

str t;
t = {2, 3};
```
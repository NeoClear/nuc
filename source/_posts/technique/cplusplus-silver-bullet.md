---
title: c++ silver bullet
date: 2019-12-19 12:40:23
tags:
- c++
---

## std

翻转一个vector

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
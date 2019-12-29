---
title: educational codeforce round 79 div2
date: 2019-12-28 19:07:39
categories:
- codeforce
tags:
- codeforce
---

没写出来的题就不放了（说到底还是懒）

<!--more-->

## A. New Year Garland

Very simple math problem, just make a compare

```python
t = int(input())

for i in range(t):
    colors = list(map(int, input().split()))
    print("No" if max(colors) * 2 > sum(colors) + 1 else "Yes")
```

## B. Verse For Santa

Very simple one dimentional dp

```python
t = int(input())

for p in range(t):
    n, s = map(int, input().split())
    verse = list(map(int, input().split()))
    if sum(verse) <= s:
        print(0)
        continue
    ans = -1
    max_index = 0
    sums = 0
    for i in range(len(verse)):
        sums += verse[i]
        if verse[i] > verse[max_index]:
            max_index = i
        if sums - verse[max_index] <= s:
            ans = max_index
        else:
            break
    print(ans + 1)
```

## C. Stack of Presents

Simple gready problem
presents which is shallower than the current deepest present takes only 1 to get
presents which is deeper than the current deepest present takes (depth - presents already get) * 2 + 1 steps to get

```python
t = int(input())

for p in range(t):
    n, m = map(int, input().split())
    stack = list(map(int, input().split()))
    send = list(map(int, input().split()))
    depth = [0] * (n + 1)
    for i in range(n):
        depth[stack[i]] = i
    ans = 0
    max_dep = -1
    for i in range(m):
        gift = send[i]
        if depth[gift] > max_dep:
            max_dep = depth[gift]
            ans += (max_dep - i) * 2 + 1
        else:
            ans += 1
    print(ans)
```
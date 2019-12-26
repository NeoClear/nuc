---
title: codeforce round 609 div1
date: 2019-12-26 01:24:44
categories:
- codeforce
tags:
- codeforce
- math
---

Interesting math problems

<!--more-->

## A. Long Beautiful Integer

The output of this problem is duplicate. The first line is always the same as the first integer of input

However, my code is tooooooooooo duplicated. It can be cleaner

```python
from typing import List

def expand(rep: List[int], digits: int) -> List[int]:
    pattern = []
    pattern.extend(rep)
    k = len(rep)
    while len(pattern) < digits:
        pattern.append(pattern[-k])
    return pattern

def inc(pattern: List[int]) -> None:
    pattern[-1] += 1
    d = 1
    while pattern[-d] == 10:
        pattern[-d] = 0
        pattern[-d - 1] += 1
        d += 1

n, k = map(int, input().split())
y = input().strip("\n")
number = []
for d in y:
    number.append(int(d))

pat = number[:min(k, len(number))]

while expand(pat, len(number)) < number:
    inc(pat)

ans = ""
for d in expand(pat, len(number)):
    ans += str(d)
print(len(ans))
print(ans)
```

## B. Domino for Young

This is a very interesting math problem
Some people think it is a dp problem at their first glance. However, it is actually math

It can be interpreted as dying blocks into black & white, and the answer is the minimum of blacks and whites

```python
n = int(input())
domino = list(map(int, input().strip().split()))

# dye[0] means black, dye[1] means white
dye = [0] * 2

for i in range(len(domino)):
    dye[i % 2] += domino[i] // 2
    dye[(i + 1) % 2] += (domino[i] + 1) // 2

print(min(dye))
```
---
title: codeforce round 611 div3
date: 2019-12-28 17:54:47
categories:
- codeforce
tags:
- codeforce
---

本此codeforce round其实真的不难。。。为啥我只做出了三题呢？？？

<!--more-->

## A. Minutes Before the New Year

Extremely easy problem

```python
t = int(input())

for _ in range(t):
    h, m = map(int, input().split())
    print((24 - h) * 60 - m)
```

## B. Candies Division

Very simple math problem
Just use division principles

```python
t = int(input())

for _ in range(t):
    n, k = map(int, input().split())
    a = n // k
    print(min(a * k + k // 2, n))
```

## C. Friends and Gifts

The optimal solution is to use a queue to keep track of people who have sent gifts
If a person encountered himself to send gift, he could just switch with a person who have already sent the gift, because two people who would receive gifts from these two people must be different and will not be the same person
My solution is a bull shit

```c++
#include <iostream>
#include <unordered_set>
#include <vector>

using namespace std;

int N;
int friends[200010];

int main() {
    cin >> N;
    for (int i = 0; i < N; i++)
        cin >> friends[i];
    unordered_set<int> connect;
    unordered_set<int> available;
    for (int i = 1; i <= N; i++) { available.insert(i); }
    for (int i = 0; i < N; i++) {
        if (friends[i] == 0)
            connect.insert(i + 1);
        else
            available.erase(friends[i]);
    }
    vector<int> same, diff, wait;
    for (const auto& it : connect) {
        if (available.count(it))
            same.push_back(it);
        else
            diff.push_back(it);
    }
    
    for (const auto& it : available) {
        if (!connect.count(it))
            wait.push_back(it);
    }


    for (int i = 0; i < same.size(); i++) {
        friends[same[i] - 1] = i == 0 ? same.back() : same[i - 1];
    }
    for (int i = 0; i < diff.size(); i++) {
        friends[diff[i] - 1] = wait[i];
    }
    if (same.size() == 1) {
        swap(friends[same[0] - 1], friends[diff[0] - 1]);
    }
    for (int i = 0; i < N; i++)
        cout << friends[i] << " ";
    return 0;
}
```
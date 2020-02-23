---
title: leetcode weekly contest 176
date: 2020-02-23 02:02:43
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

This week's contest is EXTREMELY difficult!!!

<!--more-->

## Count Negative Numbers in a Sorted Matrix

Simple

```c++
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        int cnt = 0;
        for (const auto& line : grid)
            for (const auto& ele : line)
                cnt += ele < 0 ? 1 : 0;
        return cnt;
    }
};
```

## Product of the Last K Numbers

Be careful with 0s

```c++
class ProductOfNumbers {
private:
    vector<int> product;
    set<int> isZero;
public:

    ProductOfNumbers() {
        product.push_back(1);
    }
    
    void add(int num) {
        if (num == 0)
            isZero.insert(product.size());
        product.push_back(num == 0 ? 1 : product.back() * num);
    }
    
    int getProduct(int k) {
        if (isZero.upper_bound(product.size() - 1 - k) != isZero.end())
            return 0;
        return product.back() / product[product.size() - k - 1];
    }
};
```

## Maximum Number of Events That Can Be Attended

Just a sliding window. Tough though

```c++
class Solution {
public:
    int maxEvents(vector<vector<int>>& events) {
        sort(events.begin(), events.end(), [](auto& a, auto& b) {
            return a.front() < b.front();
        });
        int pos = 0, ans = 0;
        priority_queue<int, vector<int>, greater<int>> q;
        for (int d = 1; d <= 100000; d++) {
            while (pos < events.size() && events[pos].front() == d)
                q.push(events[pos++].back());
            while (!q.empty() && q.top() < d)
                q.pop();
            if (!q.empty()) {
                q.pop();
                ans++;
            }
        }
        return ans;
    }
};
```

## Construct Target Array With Multiple Sums

Reverse engineering. Concept is simple

```c++
class Solution {
public:
    bool isPossible(vector<int>& target) {
        if (target.size() == 1)
            return target.front() == 1;
        multiset<long long> arr;
        for (const auto& it : target)
            arr.insert(it);
        
        long long sum = accumulate(arr.begin(), arr.end(), 0);
        while (sum > target.size()) {
            long long largest = *arr.rbegin();
            long long others = sum - largest;
            if (largest <= others)
                return false;
            arr.erase(largest);
            long long val = largest % others == 0 ? others : largest % others;
            arr.insert(val);
            sum -= largest - val;
        }
        return true;
    }
};
```
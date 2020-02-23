---
title: leetcode weekly contest 177
date: 2020-02-23 02:02:57
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

Make two stupid approaches. Tested on samples but didn't notice the obvious error

<!--more-->

## Number of Days Between Two Dates

Simple concept but difficult implementation

```c++
class Solution {
public:
    void parse(string format, vector<int>& day) {
        day[0] = stoi(format.substr(0, 4));
        day[1] = stoi(format.substr(5, 2));
        day[2] = stoi(format.substr(8, 2));
    }
    int getDay(int year, int month) {
        switch (month) {
            case 1:
            case 3:
            case 5:
            case 7:
            case 8:
            case 10:
            case 12:
                return 31;
            case 4:
            case 6:
            case 9:
            case 11:
                return 30;
            default:
                if (year % 4 == 0) {
                    if (year % 100 == 0) {
                        if (year % 400 == 0)
                            return 29;
                        return 28;
                    }
                    return 29;
                }
                return 28;
        }
    }
    int calc(vector<int> day1, vector<int> day2) {
        if (day1[0] == day2[0] && day1[1] == day2[1])
            return day2[2] - day1[2];
        if (day1 > day2)
            return -calc(day2, day1);
        return getDay(day1[0], day1[1]) + calc({day1[1] == 12 ? day1[0] + 1 : day1[0], day1[1] == 12 ? 1 : day1[1] + 1, day1[2]}, day2);
    }
    int daysBetweenDates(string date1, string date2) {
        vector<int> day1(3), day2(3);
        parse(date1, day1);
        parse(date2, day2);
        return abs(calc(day1, day2));
    }
};
```

## Validate Binary Tree Nodes

Simple tree problem. The feature of binary tree

```c++
class Solution {
public:
    bool validateBinaryTreeNodes(int n, vector<int>& leftChild, vector<int>& rightChild) {
        unordered_set<int> isChild;
        for (const auto& it : leftChild) {
            if (it == -1)
                continue;
            if (isChild.count(it))
                return false;
            isChild.insert(it);
        }
        for (const auto& it : rightChild) {
            if (it == -1)
                continue;
            if (isChild.count(it))
                return false;
            isChild.insert(it);
        }
        return isChild.size() == n - 1;
    }
};
```

## Closest Divisors

Loop over

```c++
class Solution {
public:
    int diff(vector<int>& t) {
        return abs(t.front() - t.back());
    }
    vector<int> closestDivisors(int num) {
        vector<int> ans = {1, num + 1};
        for (int i = 1; i <= sqrt(num + 2); i++) {
            if ((num + 1) % i == 0) {
                if (abs(i - (num + 1) / i) < diff(ans))
                    ans = {i, (num + 1) / i};
            }
            if ((num + 2) % i == 0) {
                if (abs(i - (num + 2) / i) < diff(ans))
                    ans = {i, (num + 2) / i};
            }
        }
        return ans;
    }
};
```

## Largest Multiple of Three

If a number is multiple of three, then the sum of its digits is the multiple of three (Math prob)

```c++
class Solution {
public:
    string largestMultipleOfThree(vector<int>& digits) {
        sort(digits.begin(), digits.end(), [](auto& a, auto& b) {
            return a > b;
        });
        if (digits.front() == 0)
            return "0";
        vector<int> rem0, rem1, rem2;
        for (const auto& it : digits) {
            if (it % 3 == 0)
                rem0.push_back(it);
            else if (it % 3 == 1)
                rem1.push_back(it);
            else
                rem2.push_back(it);
        }
        string ans;
        for (const auto& it : rem0)
            ans.push_back(it + '0');
        if (rem1.size() > rem2.size())
            rem1.swap(rem2);
        if ((rem2.size() - rem1.size()) % 3 == 1)
            rem2.pop_back();
        else if ((rem2.size() - rem1.size()) % 3 == 2) {
            if (!rem1.empty())
                rem1.pop_back();
            else {
                rem2.pop_back();
                rem2.pop_back();
            }
        }
        for (const auto& it : rem1) {
            ans.push_back(it + '0');
        }
        for (const auto& it : rem2) {
            ans.push_back(it + '0');
        }
        sort(ans.begin(), ans.end(), [](auto& a, auto& b) {
            return a > b;
        });
        if (!ans.empty() && ans.front() == '0')
            return "0";
        return ans;
    }
};
```

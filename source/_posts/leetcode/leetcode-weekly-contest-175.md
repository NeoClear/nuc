---
title: leetcode weekly contest 175
date: 2020-02-16 20:32:17
categories:
- leetcode
tags:
- leetcode
- weekly contest
---

The experience is horrible. The testcase of third prob is wrong, and it will give wrong output of example testcase. And the third prob is difficult too

<!--more-->

## Check If N and Its Double Exist
Simple brute force

```c++
class Solution {
public:
    bool checkIfExist(vector<int>& arr) {
        int cnt = 0;
        unordered_set<int> visited(arr.begin(), arr.end());
        for (const auto& it : arr)
            if (it == 0) { cnt++; }
        if (cnt >= 2) { return true; }
        for (const auto& it : arr)
            if (visited.count(it * 2) && it != 0)
                return true;
        return false;
    }
};
```

## Minimum Number of Steps to Make Two Strings Anagram
Simple arithmetic
```c++
class Solution {
public:
    int minSteps(string s, string t) {
        vector<pair<int, int>> count(26);
        for (int i = 0; i < s.length(); i++) {
            count[s[i] - 'a'].first++;
            count[t[i] - 'a'].second++;
        }
        int ans = 0;
        for (int i = 0; i < 26; i++)
            ans += abs(count[i].first - count[i].second);
        return ans >> 1;
    }
};
```

## Tweet Counts Per Frequency
Difficult arithmetic

```c++
class TweetCounts {
private:
    unordered_map<string, multiset<int>> tweet;
public:
    TweetCounts() {
    }
    
    void recordTweet(string tweetName, int time) {
        tweet[tweetName].insert(time);
    }
    
    vector<int> getTweetCountsPerFrequency(string freq, string tweetName, int startTime, int endTime) {
        int interval;
        if (freq == "minute")
            interval = 60;
        else if (freq == "hour")
            interval = 3600;
        else
            interval = 3600 * 24;
        if (!tweet.count(tweetName)) { return {}; }
        vector<int> time;
        for (auto it = tweet[tweetName].lower_bound(startTime); it != tweet[tweetName].end() && *it <= endTime; it++)
            time.push_back(*it);
        vector<int> ans;
        int id = 0;
        for (int begin = startTime; begin <= endTime; begin += interval) {
            int cnt = 0;
            while (id < time.size() && time[id] < min(begin + interval, endTime + 1)) {
                cnt++;
                id++;
            }
            ans.push_back(cnt);
        }
        return ans;
    }
};
```

## Maximum Students Taking Exam
Difficult dp with bit operation. Use int value to store the status of rows.
dp[r][s] means the maximum students can be placed in r rows with the rth status s

```c++
class Solution {
public:
    int maxStudents(vector<vector<char>>& seats) {
        int row = seats.size(), col = seats[0].size();
        vector<vector<int>> dp(row + 1, vector<int>(1 << col, 0));
        for (int r = 1; r <= row; r++) {
            // the situation of rth row
            for (int prev = 0; prev < (1 << col); prev++) {
                // the status of prev row
                for (int cur = 0; cur < (1 << col); cur++) {
                    // the status of current row
                    bool valid = true;
                    for (int bit = 0; bit < col && valid; bit++) {
                        if (!(cur & (1 << bit))) continue;
                        if (seats[r - 1][bit] == '#') valid = false;
                        bool l = (bit == 0 ? 0 : (cur & (1 << (bit - 1))));
                        bool r = (bit == col - 1 ? 0 : (cur & (1 << (bit + 1))));
                        bool ul = (bit == 0 ? 0 : (prev & (1 << (bit - 1))));
                        bool ur = (bit == col - 1 ? 0 : (prev & (1 << (bit + 1))));
                        if (l || r || ul || ur) valid = false;
                    }
                    if (valid)
                        dp[r][cur] = max(dp[r][cur], dp[r - 1][prev] + __builtin_popcount(cur));
                }
            }
        }
        return *max_element(dp[row].begin(), dp[row].end());
    }
};
```

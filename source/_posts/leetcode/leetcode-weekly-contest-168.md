---
title: leetcode weekly contest 168
date: 2019-12-22 15:16:34
tags:
- leetcode
- weekly contest
---

Now i tried to use python to solve these weekly contests... An awesome approach.
However, c++ is still a very powerful programing language, use it!

If interested, you can try other languages like kotlin, go, etc.

<!--more-->

## 1295. Find Numbers with Even Number of Digits

```python
# python3
class Solution:
    def is_even(self, num: int) -> bool:
        num = abs(num)
        cnt = 0
        while num != 0:
            num //= 10
            cnt += 1
        return cnt % 2 == 0

    def findNumbers(self, nums: List[int]) -> int:
        cnt = 0
        for it in nums:
            if self.is_even(it):
                cnt += 1
        return cnt
```

```java
// java
class Solution {
    public boolean is_even(int n) {
        int cnt = 0;
        for (; n != 0; n /= 10, cnt++) {}
        return cnt % 2 == 0;
    }
    public int findNumbers(int[] nums) {
        int cnt = 0;
        for (int n : nums)
            if (is_even(n))
                cnt++;
        return cnt;
    }
}
```

```c++
// c++
class Solution {
public:
    bool is_even(int n) {
        int cnt = 0;
        for (; n; n /= 10, cnt++) {}
        return cnt % 2 == 0;
    }
    int findNumbers(vector<int>& nums) {
        int cnt = 0;
        for (const auto& it : nums) {
            if (is_even(it)) { cnt++; }
        }
        return cnt;
    }
};
```

## 1296. Divide Array in Sets of K Consecutive Numbers

```python
# python
from typing import List
from collections import Counter
class Solution:
    def isPossibleDivide(self, nums: List[int], k: int) -> bool:
        cnt = Counter(nums)
        keys = sorted(cnt.keys())
        for n in keys:
            if cnt[n] > 0:
                minus = cnt[n]
                for i in range(n, n + k):
                    if cnt[i] < minus:
                        return False
                    cnt[i] -= minus
        return True
```

```c++
// c++
class Solution {
public:
    bool isPossibleDivide(vector<int>& nums, int k) {
        unordered_map<int, int> count;
        set<int> keys;
        for (const auto& it : nums) { count[it]++; }
        for (const auto& it : nums) { keys.insert(it); }
        for (const auto& it : keys) {
            if (count[it] == 0) { continue; }
            int minus = count[it];
            for (int i = it; i < it + k; i++) {
                if (count[i] < minus) { return false; }
                count[i] -= minus;
            }
        }
        return true;
    }
};
```

```java
// java
class Solution {
    public boolean isPossibleDivide(int[] nums, int k) {
        HashMap<Integer, Integer> count = new HashMap<Integer, Integer>();
        TreeSet<Integer> keys = new TreeSet<>();
        for (int n : nums) {
            count.put(n, count.getOrDefault(n, 0) + 1);
            keys.add(n);
        }
        for (int n : keys) {
            if (count.getOrDefault(n, 0) == 0)
                continue;
            int minus = count.get(n);
            for (int i = n; i < n + k; i++) {
                if (count.getOrDefault(i, 0) < minus)
                    return false;
                count.put(i, count.get(i) - minus);
            }
        }
        return true;
    }
}
```

## 1297. Maximum Number of Occurrences of a Substring

```python
# python
from typing import List
import collections
class Solution:
    def maxFreq(self, s: str, maxLetters: int, minSize: int, maxSize: int) -> int:
        cnt = {}
        for b in range(len(s) - minSize + 1):
            w = s[b: b + minSize]
            if len(collections.Counter(w)) > maxLetters:
                continue
            if w not in cnt:
                cnt[w] = 0
            cnt[w] += 1
        return max(cnt.values()) if len(cnt) != 0 else 0
```

```c++
// c++
class Solution {
public:
    int maxFreq(string s, int maxLetters, int minSize, int maxSize) {
        unordered_map<string, int> occur;
        for (int i = 0; i < s.length() - minSize + 1; i++) {
            string w = s.substr(i, minSize);
            unordered_set<char> cnt(w.begin(), w.end());
            if (cnt.size() <= maxLetters) { occur[s.substr(i, minSize)]++; }
        }
        int ans = 0;
        for (const auto& kv : occur) {
            ans = max(ans, kv.second);
        }
        return ans;
    }
};
```

```java
// java
class Solution {
    public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        HashMap<String, Integer> count = new HashMap<>();
        for (int i = 0; i < s.length() - minSize + 1; i++) {
            String w = s.substring(i, i + minSize);
            HashSet<Character> cnt = new HashSet<Character>();
            for (char c : w.toCharArray())
                cnt.add(c);
            if (cnt.size() > maxLetters)
                continue;
            count.put(w, count.getOrDefault(w, 0) + 1);
        }
        int mm = 0;
        for (int i : count.values())
            mm = Math.max(mm, i);
        return mm;
    }
}
```

## 1298. Maximum Candies You Can Get from Boxes
```python
from typing import List
from collections import Counter
class Solution:
    def maxCandies(self, status: List[int], candies: List[int], keys: List[List[int]], containedBoxes: List[List[int]], initialBoxes: List[int]) -> int:
        cnt = 0
        size = len(status)
        key = [0] * size
        visiting = Counter(initialBoxes)
        visited = Counter()
        changed = True
        while changed:
            changed = False
            remove = []
            insert = []
            for box in visiting:
                if status[box] == 1 or key[box] == 1:
                    changed = True
                    cnt += candies[box]
                    remove.append(box)
                    visited[box] = 1
                    for dest in containedBoxes[box]:
                        if dest not in visited:
                            insert.append(dest)
                    for k in keys[box]:
                        key[k] = 1
            for b in remove:
                del visiting[b]
            for b in insert:
                visiting[b] = 1
        return cnt
```

```c++
// c++
class Solution {
public:
    int maxCandies(vector<int>& status, vector<int>& candies, vector<vector<int>>& keys, vector<vector<int>>& containedBoxes, vector<int>& initialBoxes) {
        int size = status.size();
        unordered_set<int> visiting(initialBoxes.begin(), initialBoxes.end());
        unordered_set<int> visited;
        unordered_set<int> has_key;
        int cnt = 0;
        bool changed = true;
        while (changed) {
            changed = false;
            vector<int> insert, remove;
            for (const auto& box : visiting) {
                if (status[box] || has_key.count(box)) {
                    changed = true;
                    cnt += candies[box];
                    remove.push_back(box);
                    visited.insert(box);
                    for (const auto& k : keys[box])
                        has_key.insert(k);
                    for (const auto& dest : containedBoxes[box])
                        if (!visited.count(dest))
                            insert.push_back(dest);
                }
            }
            for (const auto& it : insert)
                visiting.insert(it);
            for (const auto& it : remove)
                visiting.erase(it);
        }
        return cnt;
    }
};
```

```java
// java
class Solution {
    public int maxCandies(int[] status, int[] candies, int[][] keys, int[][] containedBoxes, int[] initialBoxes) {
        int cnt = 0;
        HashSet<Integer> hasKey = new HashSet<>();
        HashSet<Integer> visiting = new HashSet<>();
        HashSet<Integer> visited = new HashSet<>();
        // Initialize visiting
        for (int i : initialBoxes)
            visiting.add(i);
        boolean changed = true;
        while (changed) {
            changed = false;
            HashSet<Integer> insert = new HashSet<>();
            HashSet<Integer> remove = new HashSet<>();
            for (int box : visiting) {
                if (status[box] == 1 || hasKey.contains(box)) {
                    cnt += candies[box];
                    changed = true;
                    visited.add(box);
                    remove.add(box);
                    for (int dest : containedBoxes[box])
                        insert.add(dest);
                    for (int k : keys[box])
                        hasKey.add((k));
                }
            }
            for (int it : remove)
                visiting.remove(it);
            for (int it : insert)
                if (!visited.contains(it))
                    visiting.add(it);
        }
        return cnt;
    }
}
```
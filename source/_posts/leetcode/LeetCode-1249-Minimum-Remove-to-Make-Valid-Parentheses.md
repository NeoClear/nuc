---
title: LeetCode 1249 Minimum Remove to Make Valid Parentheses
date: 2019-11-03 12:18:58
tags:
- leetcode
- stack
- stl
---

这是一道非常简单的stack题，当时没想法完全事因为自己菜。。。
思路：
    遇到'('就将index入栈，遇到')'时，假如stack为空，就记录当前index；假如不为空，就pop。
    全部完成后剩下的index为需要去除的parens。

<!--more-->

```c++
class Solution {
public:
    // Actually simple
    string minRemoveToMakeValid(string s) {
        vector<int> v;
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '(')
                v.push_back(i);
            else if (s[i] == ')') {
                if (v.empty())
                    s[i] = '*';
                else
                    v.pop_back();
            }
        }
        while (!v.empty()) {
            s[v.back()] = '*';
            v.pop_back();
        }
        // Learn remove function
        s.erase(remove(s.begin(), s.end(), '*'), s.end());
        return s;
    }
};
```

这里原作者使用了一个remove函数。我们看下这个函数的源代码
```c++
template <class ForwardIterator, class T>
ForwardIterator remove (ForwardIterator first, ForwardIterator last, const T& val)
{
    ForwardIterator result = first;
    while (first!=last) {
        if (!(*first == val)) {
            *result = *first;
            ++result;
        }
        ++first;
    }
    return result;
}
```
这段代码改变了原数组。
与参数匹配的block将被覆盖，并返回需要被删除的区块的head指针。

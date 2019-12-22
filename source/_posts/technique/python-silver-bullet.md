---
title: python silver bullet
date: 2019-12-22 04:16:11
tags:
- python
---

## collections

### Counter

It is a class that is similar to unordered_map in c++. It is a subclass of dict. When there is no such key in Counter, it will return a 0 (just like unordered_map in c++). Let's see its method calls.

```python
t = Counter("234u934979282fehgre")
t = Counter([431, 432, 543, 543, 543, 5423, 6532])
t = Counter({"inno": 999})
```

if you want to get its elements with duplications, you can do as follows.

```python
t = Counter("innovation")
t.elements()
```

and it will return an itertool.chain object

and for its values, you can use

```python
t = Counter("innovation")
t.values()
```

and it will return an dict_value object (just like dict.values())

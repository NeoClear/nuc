---
title: modern c++
date: 2020-02-23 22:50:48
categories:
- programing languages
tags:
- c++
---

learn some modern features of c++

TO BE HONEST, modern c++ is TOOOO complex for normal users

<!--more-->

constexpr means a function or statement can be evaluated in compile

```c++
template<typename T, size_t N>
constexpr size_t arraySize(T (&)[N]) noexcept {
    return N;
}
```


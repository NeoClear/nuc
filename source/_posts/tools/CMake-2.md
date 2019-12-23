---
title: CMake-2
date: 2019-11-18 20:48:22
tags:
- cmake
- build
---

This is the second part of CMake Tutorial.

In this part we mainly focus on how to write a script to install our software.

<!--more-->

we just need write like this.

```cmake
# CMakeLists.txt
install(TARGETS power DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/config.h"
        DESTINATION include)
```

```cmake
# math/CMakeLists.txt
install(TARGETS math_functions DESTINATION bin)
install(FILES power.h DESTINATION include)
```

Very simple right?
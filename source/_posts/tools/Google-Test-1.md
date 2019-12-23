---
title: Google Test-1
date: 2019-11-19 15:25:29
tags:
- gtest
- Google Test
- cmake
---

Google Test

<!--more-->

CMakeLists大概长这样：
```c++
cmake_minimum_required(VERSION 2.8)

project(gtest)

find_package(GTest REQUIRED)
find_package(Threads REQUIRED)

include_directories(${GTEST_INCLUDE_DIRS})

add_executable(MyTest test.cpp)
target_link_libraries(MyTest ${GTEST_BOTH_LIBRARIES})
target_link_libraries(MyTest ${CMAKE_THREAD_LIBS_INIT})

add_test(Test MyTest)
enable_testing()
```

```c++
#include <gtest/gtest.h>
#include <numeric>
#include <vector>

TEST(MyTest, Sum)
{
    std::vector<int> vec{1, 2, 3, 4, 5};
    int sum = std::accumulate(vec.begin(), vec.end(), 0);
    EXPECT_EQ(sum, 15);
}
int main(int argc, char *argv[])
{
    testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```

其中TEST宏的第一个参数是Test Suit，而第二个参数是Test Case。
最后make test就可以单元测试了。
说真的，这些代码，根本记不住啊。。。要用的时候直接拷贝吧。

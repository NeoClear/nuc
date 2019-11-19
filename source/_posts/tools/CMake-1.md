---
title: CMake-1
date: 2019-11-18 17:30:57
tags:
- cmake
- build
---

It is wired to type purely English...
But it is difficult to type Chinese in linux, so...

Today we will take a look at CMake, a cross-platform build tool.
With the help of this tool, it is simpler to port your project into different operating systems like Windows and OS-X.

This document is written based on other blogs.

## Case 1: single file

```cmake
cmake_minimum_required(VERSION 2.8)

project(math)

add_executable(power main.cpp)
```

In this case, CMake will generate a makefile(on linux) that generates a program called power given main.cpp. You do not have to consider the detail of compile process. Just one line.

Moreover, you can set up the version of the current project by doing the following:

```cmake
project(math VERSION 1.0)
```

## Case 2: multiple files

If we want to compile all files in the current directory to a program, we can use the following command to store the name of all files into a variable, and then use it in the process.

```cmake
aux_source_directory(. SRC)

add_executable(math ${SRC})
```

## Case 3: multiple files in multiple dirs

CMake makes it easier to keep the project organized.
Here is an example of how CMakeLists.txt should be in a multi-dir project.

```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 2.8)

project(power)

# Go to the subdir and manage files there
add_subdirectory(math)

add_executable(power main.cpp)

# link library to the executable file
target_link_libraries(power math_functions)
```

```cmake
# math/CMakeLists.txt
aux_source_directory(. LIB_MATH)

add_library(math_functions ${LIB_MATH})
```

## Case 4: customize compiling process

To be honest, CMake is as difficult as makefile. There are endless commands.

Sometimes we want to use different libraries based on different macros we define. Thus, we have:

```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 2.8)

project(power)

# Determine configure file
configure_file(
    "${PROJECT_SOURCE_DIR}/config.h.in"
    "${PROJECT_BINARY_DIR}/config.h"
)

# Set the macro value
option(USE_MATH
       "Use provided math library" ON)

# If true, then add library and add subdirectory
if(USE_MATH)
    include_directories("${PROJECT_SOURCE_DIR}/math")
    add_subdirectory(math)
    set(LIBS ${LIBS} math_functions)
endif(USE_MATH)

add_executable(power main.cpp)

target_link_libraries(power ${LIBS})
```

```cmake
# math/CMakeLists.txt
aux_source_directory(. LIB_MATH)

add_library(math_functions ${LIB_MATH})
```

Also we need to have a config.h.in file to generate config.h
There are several circumstances.
If:

```in
#cmakedefine USE_MATH
```

then the generated config.h will be

```c++
#define USE_MATH
```

If:

```in
#cmakedefine USE_MATH "2333"
```

then the generated config.h will be

```c++
#define USE_MATH "2333"
```

If:

```in
#cmakedefine USE_MATH "@USE_MATH@"
```

then the generated config.h will be

```c++
#define USE_MATH "ON"
```

We end here today
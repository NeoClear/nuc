---
title: opengl coordinate 2
date: 2019-11-30 11:11:02
tags:
- opengl
- visual studio
- opengl coordinate system
---

使用教程提供的vertices构建程序

加入直接绘制图像，会出现穿模现象：就是图形背面会绘制在图形正面。

解决方法：（Z缓冲）

```c++
glEnable(GL_DEPTH_TEST);
...
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
```


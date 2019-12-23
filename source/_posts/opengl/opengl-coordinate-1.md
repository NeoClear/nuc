---
title: opengl coordinate 1
date: 2019-11-29 23:23:37
tags:
- opengl
- visual studio
- opengl coordinate system
---

学学坐标系统罢。

Local Space：
相对于本地的坐标

World Space：
在整个世界的坐标

Camera：
摄像机有个拍摄坐标和角度

<!--more-->

生成坐标的代码：
```c++
glm::mat4 modelMat;
modelMat = glm::rotate(modelMat, glm::radians(-55.0f), glm::vec3(1.0f, 0.0f, 0.0f));
glm::mat4 viewMat;
viewMat = glm::translate(viewMat, glm::vec3(0.0f, 0.0f, -3.0f));
glm::mat4 projMat = glm::perspective(glm::radians(45.0f), 800.0f / 600.0f, 0.1f, 100.0f);
```

将值传入vertex shader：
```c++
gl_Position = projMat * viewMat * modelMat * vec4(aPos.x, aPos.y, aPos.z, 1.0f);
```

顺序需要反着写。（别问我为啥
不放全部代码了。
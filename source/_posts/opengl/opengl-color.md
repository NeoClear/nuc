---
title: opengl color
date: 2019-12-04 15:28:35
tags:
- opengl
- visual studio
- opengl color
---

一个颜色为v1的物体在颜色为v2的光源的照射下的颜色是v1 * v2。

所以假如环境是环绕光（就是不是单点光源的那种），只要给渲染出来的图像乘个光源的颜色就行了。

代码不放了，就几行2333

<!--more-->

```c++
glUniform3f(glGetUniformLocation(shader->ID, "objColor"), 1.0f, 0.5f, 0.31f);
glUniform3f(glGetUniformLocation(shader->ID, "ambientColor"), 1.0f, 0.5f, 0.31f);
```

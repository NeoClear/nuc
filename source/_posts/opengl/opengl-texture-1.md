---
title: opengl texture 1
date: 2019-11-25 23:19:32
tags:
- opengl
- visual studio
- opengl texture
---

第一节课总是简单些的。
使用第三方库：sbt_image。

在include的时候需要：

<!--more-->

```c++
#define STB_IMAGE_IMPLEMENTATION
#include "stb_image.h"
```

读入image data：

```c++
stbi_set_flip_vertically_on_load(true);
int width, height, nrChannels;
unsigned char* data = stbi_load("tudou.jpg", &width, &height, &nrChannels, 0);
```

调用stbi_set_flip_vertically_on_load是因为opengl的坐标和stb_image的默认坐标y轴是相反的。

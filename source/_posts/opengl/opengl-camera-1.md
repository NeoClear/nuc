---
title: opengl camera 1
date: 2019-11-30 17:46:10
tags:
---

giligili 爱~~~，不作死就不会die~~~

扯远了。。。

<!--more-->

直接贴代码：
```c++
// Camera.h
#pragma once

#include <glm/glm.hpp>
#include <glm/gtc/matrix_transform.hpp>

class Camera {
public:
    Camera(glm::vec3 position, glm::vec3 target, glm::vec3 world_up);
    Camera(glm::vec3 position, float pitch, float yaw, glm::vec3 world_up);
    
    // camera的位置
    glm::vec3 Position;
    // camera置于原点时target的位置，也就是camera的方向
    glm::vec3 Forward;
    // camera的右向量
    glm::vec3 Right;
    // camera的上向量
    glm::vec3 Up;
    // 整个世界的上向量
    glm::vec3 WorldUp;

    glm::mat4 GetViewMatrix();

};
```

```c++
// Camera.cpp
#include "Camera.h"

Camera::Camera(glm::vec3 position, glm::vec3 target, glm::vec3 world_up) {
    Position = position;
    WorldUp = world_up;
    Forward = glm::normalize(target - position);
    Right = glm::normalize(glm::cross(Forward, WorldUp));
    Up = glm::normalize(glm::cross(Forward, Right));
}

Camera::Camera(glm::vec3 position, float pitch, float yaw, glm::vec3 world_up) {
    Position = position;
    WorldUp = world_up;
    Forward.x = glm::cos(pitch) * glm::sin(yaw);
    Forward.y = glm::sin(pitch);
    Forward.z = glm::cos(pitch) * cos(yaw);
    Up = glm::normalize(glm::cross(Forward, Right));
}

glm::mat4 Camera::GetViewMatrix() {
    // 当前Camera位置、物体位置、世界的up向量
    return glm::lookAt(Position, Position + Forward, WorldUp);
}
```

最重要的是pitch、yaw和target向量之间的转换。明白这些，camera章节基本是理解了。
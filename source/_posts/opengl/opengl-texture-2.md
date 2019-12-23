---
title: opengl texture 2
date: 2019-11-26 00:10:45
tags:
- opengl
- visual studio
- opengl texture
---

贴图的uv坐标：
一张图，可以看成是一个u坐标为0至1的浮点数，v坐标为0至1的浮点数的坐标。
u可以看成x坐标，v可以看成y坐标。

曾经的绘图API只支持正方形的贴图，现在可以用长方形的贴图了。但是，但是，长方形的边还是只能是2的幂次方。

<!--more-->

采样的方式：nearest和linear
linear通过加权计算像素的颜色，而nearest直接取最近的像素块的颜色

我们在vertices数组中每行再加两个float作为uv值。

```c++
float vertices[] = {
//     ---- 位置 ----       ---- 颜色 ----     - 纹理坐标 -
     0.5f,  0.5f, 0.0f,   1.0f, 0.0f, 0.0f,   1.0f, 1.0f,   // 右上
     0.5f, -0.5f, 0.0f,   0.0f, 1.0f, 0.0f,   1.0f, 0.0f,   // 右下
    -0.5f, -0.5f, 0.0f,   0.0f, 0.0f, 1.0f,   0.0f, 0.0f,   // 左下
    -0.5f,  0.5f, 0.0f,   1.0f, 1.0f, 0.0f,   0.0f, 1.0f    // 左上
};
```

在vertex shader中我们把uv值传出去
```c++
#version 330 core

layout(location = 6) in vec3 aPos;
layout(location = 7) in vec3 aColor;
layout(location = 8) in vec2 aTexCoord;

out vec4 vertexColor;
out vec2 TexCoord;
void main() {
    gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0f);
    vertexColor = vec4(aColor.x, aColor.y, aColor.z, 1.0f);
    TexCoord = aTexCoord;
}
```

在fragment shader中我们将其与uniform shampler2D变量混合，得到改贴图对应uv坐标的颜色值
```c++
#version 330 core

in vec4 vertexColor;
in vec2 TexCoord;

uniform sampler2D ourTexture;

out vec4 color;

void main() {
    color = texture(ourTexture, TexCoord) * vertexColor;
}
```

sampler2D是指贴图变量。texture是glsl的采样函数。

接下来就是绑定texture buffer和传送数据的操作了：

```c++
glVertexAttribPointer(8, 2, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)(6 * sizeof(float)));
glEnableVertexAttribArray(8);

unsigned int TexBuffer;
glGenTextures(1, &TexBuffer);
glBindTexture(GL_TEXTURE_2D, TexBuffer);

int width, height, nrChannel;
unsigned char* data = stbi_load("container.jpg", &width, &height, &nrChannel, NULL);
if (data) {
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
    // 生成mip map，不调用会无法显示图像（因为没有为每个大小生成对应的图像）
    glGenerateMipmap(GL_TEXTURE_2D);
} else {
    cout << "Load image failed" << endl;
}

stbi_image_free(data);
```

最后在消息循环中加入以下代码

```c++
// 使用texture buffer
glBindTexture(GL_TEXTURE_2D, TexBuffer);
```

和VAO、EBO的绑定差不多（其实Texture Buffer的地位和前两者差不多）

这里放上所有代码：

```c++
// main.cpp
#define GLEW_STATIC
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>
#include <cmath>
#include "Shader.h"

#define STB_IMAGE_IMPLEMENTATION
#include "stb_image.h"

using namespace std;

// 用于绘制顶点的数组
float vertices[] = {
//     ---- 位置 ----       ---- 颜色 ----     - 纹理坐标 -
     0.5f,  0.5f, 0.0f,   1.0f, 0.0f, 0.0f,   1.0f, 1.0f,   // 右上
     0.5f, -0.5f, 0.0f,   0.0f, 1.0f, 0.0f,   1.0f, 0.0f,   // 右下
    -0.5f, -0.5f, 0.0f,   0.0f, 0.0f, 1.0f,   0.0f, 0.0f,   // 左下
    -0.5f,  0.5f, 0.0f,   1.0f, 1.0f, 0.0f,   0.0f, 1.0f    // 左上
};

// 0, 1, 2
// 2, 1, 3

// index索引
unsigned int indices[] = {
    0, 3, 2,
    2, 1, 0
};

// 处理窗口的键盘事件
void processInput(GLFWwindow* window) {
    if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
    if (glfwGetKey(window, GLFW_KEY_ENTER) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
}

int main() {
    // glfw的简单设置
    glfwInit();
    glfwInitHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwInitHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    // 设置profile为core profile
    // 大概和opengl版本有关
    // 早期的pipeline为固定管线，现代的pipeline为可编程管线
    // 所以需要确定profile版本
    glfwInitHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

    GLFWwindow* window = glfwCreateWindow(1600, 1200, "Test", NULL, NULL);
    
    glfwMakeContextCurrent(window);
    glViewport(0, 0, 1600, 1200);

    glEnable(GL_CULL_FACE);
    glCullFace(GL_BACK);
    // glPolygonMode(GL_FRONT_AND_BACK, GL_LINE);
    
    // 注意在init glew之前必须先glfwMakeContextCurrent
    // 否则会init失败
    glewExperimental = true;
    if (glewInit() != GLEW_OK) { cout << "Failed to init glew\n"; glfwTerminate(); return -1; }

    // 新建VAO并绑定至当前vertex array
    // VAO相当于VBO的一个指针
    // 一个VBO可能包含多个信息，使用VAO可以更加方便
    // 这一部分非常复杂，建议之间观看傅老师的视频
    unsigned int VAO;
    glGenVertexArrays(1, &VAO);
    glBindVertexArray(VAO);

    // 新建VBO并绑定至当前buffer
    unsigned int VBO;
    glGenBuffers(1, &VBO);
    glBindBuffer(GL_ARRAY_BUFFER, VBO);
    // 将顶点信息读入当前buffer
    glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

    // 新建EBO并绑定至当前上下文
    unsigned int EBO;
    glGenBuffers(1, &EBO);
    glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
    glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);

    Shader* testShader = new Shader("vertexSource.txt", "fragmentSource.txt");

    // 设置VAO的vertex attribute指针
    // 在这个例子里设置第6个pointer，将其指向当前当前buffer的具体位置
    // pointer的index可以是0至15中任意一个值
    glVertexAttribPointer(6, 3, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void *)0);
    // 激活当前VAO的第6个attribute pointer
    glEnableVertexAttribArray(6);

    // 设置VAO第7号vertex attribute指针为vertices颜色数据
    // 第二个参数为连续读取多少个data，第三个参数为读取数据的类型，第四个参数为读取数据的步长，最后一个参数为起始地址的偏移量
    glVertexAttribPointer(7, 3, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void *)(3 * sizeof(float)));
    // 激活当前VAO的第7个attribute pointer
    glEnableVertexAttribArray(7);

    glVertexAttribPointer(8, 2, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)(6 * sizeof(float)));
    glEnableVertexAttribArray(8);

    unsigned int TexBuffer;
    glGenTextures(1, &TexBuffer);
    glBindTexture(GL_TEXTURE_2D, TexBuffer);

    int width, height, nrChannel;
    unsigned char* data = stbi_load("container.jpg", &width, &height, &nrChannel, NULL);
    if (data) {
        glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
        // 生成mip map，不调用会无法显示图像（因为没有为每个大小生成对应的图像）
        glGenerateMipmap(GL_TEXTURE_2D);
    } else {
        cout << "Load image failed" << endl;
    }

    stbi_image_free(data);

    while (!glfwWindowShouldClose(window)) {
        processInput(window);
        glClearColor(0.2f, 0.3f, 0.3f, 0.2f);
        glClear(GL_COLOR_BUFFER_BIT);

        // 使用texture buffer
        glBindTexture(GL_TEXTURE_2D, TexBuffer);

        // 使用这个VAO
        glBindVertexArray(VAO);
        // 使用这个EBO
        glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
        testShader->use();

        glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
        // 使用当前attribute pointer指向的数据画图
        // glDrawArrays(GL_TRIANGLES, 0, 6);

        // glDrawArrays(GL_TRIANGLES, 0, 6);

        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    glfwTerminate();
    return 0;
}
```
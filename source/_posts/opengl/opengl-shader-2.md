---
title: opengl shader 2
date: 2019-11-25 17:33:50
tags:
- opengl
- visual studio
- opengl shader
---

久 等 了！
今天学习写一个类似调色板的多边形。

首先，我们的vertices数组增加了一倍。
每行三个float之后又加了三个float用来表示每个顶点的颜色。

<!--more-->

```c++
// 用于绘制顶点的数组
float vertices[] = {
    -0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 0.0f, // 0 red
    0.5f, -0.5f, 0.0f, 0.0f, 1.0f, 0.0f, // 1 green
    0.0f, 0.5f, 0.0f,  0.0f, 0.0f, 1.0f, // 2 blue
    // 0.8f, 0.8f, 0.0f,
    // 0.5f, -0.5f, 0.0f,
    0.8f, 0.8f, 0.0f, 1.0f, 0.0f, 1.0f    // 3
};
```

同时，vertex shader增添了代码从VAO的一个attribute pointer读取颜色数据，并将数据发送到fragment shader。

```c++
// 顶点shader program
const char* vertexShaderSource =
    "#version 330 core\n"
    "layout(location = 6) in vec3 aPos;\n"
    "layout(location = 7) in vec3 aColor;\n"
    "out vec4 vertexColor;\n"
    "void main() { gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0f); \n"
    "    vertexColor = vec4(aColor.x, aColor.y, aColor.z, 1.0f);\n"
    "}\n";

// fragment shader program
const char *fragmentShaderSource = 
    "#version 330 core                        \n"
    "in vec4 vertexColor;\n"
    "out vec4 color;                          \n"
    "uniform float ourColor;\n"
    "void main() {                            \n"
    "    color = vertexColor * ourColor;\n"
    "}";
```

当然，设置VAO的attribute pointer的代码也改变了。
```c++
// 设置VAO第7号vertex attribute指针为vertices颜色数据
// 第二个参数为连续读取多少个data，第三个参数为读取数据的类型，第四个参数为读取数据的步长，最后一个参数为起始地址的偏移量
glVertexAttribPointer(7, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void *)(3 * sizeof(float)));
// 激活当前VAO的第7个attribute pointer
glEnableVertexAttribArray(7);
```

当然，只要你代码写对，显卡会自动给你的fragment插值获得颜色。
上代码：
```c++
#define GLEW_STATIC
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>
#include <cmath>

using namespace std;

// 用于绘制顶点的数组
float vertices[] = {
    -0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 0.0f, // 0 red
    0.5f, -0.5f, 0.0f, 0.0f, 1.0f, 0.0f, // 1 green
    0.0f, 0.5f, 0.0f,  0.0f, 0.0f, 1.0f, // 2 blue
    // 0.8f, 0.8f, 0.0f,
    // 0.5f, -0.5f, 0.0f,
    0.8f, 0.8f, 0.0f, 1.0f, 0.0f, 1.0f    // 3
};

// 0, 1, 2
// 2, 1, 3

// index索引
unsigned int indices[] = {
    0, 1, 2,
    2, 1, 3
};

// 顶点shader program
const char* vertexShaderSource =
    "#version 330 core\n"
    "layout(location = 6) in vec3 aPos;\n"
    "layout(location = 7) in vec3 aColor;\n"
    "out vec4 vertexColor;\n"
    "void main() { gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0f); \n"
    "    vertexColor = vec4(aColor.x, aColor.y, aColor.z, 1.0f);\n"
    "}\n";

// fragment shader program
const char *fragmentShaderSource = 
    "#version 330 core                        \n"
    "in vec4 vertexColor;\n"
    "out vec4 color;                          \n"
    "uniform float ourColor;\n"
    "void main() {                            \n"
    "    color = vertexColor * ourColor;\n"
    "}";

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

    // 编译vertex shader
    unsigned int vertexShader = glCreateShader(GL_VERTEX_SHADER);
    glShaderSource(vertexShader, 1, &vertexShaderSource, NULL);
    glCompileShader(vertexShader);

    // 编译fragment shader
    unsigned int fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
    glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL);
    glCompileShader(fragmentShader);

    // 创建shader program
    unsigned int shaderProgram = glCreateProgram();
    glAttachShader(shaderProgram, vertexShader);
    glAttachShader(shaderProgram, fragmentShader);
    glLinkProgram(shaderProgram);

    // 设置VAO的vertex attribute指针
    // 在这个例子里设置第6个pointer，将其指向当前当前buffer的具体位置
    // pointer的index可以是0至15中任意一个值
    glVertexAttribPointer(6, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void *)0);
    // 激活当前VAO的第6个attribute pointer
    glEnableVertexAttribArray(6);

    // 设置VAO第7号vertex attribute指针为vertices颜色数据
    // 第二个参数为连续读取多少个data，第三个参数为读取数据的类型，第四个参数为读取数据的步长，最后一个参数为起始地址的偏移量
    glVertexAttribPointer(7, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void *)(3 * sizeof(float)));
    // 激活当前VAO的第7个attribute pointer
    glEnableVertexAttribArray(7);

    while (!glfwWindowShouldClose(window)) {
        processInput(window);
        glClearColor(0.2f, 0.3f, 0.3f, 0.2f);
        glClear(GL_COLOR_BUFFER_BIT);

        // 使用这个VAO
        glBindVertexArray(VAO);
        // 使用这个EBO
        glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);

        float timeValue = glfwGetTime();
        float greenValue = (sin(timeValue) / 2.0f) + 0.5f;
        // cout << greenValue << endl;
        int vertexColorLocation = glGetUniformLocation(shaderProgram, "ourColor");
        // 使用设置的shader program
        glUseProgram(shaderProgram);
        glUniform1f(vertexColorLocation, greenValue);

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
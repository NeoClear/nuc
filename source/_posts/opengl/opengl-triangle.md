---
title: opengl triangle
date: 2019-11-23 21:04:19
tags:
- opengl
- visual studio
---

开头先放下傅老师的视频教程地址：[是台湾腔，wsl](https://www.bilibili.com/video/av24353839)

visual studio绝对是宇宙第一IDE！
在linux上调了半年的CMake，用visual studio两分钟搞定。
微软大法好。

今天的opengl学习内容是在window上画一个三角形。
直接贴代码吧，分着写多累呐。
```c++
#define GLEW_STATIC
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>

using namespace std;

// 用于绘制顶点的数组
float vertices[] = {
    -0.5f, -0.5f, 0.0f,
    0.5f, -0.5f, 0.0f,
    0.0f, 0.5f, 0.0f,
    0.8f, 0.8f, 0.0f,
    // 0.5f, -0.5f, 0.0f,
    // 0.0f, 0.5f, 0.0f
};

// index索引
unsigned int indices[] = {
    0, 1, 2,
    2, 1, 3
};

// 顶点shader program
const char *vertexShaderSource =
    "#version 330 core\n"
    // 此行指将VAO中第6号index中指向的data储存在aPos中
    // location可以是0至15中任何一个值
    // 当然需要和下文中的attribute pointer的值相同
    "layout(location = 6) in vec3 aPos;\n"
    "void main() { gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0f); }\n";

// fragment shader program
const char *fragmentShaderSource = 
    "#version 330 core                        \n"
    "out vec4 color;                          \n"
    "void main() {                            \n"
    "    color = vec4(1.0f, 0.5f, 0.2f, 1.0f);\n"
    "}";

// 处理窗口的键盘事件
void processInput(GLFWwindow* window) {
    if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
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

    GLFWwindow* window = glfwCreateWindow(800, 600, "Test", NULL, NULL);
    
    glfwMakeContextCurrent(window);
    glViewport(0, 0, 800, 600);

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
    glVertexAttribPointer(6, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void *)0);
    // 激活当前VAO的第6个attribute pointer
    glEnableVertexAttribArray(6);

    while (!glfwWindowShouldClose(window)) {
        processInput(window);
        glClearColor(0.2f, 0.3f, 0.3f, 0.2f);
        glClear(GL_COLOR_BUFFER_BIT);

        // 使用这个VAO
        glBindVertexArray(VAO);
        // 使用这个EBO
        glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
        // 使用设置的shader program
        glUseProgram(shaderProgram);
        // 使用当前的EBO画图
        glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
        // 使用当前attribute pointer指向的数据画图
        // glDrawArrays(GL_TRIANGLES, 0, 6);

        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    glfwTerminate();
    return 0;
}
```

有一说一，opengl的代码，不对着模板打，完全不知道怎么写
有一万个名字差不多的函数要调用，人能记得住🐎？？？

本节内容的重点是明白VBO和VAO的含义。
VBO相当与显卡上的一块缓冲区（准确来说是显存），其内容无法被显卡识别，是由CPU直接通过PCI-E复制到显卡上的。
VAO相当与指向一个VBO的指针，内部有15个小指针（0至15）。通过指定VAO和设置Attribute Pointer，显卡的opengl API能识别显存中的数据。（和CPU中的高速缓存地址计算寄存器差不多吧）

再接再厉吧，opengl真的不是人能学的。
没办法，谁让AMD是显卡公司呢

加一点内容：

opengl默认绘制三角形为逆时针绘制，假如三角形的点排序为顺时针，则会绘制图形的背面。
opengl默认绘制图形背面。假如不需要绘制图形背面，可以加入以下代码：
```c++
glEnable(GL_CULL_FACE);
glCullFace(GL_FRONT);
```

假如想要将多边形绘制成线条状，可以使用以下函数：
```c++
glPolygonMode(GL_FRONT_AND_BACK, GL_LINE);
```

opengl可以理解为一个状态机，拥有一个context。
上文中的各种bind函数将VAO、VBO、EBO绑定至当前context，然后使用其组合绘制图形。
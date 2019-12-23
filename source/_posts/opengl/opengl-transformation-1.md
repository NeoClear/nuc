---
title: opengl transformation 1
date: 2019-11-27 22:41:06
tags:
- opengl
- visual studio
- opengl transformation
---

开始学线性代数了。。。

缩放：
S1 0  0  0       x       S1 * x
0  S2 0  0   *   y   =   S2 * y
0  0  S3 0       z       S3 * z
0  0  0  1       1       1

<!--more-->

位移
1  0  0  T1      x       x + T1
0  1  0  T2  *   y   =   y + T2
0  0  0  T3      z       z + T3
0  0  0  1       1       1

x y z w
如果表示空间顶点的位置，w为1
如果表示两点的差或其他量，w为0。（自己体会）

另外还有沿x、y、z旋转的矩阵，这里就不展示了，有兴趣自己上网站看。

万向节死锁（Gimbal Lock）：
自己上网查，这玩意没法直接描述。
解决方法：四元数（我也不知道这啥玩意）

变换的顺序：
位移 -> 旋转 -> 缩放
大概是因为local space和global space的关系罢。

直接贴代码（有这么不走心的🐎？？？）：
```c++
#define GLEW_STATIC
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>
#include <cmath>
#include "Shader.h"

#include "glm/glm.hpp"
#include "glm/gtc/matrix_transform.hpp"
#include "glm/gtc/type_ptr.hpp"


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

inline void init() {
    glm::vec4 vec(1.0f, 0, 0, 1.0f);

    // create an identity 4x4 matrix
    glm::mat4 trans = glm::mat4(1.0f);

    trans = glm::translate(trans, glm::vec3(2.0f, 0, -1.0f));
    vec = trans * vec;

    cout << vec.x << ", " << vec.y << ", " << vec.z << endl;

}

int main() {
    // init();
    stbi_set_flip_vertically_on_load(true);

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

    // 设置texture buffer
    unsigned int TexBufferA;
    glGenTextures(1, &TexBufferA);
    // 使用第一个槽位
    glActiveTexture(GL_TEXTURE0);
    glBindTexture(GL_TEXTURE_2D, TexBufferA);

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

    unsigned int TexBufferB;
    glGenTextures(1, &TexBufferB);
    glActiveTexture(GL_TEXTURE3);
    glBindTexture(GL_TEXTURE_2D, TexBufferB);

    unsigned char* data2 = stbi_load("awesomeface.png", &width, &height, &nrChannel, NULL);

    if (data2) {
        glTexImage2D(GL_TEXTURE_2D, 0, GL_RGBA, width, height, 0, GL_RGBA, GL_UNSIGNED_BYTE, data2);
        // 生成mip map，不调用会无法显示图像（因为没有为每个大小生成对应的图像）
        glGenerateMipmap(GL_TEXTURE_2D);
    }
    else {
        cout << "Load image failed" << endl;
    }

    // 在这里计算转换矩阵

    
    // trans = glm::rotate(trans, glm::radians(90.0f), glm::vec3(0.0f, 0.0f, 1.0f));
    // trans = glm::scale(trans, glm::vec3(0.5f, 0.5f, 0.5f));

    while (!glfwWindowShouldClose(window)) {
        glm::mat4 trans;
        trans = glm::translate(trans, glm::vec3(-0.5f, 0.0f, 0.0f));
        trans = glm::rotate(trans, glm::radians(45.0f), glm::vec3(0.0f, 0.0f, 1.0f));
        trans = glm::scale(trans, glm::vec3(2.0f, 1.0f, 1.0f));

        processInput(window);
        glClearColor(0.2f, 0.3f, 0.3f, 0.2f);
        glClear(GL_COLOR_BUFFER_BIT);
        // 使用texture buffer
        glActiveTexture(GL_TEXTURE0);
        glBindTexture(GL_TEXTURE_2D, TexBufferA);
        // 使用第三个槽位
        glActiveTexture(GL_TEXTURE3);
        glBindTexture(GL_TEXTURE_2D, TexBufferB);

        // 使用这个VAO
        glBindVertexArray(VAO);
        // 使用这个EBO
        glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
        testShader->use();
        // 将材质uniform变量设置为int值，从0至15
        // 鬼晓得是咋回事，这不是贴图变量吗？？？
        glUniform1i(glGetUniformLocation(testShader->ID, "ourTexture"), 0);
        glUniform1i(glGetUniformLocation(testShader->ID, "ourTexture"), 3);

        glUniformMatrix4fv(glGetUniformLocation(testShader->ID, "transform"),
                          1, GL_FALSE, glm::value_ptr(trans));

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
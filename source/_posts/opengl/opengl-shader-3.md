---
title: opengl shader 3
date: 2019-11-25 18:08:33
tags:
- opengl
- visual studio
- opengl shader
---

这次准备写一个Shader Class，封装shader代码的读取和shader的编译

代码其实也不复杂。。。（are you serious？？？）

<!--more-->

先上shader class的代码
```c++
#include "Shader.h"
#include <iostream>
#include <fstream>
#include <sstream>

#define GLEW_STATIC
#include <GL/glew.h>
#include <GLFW/glfw3.h>

using namespace std;

Shader::Shader(const char* vertexPath, const char* fragmentPath) {
    ifstream vertexFile;
    ifstream fragmentFile;
    stringstream vertexSStream;
    stringstream fragmentSStream;

    vertexFile.open(vertexPath);
    fragmentFile.open(fragmentPath);
    vertexFile.exceptions(ifstream::failbit || ifstream::badbit);
    fragmentFile.exceptions(ifstream::failbit || ifstream::badbit);

    try {
        // 从文件读取shader program
        if (!vertexFile.is_open() || !fragmentFile.is_open())
            throw exception("open file error!\n");
        vertexSStream << vertexFile.rdbuf();
        fragmentSStream << fragmentFile.rdbuf();

        vertexString = vertexSStream.str();
        fragmentString = fragmentSStream.str();
        vertexCode = vertexString.c_str();
        fragmentCode = fragmentString.c_str();

        unsigned int vertex, fragment;

        // 编译vertex shader
        vertex = glCreateShader(GL_VERTEX_SHADER);
        glShaderSource(vertex, 1, &vertexCode, NULL);
        glCompileShader(vertex);
        checkCompileErrors(vertex, "VERTEX");

        // 编译fragment shader
        fragment = glCreateShader(GL_FRAGMENT_SHADER);
        glShaderSource(fragment, 1, &fragmentCode, NULL);
        glCompileShader(fragment);
        checkCompileErrors(fragment, "FRAGMENT");

        // 链接shader program
        ID = glCreateProgram();
        glAttachShader(ID, vertex);
        glAttachShader(ID, fragment);
        glLinkProgram(ID);
        checkCompileErrors(ID, "PROGRAM");

        glDeleteShader(vertex);
        glDeleteShader(fragment);
    } catch (const exception& e) {
        cout << e.what() << endl;
    }
}

void Shader::use() {
    // 使用当前program
    glUseProgram(ID);
}

void Shader::checkCompileErrors(unsigned int id, std::string type) {
    int success;
    char infoLog[512];
    if (type != "PROGRAM") {
        // 假如是不是program类别
        glGetShaderiv(id, GL_COMPILE_STATUS, &success);
        if (!success) {
            glGetShaderInfoLog(id, 512, NULL, infoLog);
            cout << "Shader Compile Error: " << infoLog << endl;
        }
    } else {
        // 假如是program类别
        glGetProgramiv(id, GL_LINK_STATUS, &success);
        if (!success) {
            glGetProgramInfoLog(id, 512, NULL, infoLog);
            cout << "Program Compile Error: " << infoLog << endl;
        }
    }
}
```

注意：需要在主程序init glew过后才能使用shader。
当然，shader class并不是万能的，VAO、VBO、EBO之类的还是需要自己维护。
（说真的，封装之后代码就没有那么清楚了。。。）
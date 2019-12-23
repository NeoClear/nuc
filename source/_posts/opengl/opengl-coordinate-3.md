---
title: opengl coordinate 3
date: 2019-11-30 14:56:23
tags:
- opengl
- visual studio
- opengl coordinate system
---

![coordinate system](img/opengl/multiple_cube.png)

重点：

<!--more-->

```c++
for (unsigned int i = 0; i < 10; i++) {
    glm::mat4 modelMat2;
    modelMat2 = glm::translate(modelMat2, cubePositions[i]);
    testShader->use();
    // 将材质uniform变量设置为int值，从0至15
    // 鬼晓得是咋回事，这不是贴图变量吗？？？
    glUniform1i(glGetUniformLocation(testShader->ID, "ourTexture"), 0);
    glUniform1i(glGetUniformLocation(testShader->ID, "ourTexture"), 3);

    /* glUniformMatrix4fv(glGetUniformLocation(testShader->ID, "transform"),
                        1, GL_FALSE, glm::value_ptr(trans));*/
    glUniformMatrix4fv(glGetUniformLocation(testShader->ID, "modelMat"),
        1, GL_FALSE, glm::value_ptr(modelMat2));
    glUniformMatrix4fv(glGetUniformLocation(testShader->ID, "viewMat"),
        1, GL_FALSE, glm::value_ptr(viewMat));
    glUniformMatrix4fv(glGetUniformLocation(testShader->ID, "projMat"),
        1, GL_FALSE, glm::value_ptr(projMat));
    glDrawArrays(GL_TRIANGLES, 0, 36);
}
```

用一个循环读取每个顶点的数据，绘制一个正方体
---
title: opengl lighting 1
date: 2019-12-04 17:04:55
tags:
- opengl
- visual studio
- opengl lighting
---

开始学光照惹。。。
有种不好的感觉

ambient -> diffuse -> specular -> combined(Phong)

## 环境颜色

将当前点的法向量和光线向量做点乘，就能得到当前点的环境光下的颜色

## 漫反射光照

将一个平面上的一点的法向量乘以这个点到光源的向量再乘上环境光的颜色，就可以得到这个点的漫反射的值。

```c++
vec3 lightDir = normalize(lightPos - FragPos);
vec3 diffuse = max(dot(lightDir, Normal), 0.0f) * lightColor;
FragColor = vec4((diffuse + ambientColor) / 2 * objColor, 1.0f);
```

## 镜面反射

计算平面上一个点的光反射向量与观察者到此点的向量的点成得到角度的cos值，通过cos值加权计算得到lightColor在此处的权重，最后与ambient和diffuse一起得到此处的光照。

```c++
vec3 cameraVec = normalize(cameraPos - FragPos);
vec3 specular = pow(max(dot(reflectVec, cameraVec), 0.0f), 32) * lightColor;
```
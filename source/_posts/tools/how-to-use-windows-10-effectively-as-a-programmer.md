---
title: 如何高效使用windows 10
date: 2019-12-18 18:47:02
tags:
- windows
- windows 10
- microsoft
---

最近重装Windows 10（因为一些不可抗力），对于如何将windows 10打造成cs系自闭学生的代码环境深有体会。于下文讲解一二。

首先，你需要有一个非常帅气的bash（就像Linux平台一样）。但是众所周知，windows自带的powershell和cmd非常的丑，特别是powershell，虽然字体还算不错，但是那个深蓝的背景是什么鬼？？？果然微软内部有蓝色巨人的内应（什么玩意）。当然，我也曾经尝试过第三方的terminal，比如msys2和cmder。msys2内置pacman，安装应用自然是非常方便，但是硬伤也很明显：不清真。外观丑陋，一股浓浓的win7和xp的结合体风格（现在是9012年了喂）。cmder也不是最终解决方案：这个软件是portable version，和系统并不是深度结合，令完美主义者抓狂，而且有时候下载的软件会报错（没错就是这样）。所以，我们选择windows terminal。这个终端软件在2019年初的microsoft build大会第一次与世人见面（没错微软传统的预览版）。据说2019年底能出正式版（假的吧，我现在还没见着正式版呢，于2019年12月中旬）。当然，就算预览版，还是非常好用的。windows terminal其实就是给每个终端软件再加一个壳，所以其内核还是用户自己选择的bash软件（一般是都是cmd或者powershell）。下载方式：在microsoft store里下载。

当然，选择了shell之后，我们还是需要选择真正的bash程序。这里我们放弃powershell，因为powershell的弱智的deep♂蓝色背景（没错又是它）。这里，我们选择powershell core。这是一个与powershell类似，但跨平台的powershell终端软件（微软还是干了件大好事）。安装方式：上微软github页面下载msi。

最后，还要调整下windows terminal的设置![profile.json](text/windows10/windows_terminal_profile.json)。这里大概设置了下配色和快捷键和调用powershell core（直接复制粘贴就行了，别问）。

如果进行到这里，一个还算漂亮的与windows深度绑定的与linux风格非常像的terminal软件算是完成了。之后我们还需要了解下windows下的cli软件。

首先是包管理软件。这里我们选择chocolately。安装方式：上官网看教程。安装完choco之后就可以以linux方式安装cli软件了（真的非常方便）。

一般来说cs的苦逼学生还需要c/c++编译器和python解释器。c/c++我推荐clang（因为体验还算可以）。llvm官网有windows的prebuild binaries版本，直接下载安装就行。运行clang需要vs的支持。这里只要安装vs最新版的c/c++核心功能就行，不需要完整的vs（可以只安装microsoft c++ toolset from command line）。

最后需要安装的就是我们的python啦。有意思的是，python在微软商店有售，如果不想整msi安装包可以直接去微软商店下载（一般来说在microsoft store下载的程序更干净，更容易卸载）。

在这里一个cs学生的windows 10就设置完毕啦。之后想装什么cli软件可以通过choco下载。

强烈安利surface pro系列（没错我就是微软的舔🐕）。
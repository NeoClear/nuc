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

<!--more-->

当然，选择了shell之后，我们还是需要选择真正的bash程序。这里我们放弃powershell，因为powershell的弱智的deep♂蓝色背景（没错又是它）。这里，我们选择powershell core。这是一个与powershell类似，但跨平台的powershell终端软件（微软还是干了件大好事）。安装方式：上微软github页面下载msi。

最后，还要调整下windows terminal的设置。这里大概设置了下配色和快捷键和调用powershell core（直接复制粘贴就行了，别问）。

```json
// profile.json
{
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "globals" : 
    {
        "alwaysShowTabs" : true,
        "defaultProfile" : "{90cbdc15-f4fe-49d2-a245-ec066b70845f}",
        "initialCols" : 120,
        "initialRows" : 30,
        "keybindings" : 
        [
            {
                "command" : "closeTab",
                "keys" : 
                [
                    "ctrl+w"
                ]
            },
            {
                "command" : "newTab",
                "keys" : 
                [
                    "ctrl+t"
                ]
            },
            {
                "command" : "newTabProfile0",
                "keys" : 
                [
                    "ctrl+shift+1"
                ]
            },
            {
                "command" : "newTabProfile1",
                "keys" : 
                [
                    "ctrl+shift+2"
                ]
            },
            {
                "command" : "newTabProfile2",
                "keys" : 
                [
                    "ctrl+shift+3"
                ]
            },
            {
                "command" : "newTabProfile3",
                "keys" : 
                [
                    "ctrl+shift+4"
                ]
            },
            {
                "command" : "newTabProfile4",
                "keys" : 
                [
                    "ctrl+shift+5"
                ]
            },
            {
                "command" : "newTabProfile5",
                "keys" : 
                [
                    "ctrl+shift+6"
                ]
            },
            {
                "command" : "newTabProfile6",
                "keys" : 
                [
                    "ctrl+shift+7"
                ]
            },
            {
                "command" : "newTabProfile7",
                "keys" : 
                [
                    "ctrl+shift+8"
                ]
            },
            {
                "command" : "newTabProfile8",
                "keys" : 
                [
                    "ctrl+shift+9"
                ]
            },
            {
                "command" : "nextTab",
                "keys" : 
                [
                    "ctrl+tab"
                ]
            },
            {
                "command" : "openSettings",
                "keys" : 
                [
                    "ctrl+,"
                ]
            },
            {
                "command" : "prevTab",
                "keys" : 
                [
                    "ctrl+shift+tab"
                ]
            },
            {
                "command" : "scrollDown",
                "keys" : 
                [
                    "ctrl+shift+down"
                ]
            },
            {
                "command" : "scrollDownPage",
                "keys" : 
                [
                    "ctrl+shift+pgdn"
                ]
            },
            {
                "command" : "scrollUp",
                "keys" : 
                [
                    "ctrl+shift+up"
                ]
            },
            {
                "command" : "scrollUpPage",
                "keys" : 
                [
                    "ctrl+shift+pgup"
                ]
            },
            {
                "command" : "switchToTab0",
                "keys" : 
                [
                    "alt+1"
                ]
            },
            {
                "command" : "switchToTab1",
                "keys" : 
                [
                    "alt+2"
                ]
            },
            {
                "command" : "switchToTab2",
                "keys" : 
                [
                    "alt+3"
                ]
            },
            {
                "command" : "switchToTab3",
                "keys" : 
                [
                    "alt+4"
                ]
            },
            {
                "command" : "switchToTab4",
                "keys" : 
                [
                    "alt+5"
                ]
            },
            {
                "command" : "switchToTab5",
                "keys" : 
                [
                    "alt+6"
                ]
            },
            {
                "command" : "switchToTab6",
                "keys" : 
                [
                    "alt+7"
                ]
            },
            {
                "command" : "switchToTab7",
                "keys" : 
                [
                    "alt+8"
                ]
            },
            {
                "command" : "switchToTab8",
                "keys" : 
                [
                    "alt+9"
                ]
            }
        ],
        "requestedTheme" : "system",
        "showTabsInTitlebar" : true,
        "showTerminalTitleInTitlebar" : true,
        "wordDelimiters" : " ./\\()\"'-:,.;<>~!@#$%^&*|+=[]{}~?\u2502"
    },
    "profiles" : 
    [
        {
		"acrylicOpacity" : 0.5,
		"closeOnExit" : true,
		"colorScheme" : "Solarized Light",
		"commandline" : "C:\\Program Files\\PowerShell\\7-preview\\pwsh.exe",
		"cursorColor" : "#657B83",
		"cursorShape" : "bar",
		"fontFace" : "Consolas",
		"fontSize" : 12,
		"guid" : "{90cbdc15-f4fe-49d2-a245-ec066b70845f}",
		"historySize" : 9001,
		"icon" : "C:\\Program Files\\PowerShell\\7-preview\\assets\\Powershell_av_colors.ico",
		"name" : "PowerShell Core",
		"padding" : "0, 0, 0, 0",
		"snapOnInput" : true,
		"startingDirectory" : "%USERPROFILE%",
		"useAcrylic" : false
	},
        {
            "guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
            "hidden": false,
            "name": "PowerShell Core",
            "source": "Windows.Terminal.PowershellCore"
        },
        {
            "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
            "hidden": false,
            "name": "Azure Cloud Shell",
            "source": "Windows.Terminal.Azure"
        }
    ],
    "schemes" : 
    [
        {
            "background" : "#0C0C0C",
            "black" : "#0C0C0C",
            "blue" : "#0037DA",
            "brightBlack" : "#767676",
            "brightBlue" : "#3B78FF",
            "brightCyan" : "#61D6D6",
            "brightGreen" : "#16C60C",
            "brightPurple" : "#B4009E",
            "brightRed" : "#E74856",
            "brightWhite" : "#F2F2F2",
            "brightYellow" : "#F9F1A5",
            "cyan" : "#3A96DD",
            "foreground" : "#A7B191",
            "green" : "#13A10E",
            "name" : "Campbell",
            "purple" : "#881798",
            "red" : "#C50F1F",
            "white" : "#CCCCCC",
            "yellow" : "#C19C00"
        },
        {
            "background" : "#073642",
            "black" : "#073642",
            "blue" : "#268BD2",
            "brightBlack" : "#002B36",
            "brightBlue" : "#839496",
            "brightCyan" : "#93A1A1",
            "brightGreen" : "#586E75",
            "brightPurple" : "#6C71C4",
            "brightRed" : "#CB4B16",
            "brightWhite" : "#FDF6E3",
            "brightYellow" : "#657B83",
            "cyan" : "#2AA198",
            "foreground" : "#FDF6E3",
            "green" : "#859900",
            "name" : "Solarized Dark",
            "purple" : "#D33682",
            "red" : "#D30102",
            "white" : "#EEE8D5",
            "yellow" : "#B58900"
        },
        {
            "background" : "#FDF6E3",
            "black" : "#073642",
            "blue" : "#268BD2",
            "brightBlack" : "#002B36",
            "brightBlue" : "#839496",
            "brightCyan" : "#93A1A1",
            "brightGreen" : "#586E75",
            "brightPurple" : "#6C71C4",
            "brightRed" : "#CB4B16",
            "brightWhite" : "#FDF6E3",
            "brightYellow" : "#657B83",
            "cyan" : "#2AA198",
            "foreground" : "#073642",
            "green" : "#859900",
            "name" : "Solarized Light",
            "purple" : "#D33682",
            "red" : "#D30102",
            "white" : "#EEE8D5",
            "yellow" : "#B58900"
        },
        {
            "background" : "#2C001E",
            "black" : "#EEEEEC",
            "blue" : "#268BD2",
            "brightBlack" : "#002B36",
            "brightBlue" : "#839496",
            "brightCyan" : "#93A1A1",
            "brightGreen" : "#586E75",
            "brightPurple" : "#6C71C4",
            "brightRed" : "#CB4B16",
            "brightWhite" : "#FDF6E3",
            "brightYellow" : "#657B83",
            "cyan" : "#2AA198",
            "foreground" : "#EEEEEC",
            "green" : "#729FCF",
            "name" : "Ubuntu",
            "purple" : "#D33682",
            "red" : "#16C60C",
            "white" : "#EEE8D5",
            "yellow" : "#B58900"
        },
        {
            "background" : "#2C001E",
            "black" : "#4E9A06",
            "blue" : "#3465A4",
            "brightBlack" : "#555753",
            "brightBlue" : "#729FCF",
            "brightCyan" : "#34E2E2",
            "brightGreen" : "#8AE234",
            "brightPurple" : "#AD7FA8",
            "brightRed" : "#EF2929",
            "brightWhite" : "#EEEEEE",
            "brightYellow" : "#FCE94F",
            "cyan" : "#06989A",
            "foreground" : "#EEEEEE",
            "green" : "#300A24",
            "name" : "UbuntuLegit",
            "purple" : "#75507B",
            "red" : "#CC0000",
            "white" : "#D3D7CF",
            "yellow" : "#C4A000"
        },
        {
            "background" : "#F7F7F7",
            "black" : "#090300",
            "blue" : "#01A0E4",
            "brightBlack" : "#5C5855",
            "brightBlue" : "#807D7C",
            "brightCyan" : "#CDAB53",
            "brightGreen" : "#3A3432",
            "brightPurple" : "#D6D5D4",
            "brightRed" : "#E8BBD0",
            "brightWhite" : "#F7F7F7",
            "brightYellow" : "#4A4543",
            "cyan" : "#B5E4F4",
            "foreground" : "#4A4543",
            "green" : "#01A252",
            "name" : "3024 Day",
            "purple" : "#A16A94",
            "red" : "#DB2D20",
            "white" : "#A5A2A2",
            "yellow" : "#FDED02"
        },
        {
            "background" : "#FFFFFF",
            "black" : "#011627",
            "blue" : "#4876D6",
            "brightBlack" : "#7A8181",
            "brightBlue" : "#5CA7E4",
            "brightCyan" : "#00C990",
            "brightGreen" : "#49D0C5",
            "brightPurple" : "#697098",
            "brightRed" : "#F76E6E",
            "brightWhite" : "#989FB1",
            "brightYellow" : "#DAC26B",
            "cyan" : "#08916A",
            "foreground" : "#403F53",
            "green" : "#2AA298",
            "name" : "Night Owlish Light",
            "purple" : "#403F53",
            "red" : "#D3423E",
            "white" : "#7A8181",
            "yellow" : "#DAAA01"
        },
        {
            "background" : "#FFFFFF",
            "black" : "#000000",
            "blue" : "#4271AE",
            "brightBlack" : "#000000",
            "brightBlue" : "#4271AE",
            "brightCyan" : "#3E999F",
            "brightGreen" : "#718C00",
            "brightPurple" : "#8959A8",
            "brightRed" : "#C82829",
            "brightWhite" : "#FFFFFF",
            "brightYellow" : "#EAB700",
            "cyan" : "#3E999F",
            "foreground" : "#4D4D4C",
            "green" : "#718C00",
            "name" : "Tomorrow",
            "purple" : "#8959A8",
            "red" : "#C82829",
            "white" : "#FFFFFF",
            "yellow" : "#EAB700"
        }
    ]
}
```

如果进行到这里，一个还算漂亮的与windows深度绑定的与linux风格非常像的terminal软件算是完成了。之后我们还需要了解下windows下的cli软件。

首先是包管理软件。这里我们选择chocolately。安装方式：上官网看教程。安装完choco之后就可以以linux方式安装cli软件了（真的非常方便）。

一般来说cs的苦逼学生还需要c/c++编译器和python解释器。c/c++我推荐clang（因为体验还算可以）。llvm官网有windows的prebuild binaries版本，直接下载安装就行。运行clang需要vs的支持。这里只要安装vs最新版的c/c++核心功能就行，不需要完整的vs（可以只安装microsoft c++ toolset from command line）。

最后需要安装的就是我们的python啦。有意思的是，python在微软商店有售，如果不想整msi安装包可以直接去微软商店下载（一般来说在microsoft store下载的程序更干净，更容易卸载）。

在这里一个cs学生的windows 10就设置完毕啦。之后想装什么cli软件可以通过choco下载。

强烈安利surface pro系列（没错我就是微软的舔🐕）。
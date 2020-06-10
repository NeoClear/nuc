---
title: windows terminal
date: 2020-06-09 21:02:21
categories:
- windows 10
tags:
- windows 10
- windows terminal
---

How to use Windows Terminal Effectively? Let's take a look

<!--more-->

## Window Management

To split windows in the same tab, we have the following config file

```json
{
    "command": {
        "action": "splitPane",
        "split": "vertical"
    },
    "keys": "ctrl+["
},
{
    "command": {
        "action": "splitPane",
        "split": "horizontal"
    },
    "keys": "ctrl+]"
},
{
    "command": {
        "action": "splitPane",
        "split": "auto"
    },
    "keys": "ctrl+\\" 
        }
```

## Change Focus Over Windows

The default hotkey to move focus over windows is alt + arrow keys
You can customize it through config

```json
{ "command": { "action": "moveFocus", "direction": "down" }, "keys": "alt+down" },
{ "command": { "action": "moveFocus", "direction": "left" }, "keys": "alt+left" },
{ "command": { "action": "moveFocus", "direction": "right" }, "keys": "alt+right" },
{ "command": { "action": "moveFocus", "direction": "up" }, "keys": "alt+up" }
```

## Close Pane

Close the pane

```json
{
    "command": "closePane",
    "keys": "ctrl+shift+w"
}
```

## Duplicate Pane

Create a new window given current window's profile

```json
{
    "command": {
        "action": "splitPane",
        "split": "auto",
        "splitMode": "duplicate"
    },
    "keys": "ctrl+|"
}
```

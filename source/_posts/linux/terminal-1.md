---
title: terminal-1
date: 2020-05-01 19:48:43
categories:
- silver bullet
tags:
- linux
- unix
- powershell
- windows
---

Currently I am trying to build a server system for remote access and computing. Here is what I have learnt in this process

<!--more-->

# Linux Techniques

## How to execute programs on a remote server after logout?

It is a tricky question, if you use nohup and & to detach it, then you cannot resume it after you logout and log back. The tool we use is called screen, here are some basic operations

```bash
# create a new session
$ screen -S <session>

# detach from a session by using C-a d

# re-attach a session
$ screen -r <session>
```

## How to create user

Sometimes we want to create user to handle a certain task. For example, we might want a new user to run minecrat server. We do this though following commands

```bash
# create user
$ sudo useradd <username>

# set password
$ sudo passwd <username>
```

## How to change ssh port

Sometimes we want to change default ssh port to prevent attacks. It can be done in this way

First, find the file /etc/ssh/sshd_config
Then add or change the line to **port=<portnum>**

Then restart the ssh service through
```bash
$ sudo service restart ssh
```
or
```bash
$ sudo systemctl restart ssh
```

Actually I don't understand why there are so many ways of dealing with system services

## How to transfer files between local & server

We can use scp comands

```bash
scp /path/to/file host@address:<path to destination>
```

If copy a directory, use recursive transfer
```bash
scp -r /path/to/file host@address:<path to destination>
```

## How to get system specs

```bash
hostnamectl
```

If you can install third-party programs, use

```bash
neofetch
```

## How to get local ip of linux

```bash
hostname -I
```

## How to expose local ssh server to public

Use ngrok or ip forwarding. ip forwarding is better but it does not work on my router. ngrok always work but address changes every time you start it.

# Powershell Techniques

## How to remove a directory

In linux it is easy, but in powershell there is no equivalent way of doing this. We have to use powershell codes

```ps
Remove-Item -path <path> -recurse
```

+++
title = 'STracek'
date = 2024-04-07T09:29:16.1616+05:30
draft = true
tags =[]
+++ 

`It allows you to trace system calls made by a process. System calls are the fundamental interfaces between user space and the kernel.`

Strace -f -p PID -> -f for trace child thread

`strace -c cmd or PID` -> print no of time each sys function get called and timing info

Note : Strace slow down the process

	ltrace -> to see all library call

## Using with htop

we can attach strace on running process `sudo htop` choose the process and press `s` it will attach the strace
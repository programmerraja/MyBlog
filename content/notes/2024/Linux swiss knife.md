+++
title = 'Linux swiss knife'
date = 2024-06-21T06:58:14.1414+05:30
draft = true
tags =[]
+++ 



 processes consuming the most CPU
- `ps H -eo pid,pcpu | sort -nk2 | tail`

 check the connection status of a specific port?
 - `netstat -lap | fgrep 22022`

**View memory usage of processes:**
- `ps aux --sort=-%mem | awk 'NR<=10{print $0}'`

` sudo du -ah / | sort -n -r | head -n 20` -> top 20 most memory used file 
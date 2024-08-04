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



## Python script

```python
import psutil

connections = psutil.net_connections()

for conn in connections:
    print(f"PID: {conn.pid} | Status: {conn.status} | Local Address: {conn.laddr} | Remote Address: {conn.raddr}")

```

Same in CMD
`netstat -tuln | awk 'NR > 2 {print "PID: " $7 " | Status: " $6 " | Local Address: " $4 " | Remote Address: " $5}'` 


Vmstat
```sh
vmstat  | tail -n 1 | awk '{print "| procs | r (run queue) | "$1" |\n| procs | b (uninterruptible sleep) | "$2" |\n| memory | swpd (swap used) | "$3" |\n| memory | free (free memory) | "$4" |\n| memory | buff (buffer memory) | "$5" |\n| memory | cache (cache memory) | "$6" |\n| swap | si (swap in) | "$7" |\n| swap | so (swap out) | "$8" |\n| io | bi (blocks read) | "$9" |\n| io | bo (blocks written) | "$10" |\n| system | in (interrupts) | "$11" |\n| system | cs (context switches) | "$12" |\n| cpu | us (user mode) | "$13"% |\n| cpu | sy (system mode) | "$14"% |\n| cpu | id (idle) | "$15"% |\n| cpu | wa (waiting for I/O) | "$16"% |\n| cpu | st (steal mode) | "$17"% |"}'

```


Tell which process waiting for DISK IO
```
watch -n1 -d "ps axu | awk '{if (\$8==\"D\") {print \$0}}'"
```

```
iostat -x 2 5
```

example above will print a report every `2` seconds for `5` intervals; the `-x` flag tells `iostat` to print out an extended report.
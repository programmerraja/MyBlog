
+++
title = 'Linux'
date = 2024-02-11T19:01:46.4646+05:30
draft = true
tags =[]
+++ 

## Linux Filesystem

## Getting Help

Every command, application, or utility has a dedicated help file in Linux by typing `--help` in front of all linux cmd or we can refer Manual Pages with man `man cmd` 

## CMD
- **whereis:**  locate bin file `whereis filename`
- **which:** Return current **PATH** var for the bin `whihc node`
- **find:** search for file name ,date of creation or modification, etc `find path -type f -name filename`

#### Files and Directories CMD
	get from chatgpt

#### Text Manipulation
- cat
- head
- tail
- nl
- sed
- more 
- less

#### Networks
- ifconfig (Changing Your IP Address.Spoofing Your MAC Address)
- iwconfig  wireless

Linux has a Dynamic Host Configuration Protocol (DHCP) server that runs a daemon a process that runs in the background called dhcpd,
- dhclient eth0 (The dhclient command sends a DHCPDISCOVER request from the network interface specified (here, eth0 ). It then receives an offer ( DHCPOFFER ) from the DHCP server)

Changing Your DNS Server
- /etc/resolv.conf -> change content here

If you’re using a DHCP address and the DHCP server provides a DNS setting, the DHCP server will replace the contents of the file when it renews the DHCP address.

The hosts file is located at /etc/hosts, and kind of as with
DNS, you can use it to specify your own IP address–domain name mapping.
In other words, you can determine which IP address your browser goes to
when you enter www.microsoft.com (or any other domain) into the browser,
rather than let the DNS server decide. As a hacker, this can be useful for
hijacking a TCP connection on your local area network to direct traffic to a
malicious web server with a tool such as dnsspoof .

`ip -s link show dev interfaceaname` 
display detailed information about the network interface
- **mtu 1500**: The Maximum Transmission Unit, which is the largest size of a packet that can be transmitted over the network (1500 bytes in this case).
    
- **qdisc noqueue**: The queuing discipline used for packet scheduling. `noqueue` means there's no queueing discipline applied.
    
- **state UP**: The operational state of the interface is "UP," indicating it is active and ready to transmit and receive data.
- 
- **RX (Receive) Statistics**: Information about packets received by the interface, such as the number of packets, bytes received, and any errors.
- **TX (Transmit) Statistics**: Information about packets transmitted by the interface, including the number of packets, bytes sent, and any error



Advanced Packaging Tool
- apt-cache search pkg-name
- apt-get install packagename
- apt-get remove snort
- apt-get purge snort :remove config file

The servers that hold the software for particular distributions of Linux are
known as repositories

The repositories your system will search for software are stored in the
**/etc/apt/sources.list** file

Types of Users

Permission
- chown username file
- Octal and Binary Representations of Permissions (chatgpt)
- Setting More Secure Default Permissions with Masks


process 
- ps
- top
- Changing Process Priority with nice
- kill
- Running Processes in the Background
- Scheduling Processes

Managing User Environment  Variables
- env
- set
- export -> to refelct every shell


Filesystem and Storage Device Management

## Proc file system

in kuberntes the process id of the running is 1

ls -l
cwd
exec -> entry point
io -> the io used by the process
task -> if it has multi thread it will be listed here what each thread doing


all the content in proc file not generated they were pulled from current running process linux shows like us it is file


iotop -> show who using most io in porcess

`/proc/net` directory in Linux contains various files and subdirectories that provide information related to networking
- **tcp**, **udp**, **udp6**, **tcp6**: These directories contain information about active TCP and UDP connections. in hexdecimal format
- `cat /proc/net/ip/ip_forward` -> has 0 or 1. Zero mean it won't forward the packet 1 mean it act as router turn off when it not need to increase the performance.

`PID/fd`  -> tell how many files are opened by the files

`cat PID/status` - have the current status of the process and 
`voluntary_ctxt_switches` and `nonvoluntary_ctxt_switches` – this tells you how many times the process was taken off CPU (or put back).

cmd to print ip of process
```
cat /proc/$(pidof java)/net/tcp \ | awk -v DPORT=$(printf ":%x" 9042) '$3 ~ DPORT { print $3}' \ | sort -u \ | cut -f1 -d':' \ | awk '{gsub(/../,"0x& ")} OFS="." {for(i=NF;i>0;i--) printf "%d%s", $i, (i == 1 ? ORS : OFS)}'
```

```
This document describes the interfaces /proc/net/tcp and /proc/net/tcp6.
Note that these interfaces are deprecated in favor of tcp_diag.

These /proc interfaces provide information about currently active TCP
connections, and are implemented by tcp4_seq_show() in net/ipv4/tcp_ipv4.c
and tcp6_seq_show() in net/ipv6/tcp_ipv6.c, respectively.

It will first list all listening TCP sockets, and next list all established
TCP connections. A typical entry of /proc/net/tcp would look like this (split
up into 3 parts because of the length of the line):

   46: 010310AC:9C4C 030310AC:1770 01
   |      |      |      |      |   |--> connection state
   |      |      |      |      |------> remote TCP port number
   |      |      |      |-------------> remote IPv4 address
   |      |      |--------------------> local TCP port number
   |      |---------------------------> local IPv4 address
   |----------------------------------> number of entry

   00000150:00000000 01:00000019 00000000
      |        |     |     |       |--> number of unrecovered RTO timeouts
      |        |     |     |----------> number of jiffies until timer expires
      |        |     |----------------> timer_active (see below)
      |        |----------------------> receive-queue
      |-------------------------------> transmit-queue

   1000        0 54165785 4 cd1e6040 25 4 27 3 -1
    |          |    |     |    |     |  | |  | |--> slow start size threshold,
    |          |    |     |    |     |  | |  |      or -1 if the threshold
    |          |    |     |    |     |  | |  |      is >= 0xFFFF
    |          |    |     |    |     |  | |  |----> sending congestion window
    |          |    |     |    |     |  | |-------> (ack.quick<<1)|ack.pingpong
    |          |    |     |    |     |  |---------> Predicted tick of soft clock
    |          |    |     |    |     |              (delayed ACK control data)
    |          |    |     |    |     |------------> retransmit timeout
    |          |    |     |    |------------------> location of socket in memory
    |          |    |     |-----------------------> socket reference count
    |          |    |-----------------------------> inode
    |          |----------------------------------> unanswered 0-window probes
    |---------------------------------------------> uid

timer_active:
  0  no timer is pending
  1  retransmit-timer is pending
  2  another timer (e.g. delayed ack or keepalive) is pending
  3  this is a socket in TIME_WAIT state. Not all fields will contain
     data (or even exist)
  4  zero window probe timer is pending
```


Mem file 

Maps file

``55c58ca07000-55c58ca0c000 r--p 00000000 fd:01 6815950  /usr/bin/dash``

- **Address Range (`55c58ca07000-55c58ca0c000`)**: This shows the range of virtual memory addresses that this mapping covers. The first address (`55c58ca07000`) is the starting address, and the second address (`55c58ca0c000`) is the ending address. The memory in this range is accessible to the process.

- **Permissions (`r--p`)**: Indicates the permissions for this memory region:
	- `r`: Readable
    - `-`: Not writable
    - `p`: Private (not shared with other processes)

- **Offset (`00000000`)**: This is the offset within the file or device that the mapping is backed by. For executable files, this typically represents the offset within the file where this section resides.
    
- **Device (`fd:01`)**: This specifies the device (file descriptor) that backs this mapping. In this case, `fd:01` suggests a file descriptor associated with a file.
    
- **Inode (`6815950`)**: The inode number of the file that backs this mapping. This uniquely identifies the file within the filesystem.
    
- **File Path (`/usr/bin/dash`)**: The path to the file or device that backs this memory mapping. In this case, it's `/usr/bin/dash`, which is the executable file of the `dash` shell.

### Common Types of Memory Regions:

- **Code (`r-xp`)**: Executable code of the program.
- **Data (`rw-p`)**: Read-write data.
- **Read-only data (`r--p`)**: Read-only data such as constants or strings.
- **Heap (`rw-p`)**: Dynamically allocated memory.
- **Stack (`rw-p`)**: The process's stack, where local variables and function call information are stored.
- **Shared libraries (`r-xp` or `r--p`)**: Memory regions mapped from shared libraries.





EBPF

`man bpf` -> The  bpf()  system  call  performs a range of operations related to ex‐
tended Berkeley Packet Filters.  Extended BPF (or eBPF) is  similar  to the  original  ("classic")  BPF  (cBPF) used to filter network packets. For both cBPF and eBPF programs, the  kernel  statically  analyzes  the programs  before loading them, in order to ensure that they cannot harm the running system.

 eBPF extends cBPF in multiple ways, including the  ability  to  call  a
fixed set of in-kernel helper functions (via the BPF_CALL opcode extension provided by eBPF) and access shared data structures such  as  eBPF maps.

BCC make bpf program easier to write


iotop -> tell which process doing more io

EBF  -> take a system call 

EBF projects
1. Katran by facebook (l4 loadbalancer)
2. cillium (networking sercurity and load balancing for k8)
3. bcc,bpftrace (netflix perf troubleshooting)
4. android security (google andrioid bpf)
5. traffice optimizaiton cloudfare
6. falco container runtime security 


https://tanelpoder.com/psnapper/


BCC allow python to write epbf



CMDs
1. ps -eLf | awk '{print $8}' | sort | uniq -c | sort -nr
2.  ps -eLo state,user | sort | uniq -c | sort -nr
3. **ps -eo s,user | grep ^[RD] | sort | uniq -c | sort -nbr | head -20** -> Runnable state and **Uninterruptible** sleep
4.  **ps -eo s,user,cmd | grep ^[RD] | sort | uniq -c | sort -nbr | head -20**

- Check how many threads are in R state


Tool
- https://github.com/tanelpoder/0xtools (do profile on proc file for 5 sec and show the top result)
- https://github.com/iovisor/bcc

pchar -measures  the  characteristics/speed  of  the network path between two Internet hosts, on either IPv4 or IPv6 networks it use tracrroute. it tell bandwidth between each router


## lSOF
list open files
- lsof -i tcp:80 -> the process used port 80 -i -> internet
- lsof -P processid -> give all open files by the process



 ## USE method 
 Use mehod are used to find bottel necks in the system
 - _Utilization_ is the percentage of time that the resource is busy servicing work during a specific time interval
 - Saturation refers to the degree to which a resource is being overused.disk saturation measures the length of the disk queue, indicating how many I/O requests are waiting to be serviced.
 - _Errors_ in terms of the USE method refer to the count of error events

The first step in the USE method is to create a list of resources. in the system like cpu,memory, etc 
https://queue.acm.org/detail.cfm?id=2413037








The command `sync; echo 1 > /proc/sys/vm/drop_caches` is used to clear the cache in Linux.

Here's a breakdown of what each part of the command does:

- `sync`: This command flushes the file system buffer, ensuring that all data is written to disk.
- `echo 1 > /proc/sys/vm/drop_caches`: This command writes the value `1` to the file `/proc/sys/vm/drop_caches`, which is a special file in Linux that controls the cache behavior.





## Storage

Linux makes it easy to see the actual storage disks by representing them as files in the **/dev** directory. Each disk is identified by a series of letters.

- sd = storage disk
- a or b = first or second disk (it also counts c, d, etc.)
- 1 or 2 = partitions on the disk (it also counts higher)

The result is that /dev/sdb1 is storage device ( sd), second device ( b), first partition ( 1), or the first partition on the second storage device.

Use the ls command to display the contents of the /dev/disk directory. In Ubuntu, you can view storage devices by-id, by-path, etc. You should see at least one entry that reads sda. That’s the first storage disk in the system. When you add a second disk, it will be labeled as sdb. Other identifiers include the disk’s Universally Unique ID (UUID).

Partition
A partition is a defined storage area on a hard drive. Essentially, it's a way of dividing a physical disk into separate, distinct sections. Each partition can be treated as an **independent disk by the operating system.**

### isstat



## System performance steps

### Uptime
look for the load average field.  It represents the average number of processes that are waiting to be executed by the CPU over a certain period.

```
>uptime
09:37:45 up 13:09,  1 user,  load average: 0.13, 0.40, 0.50
```
1. **First number (0.13)**: The load average over the last 1 minute.
2. **Second number (0.40)**: The load average over the last 5 minutes.
3. **Third number (0.50)**: The load average over the last 15 minutes.

**Interpretation:**
- **0.0-0.5**: The system is idle or lightly loaded.
- **0.5-1.0**: The system is moderately loaded, with some processes waiting for CPU time.
- **1.0-2.0**: The system is heavily loaded, with many processes waiting for CPU time.
- **Above 2.0**: The system is extremely loaded, with a high risk of performance issues or crashes.

### vmstat

 provides a snapshot of the system's memory, swap, I/O, and CPU usage.
```
>vmstat
procs -----memory---------- ---swap-- -----io---- -system----cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 20291856 747012 5818664    0    0    36    50  234  224  7  2 91  0  0

CMD to print in proper table format
vmstat  | tail -n 1 | awk '{print "| procs | r (run queue) | "$1" |\n| procs | b (uninterruptible sleep) | "$2" |\n| memory | swpd (swap used) | "$3" |\n| memory | free (free memory) | "$4" |\n| memory | buff (buffer memory) | "$5" |\n| memory | cache (cache memory) | "$6" |\n| swap | si (swap in) | "$7" |\n| swap | so (swap out) | "$8" |\n| io | bi (blocks read) | "$9" |\n| io | bo (blocks written) | "$10" |\n| system | in (interrupts) | "$11" |\n| system | cs (context switches) | "$12" |\n| cpu | us (user mode) | "$13"% |\n| cpu | sy (system mode) | "$14"% |\n| cpu | id (idle) | "$15"% |\n| cpu | wa (waiting for I/O) | "$16"% |\n| cpu | st (steal mode) | "$17"% |"}'

```

**procs**
- `r`: The number of processes waiting for CPU time (run queue).
- `b`: The number of processes in uninterruptible sleep (usually waiting for I/O). 
**memory**
- `swpd`: The amount of swap space used (in kilobytes). 
- `free`: The amount of free memory available (in kilobytes).
- `buff`: The amount of memory used for buffers (in kilobytes).
- `cache`: The amount of memory used for caching (in kilobytes). 
**swap**
- `si`: The number of kilobytes swapped in from disk per second. 
- `so`: The number of kilobytes swapped out to disk per second. 
**io**
- `bi`: The number of blocks read from disk per second. 
- `bo`: The number of blocks written to disk per second.
**system**
- `in`: The number of interrupts per second.
- `cs`: The number of context switches per second. 
**cpu**
- `us`: The percentage of CPU time spent in user mode. 
- `sy`: The percentage of CPU time spent in system mode.
- `id`: The percentage of CPU time spent idle. 
- `wa`: The percentage of CPU time spent waiting for I/O. 
- `st`: The percentage of CPU time spent in steal mode (usually due to virtualization).


TO get no of system call used by the program

`sudo strace  -c  node filename.js` -> will print all sys call and time it take

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
`If you’re using a DHCP address and the DHCP server provides a DNS setting, the
DHCP server will replace the contents of the file when it renews the DHCP address.

The hosts file is located at /etc/hosts, and kind of as with
DNS, you can use it to specify your own IP address–domain name mapping.
In other words, you can determine which IP address your browser goes to
when you enter www.microsoft.com (or any other domain) into the browser,
rather than let the DNS server decide. As a hacker, this can be useful for
hijacking a TCP connection on your local area network to direct traffic to a
malicious web server with a tool such as dnsspoof .
From the command line, type in the followin`


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

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
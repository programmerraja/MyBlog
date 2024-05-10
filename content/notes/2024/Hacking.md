+++
title = 'Hacking'
date = 2024-01-25T07:41:37.3737+05:30
draft = true
tags =[]
+++ 


WIfi hacking
- sudo bettercap -iface etho
- ARP spoffing (to get in man in middle attack between the WIFI)

Evil twin attack
WIFI PINEAPPLE
dnsmasq
hostpad conf

air crack ng airodump ng  -> capture the password send by other device and try to guess it 
deauth -> make already connected one to disconnect and listen for the password

cewl ,pipal ->  tool to guess password from there website and so on 


Burpsuite toutorial
1. https://hacklido.com/blog/628-burpsuite-101-exploring-burp-repeater-and-burp-comparer


#### Reverse shell
- [Revershell generator](https://www.revshells.com/) 
- [Fancy reverse and bind shell handler](https://github.com/calebstewart/pwncat)
- 
- 

#### open-source intelligence tools

- Maltego
- Mitaka
- SpiderFoot
- Spyse
- BuiltWith
- Intelligence X
- DarkSearch.io
- Grep.app
- Recon-ng
- theHarvester
- Shodan
- Metagoofil
- Searchcode
- SpiderFoot
- Babel X


#### Tool
1. [A modular exploitation framework extensible with Lua](https://github.com/farinap5/Venera)
2. https://github.com/assetnote/surf [ SSRF vulnerabilities on Modern Cloud Environments.]
3. https://caido.io/ # A lightweight web security auditing toolkit brupsuite alternative
4. https://cylect.io/

## Networking
## Nmap

#### CMDS

- `nmap -iR 3` -> random ip scan be carefull don't use over our ISP will note
- `nmap --reason ip` -> tell the reason for the port open close does it recevie sync ack packet or what happen
- `nmap -V` -> verbose outut print log and double VV more verbose
- `nmap ip or ip/16 (CIDR notation)`  -> scan for all port in that ip
- `nmap --packet-trace ip`  -> print all packets transfered
- `nmap -F ip` -> scan for commonly used port
- `nmap -p portno1,etc ip` ->specfic port
- `nmap -sP ip` -> only ping (namp used to check first by ping)
- `nmap -PS ip` -> Send TCP sync on 
- `nmap -PA ip` -> Send TCP sync and act 
- `nmap --traceroute ip` -> print traceroute
- `nmap -R ip` -> reverse DNS 
- `nmap -sS ip` -> only tcp sync scan
- `nmap -O ip` -> os detection
- `nmap -sV ip` -> software used on the port
- `nmap -D RND:5 [target]` ->will perform a scan on the specified target IP address, while also including 5 randomly selected decoy IP addresses to disguise the true source of the scan. be careful with ISP
- 

#### Port state
1. **Open**: This means that the scanned port is accessible, and there is an application or service actively accepting connections on that port. An open port can indicate a potential entry point for communication with a system.
    
2. **Closed**: A closed port means that there is no application listening on that port. The system actively responds with a TCP RST packet for TCP scans or with an ICMP port unreachable message for UDP scans, indicating that the port is closed and no connection is possible.
    
3. **Filtered**: This state means that `nmap` was unable to determine whether the port is open or closed. This typically happens when a firewall or other network filtering device prevents `nmap` from making a reliable determination of the port's state. `nmap` cannot reliably tell whether the port is open or closed, so it reports the port as filtered.
    
4. **Unfiltered**: This state indicates that the port is accessible, but `nmap` cannot determine whether it is open or closed. It means that `nmap` was able to establish that the port is not firewalled, but it does not provide enough information to determine whether there is an active service listening on that port.
    
5. **Open|Filtered**: This state indicates that `nmap` cannot determine whether the port is open or filtered. It could mean that there is a firewall or similar device blocking the port, or it could mean that there is a service actively listening on the port.
    
6. **Closed|Filtered**: This state means that `nmap` cannot determine whether the port is closed or filtered. It could indicate a firewall or network filtering device that prevents `nmap` from making a determination about the port's state.

Zenmap is GUI for nmap

#### Scanning
- [Invisible network protocol sniffer](https://github.com/casterbyte/Above )
- 
## OSNIT 

#### Tools
- https://inteltechniques.com/tools/ (best contain all tools in one place)
- https://whatsmyname.app/ Enter the username(s) in the search box, select any category filters & click the search icon or press CTRL+Enter
- [waybackmachine](https://web.archive.org/web/20030315000000*/http://sep11.wikipedia.org/wiki/In_Memoriam) 
- https://urlscan.io/ by default if we scan it can be access by anyone try to scan in private mode. if you suspect the url use this website
- https://www.virustotal.com/gui/ 
- https://dnsdumpster.com/   DNS Lookup read all record and show in graph format 
- https://www.nmmapper.com/
- https://chromewebstore.google.com/detail/instant-data-scraper/ofaokhiedipichpaobibbnahnkdoiiah
- [Free online free chrome browser](https://kasmweb.com/ )
- https://epieos.com/  tool for email and phone reverse lookup
- [SpiderFoot](https://github.com/smicallef/spiderfoot ) automates OSINT for threat intelligence and mapping your attack surface.
- https://www.osintcombine.com/
- 
#### Resources
1. https://www.myosint.training/ free course
2. https://start.me/p/DPYPMz/the-ultimate-osint-collection


People
- https://github.com/WebBreacher


Scanning tool
- https://null-byte.wonderhowto.com/how-to/hack-like-pro-scan-for-vulnerabilities-with-nessus-0169971/ 


Phising
- https://github.com/kgretzky/evilginx2 Standalone man-in-the-middle attack framework used for phishing login credentials along with session cookies, allowing for the bypass of 2-factor authentication
- keylogger https://github.com/Geeoon/DNS-Tunnel-Keylogger


https://dnsdumpster.com/footprinting-reconnaissance/
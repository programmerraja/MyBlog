+++
title = 'Wireshark Notes'
date  = 2024-01-17T08:19:32.3232+05:30
draft = false
+++


Wireshark
- help -> content have doc for all feature


Analysis TCP

Stream index : calculate by wireshark to keep track of stream 
Sequence number Will show relative sequence number 1,2 to see the original right click -> protocol prefrences -> uncheck relative sequence number.
flags  ack,reset,etc.
window size value
calculated window size -> host window size
checksum : wireshark not validate the check if we want ight click -> protocol prefrences -> check validate TCP check sum ifpossible
options max segement size

to see the sync ack flow graph

statistics ->flow graph


Analysis UDP

It just show as in format digram


IPV6

No broadcast
no fragment
ttl -> hop limit

ICMP
used for error reporting and query


DNS
transaction id unique id for each query
flags 
questions : 1 (no of questions)
queries  hold the queries that we ask to DNS

in response

Answer PRS  : 2 ->no of answer

answers -> hold the answers for the questions

DHCP:
(DORA)
Discover
offer
request 
ACK


Expert system
- help to alert the admin on possible issue (will show the color on bottom left corner )
- Red  Error
- yellow possible problem
- green need to be noted
- Blue typical workflow


- zero window -> buffer is full 
- dupicate ACK

TCP stream graph

export object from the packet data file -> export -> choose the item

Tshark cmd line tool

how decode ssl

congestion 

WIFI

3 type of frame
1. management frame (beacon)
2. COntrol frrame
3. data frame

window cmd line tool
1. netsh trace 
2. dumpcap 
3. tshark

Io graph
1. Statistsc -> Io graph choose the filter,x axis,y axis and see in visula manner we can add our own filter and x axis and y axis

converstation
 1. between 2 endpoints
 2. we can choose graph to see round trip time, throughput, window scaling

TCP stream graph
1. Stevens graph tcp sequence number over time
2. tcptrace visulaize TCP metrics
3. throughput average through
4. round tripe time
5. window scalling window size and outstanding bytes 

TCP options
timestamps 
max segement size
selective ack
nop


voice over ip

Real time transport protocol for media
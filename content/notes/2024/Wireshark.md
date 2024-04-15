+++
title = 'Wireshark Notes'
date  = 2024-01-17T08:19:32.3232+05:30
draft = true
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

[] -> data wrap with this add by wireshark

Delta time -> preferejnce -> column add (will tell sec between tow packet)

voice over ip

Real time transport protocol for media


Right click -> conversation filter -> give the converstion between the IP in order


Duplicate ACKS


client
Seq = 0 
ack = -

server
Ack =client seq+1
seq = 0 (his seq number)

server (sending data)
seq = 0 
ack = seq+1

client
ack = server_seq+len
seq = 0 (when ever client send data only the client seq get increased)






Time to live ->  (when server or client sent it start with 64 or 128 or255 ) so on reveceive pakcet check what is the TTL to find how may routes it taked.


In TCP, when there's a gap in the received sequence numbers, the receiver uses several mechanisms to handle the situation and ensure reliable data transfer. Let's use your example to illustrate how TCP handles a missing segment:

1. **Initial Transmission:**
   - Sender (Host A) sends TCP segments with sequence numbers 1 to 2, 4 to 6, 6 to 8, and 8 to 10.
   - Receiver (Host B) successfully receives and acknowledges segments 1 to 2, 4 to 6, 6 to 8, and 8 to 10.

   ```
   Host A        ----->   Host B
   SEQ: 1-2              ACK: 3
   SEQ: 4-6              ACK: 7
   SEQ: 6-8              ACK: 9
   SEQ: 8-10             ACK: 11
   ```

2. **Missing Segment (2 to 4):**
   - Segment with sequence numbers 2 to 4 is not received by Host B.

3. **Receiver's Perspective:**
   - Host B detects the missing segment and sends a Duplicate ACK for the last correctly received segment (ACK: 3 in this case).

   ```
   Host A        <-----   Host B
   SEQ: 1-2              ACK: 3 (DUP ACK)
   SEQ: 4-6
   SEQ: 6-8
   SEQ: 8-10
   ```

4. **Retransmission Triggered:**
   - Host A, upon receiving the Duplicate ACK, realizes that Host B is missing segment 2 to 4.
   - Host A retransmits the missing segment (2 to 4).

   ```
   Host A        ----->   Host B
   SEQ: 2-4              ACK: 7
   ```

5. **Subsequent Acknowledgments:**
   - Host B acknowledges the retransmitted segment (ACK: 5).

   ```
   Host A        <-----   Host B
   SEQ: 2-4              ACK: 5
   ```

Now, the TCP sender and receiver have cooperatively resolved the issue caused by the missing segment. This process demonstrates how TCP uses acknowledgments, duplicate acknowledgments, and retransmissions to recover from packet loss and ensure the reliable delivery of data.





MTU link layer -> Maximum Transmission Unit  maximum size of an IP packet that can be sent over a link 
Fragmentation is allowed by MTU. If a packet is larger than the MTU, it is split up into smaller parts.

MTU is set on each router when the packet come big then that it will do fragmentation

MSS stands for Maximum Segment Size. It is the maximum amount of data that can be sent in a single TCP segment.

The Transmission Control Protocol (TCP) Maximum Segment Size (MSS) defines the maximum amount of data that a host accepts in a single TCP/IPv4 datagram.

This TCP/IPv4 datagram is possibly fragmented at the IPv4 layer. The MSS value is sent as a TCP header option only in TCP SYN segments.

Each side of a TCP connection reports its MSS value to the other side. The MSS value is _not_ negotiated between hosts.

A TCP option called MSS is negotiated as part of the handshake. The IP header and the TCP header are not included.

Max can be 1460







TCP Sack (selective ack)

let say we have send 1 to 100 100 to 200  and then 200 to 300 . 
let say client not receview the 100 to 200 client sent ACK for 100 with sak having left edge 100 and right edge as 200 (where it marked as dup ack)


filters

- `tcp.analysis.flags && !tcp.analysis.window_update` -> wil list all retransmit tcp ,duplicate and window update packet
- `tcp.flags.reset` -> the packet from server where server is not listening on the port
- `http.time > 0.25` -> the http that take more then 25 milisec
- 




Give OSII 

Any idea about OSII layer


![[Pasted image 20240303172632.png]]


RFC

ip.dst == 104.22.71.140  or ip.src == 104.22.71.140 and ssl
TLS between http and TCP

ip.dst == 104.22.71.140 or ip.src ==  104.22.71.140 or ip.src == 172.67.42.249 or ip.dst == 172.67.42.249

 ip.src == 172.67.42.249 or ip.dst == 172.67.42.249


# TLS 1.2 Handshake

It takes 4 steps to complete the handshake before sending the first encrypted request from a browser:

1. Client Hello
2. Server Hello
3. Client key exchange and generate the master secret
4. Finished


https://cabulous.medium.com/tls-1-2-andtls-1-3-handshake-walkthrough-4cfd0a798164

![[Pasted image 20240303190219.png]]

HTPP2

https://cabulous.medium.com/http-2-and-how-it-works-9f645458e4b2


  
SSL/TLS session keys and the server's private key serve different purposes in the context of secure communication over the internet.

1



SS0

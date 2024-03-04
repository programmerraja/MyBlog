+++
title = 'Networking Notes'
date  = 2024-01-12T08:19:32.3232+05:30
draft = false
+++

## OSI Model

The Open Systems Interconnection (OSI) model describes seven layers that computer systems use to communicate over a network.

1. **Application Layer** (Htttp,DHCP,FMTP,Telnet,NFS)
2. **Presentation Layer** (Encryption,Data format,Data compression,ASCII to Bin)
3. **Session Layer** (TCP/IP socket authentication RTCP protocol)
4. **Transport Layer** (Process level addressing TCP/UDP, Multiplexing,demultiplexing,segmentation,connection establishment)
5. **Network Layer** (logical addressing IP , Routing, packet encapsulation)
6. **Data Link Layer** (LAN,MAC,Data framing,Error detection)
7. **Physical Layer** (Converting Bin to singals to transfer the data)

## Application Layer

#### HTTP/HTTPS

Is the protocol used to communicate with server. it underling using TCP protocol

#### HTTP Message format

| **Start Line or Status Line** | GET /example HTTP/1.1 |
| ----------------------------- | -------------------------------------------------------- |
| **Headers**                   | General Headers (e.g., Cache-Control)                     |
|                               | Request Headers (e.g., User-Agent)                        |
|                               | Response Headers (e.g., Content-Type)                     |
|                               | Entity Headers (e.g., Content-Length)                     |
| **(Empty line)**              |                                                          |
| **Message Body**              | Request Body (for Request) / Response Body (for Response) |
message consists of a start-line followed by a CRLF(carriage-return and linefeed) and a sequence of octets in a format similar to the Internet Message Format

example
```plaintext

GET / HTTP/1.1 CRLFHost: google.comCRLFCRLF

response would look like:

HTTP/1.1 200 OK CRLFServer: googleCRLFContent-Length: 100CRLFtext/html; charset=UTF-8CRLFCRLF<100 bytes of data>

will be decoded as

Http/1.1 200 ok
server:google
Content-Length: 100
charset=UTF-8
<100 bytes of data>
```

because of this we cannot do multiplexed two request need to send in ordered and response need to be in ordered.

If the client sends Request 1, Request 2, and then Request 3 over a single TCP connection, it will receive Response 1, Response 2, and then Response 3 in the same order.

#### Methods of HTTP

1. **GET** : Used to request data from a specified resource.

2. **PUT**: Used to submit data to be processed to a specified resource.

3. **POST**: Used to update a resource or create a new resource if it doesn't exist.

4. **DELETE**: Used to request the removal of a resource.

5. **OPTION**: The OPTIONS method is used to inquire about the communication options available for a target resource. It is often employed to check the allowed methods or capabilities of a server. `Note`: Whenever in browser requesting different origin(test.com) from the current webpage origin(google.com) browser first do option request to check does the origin (test.com) allowing this origin(google.com) to acess 

6. **TRACE** :The TRACE method is designed for diagnostic purposes. When a server receives a TRACE request, it echoes the received request back to the client. This can be useful for debugging and understanding how intermediaries (such as proxies or servers) modify the request.

7. **HEAD**: Similar to GET but only requests the headers of the response, not the actual data.

8. **PATCH**:Used to apply partial modifications to a resource

#### HTTP Status code range

| Status Code Range | Category          | Description                                     |
| ------------------ | ----------------- | ----------------------------------------------- |
| 1xx               | Informational     | Request received, continuing process            |
| 2xx               | Successful        | The request was successfully received and processed |
| 3xx               | Redirection       | Further action needs to be taken to complete the request |
| 4xx               | Client Error      | The request contains bad syntax or cannot be fulfilled by the server |
| 5xx               | Server Error      | The server failed to fulfill a valid request     |

#### HTTP Cache

HTTP caching helps reduce latency, minimize network usage, and improve the overall user experience by avoiding unnecessary resource re-fetching.

**How Browser do caching**

- Broweser do caching only if the response contain the `Cache-Control` header with value `max-age=seconds` specifies that the resource is valid for that much seconds 
- `ETag` is a unique identifier assigned by the server to a specific version of a resource. When a client requests the resource again, it can include the ETag in the request headers. If the resource hasn't changed, the server may respond with a 304 Not Modified status

#### HTTP2

Is the improvement of the http and have some key features like below

1. **Multiplexing:**  HTTP/2 allows multiple requests and responses to be sent and received concurrently on a single connection, improving efficiency and reducing latency.
    
2. **Header Compression:** HTTP/2 uses a more efficient header compression algorithm called HPACK. 
3. **Binary Protocol:** While HTTP/1.1 uses plain text for communication, HTTP/2 employs a binary protocol. This binary format is more compact and efficient
    
4. **Server Push:** HTTP/2 supports server push, which allows the server to send additional resources (like images, stylesheets, or scripts) to the client before the client requests them.
    
5. **Stream Prioritization:** Allows the client to indicate the priority of each resource. 
    
6. **Connection Multiplexing:** HTTP/2 uses a single, multiplexed connection per origin.

 **HTTP/2 frames** that have type, length, flags, stream identifier (ID) and payload. The stream ID makes it clear which bytes on the wire apply to which message, allowing safe multiplexing and concurrency. Streams are bidirectional. Clients send frames and servers reply with frames using the same ID.
 
Client requests always use odd-numbered stream IDs, so subsequent requests would use stream IDs 3, 5, and so on. Responses can be served in any order, and frames from different streams can be interleaved.

A server that is unable to establish a new stream identifier can send a **GOAWAY** frame so that the client is forced to open a new connection for new streams

#### Format

```
+-----------------------------------------------+
|                 Frame Header                  |
+-----------------------------------------------+
|                 Frame Payload                 |
+-----------------------------------------------+
```

**Frame Header:** Represents the common structure at the beginning of each frame. It includes fields such as:

- Length: The length of the frame payload.

- Type: The type of the frame (e.g., HEADERS, DATA, SETTINGS, etc.).

	1. **HEADERS Frame (Type 0x1):**
	    - Carries header fields for a particular stream.
	    - Can be used for request or response headers.
	2. **PRIORITY Frame (Type 0x2):**
	    - Indicates the sender's priority weighting for a stream.
	    - Helps in managing the order of processing streams.
	3. **RST_STREAM Frame (Type 0x3):**
	    - Indicates that a stream is being terminated or reset.
	    - Includes an error code indicating the reason for termination.
	    - this will send when we navigate to another page browser will send to free up resources in server that no need anymore
	1. **SETTINGS Frame (Type 0x4):**
	    - Used to communicate configuration settings for both the client and server.
	    - Parameters include initial window size, maximum frame size, etc.
	2. **PUSH_PROMISE Frame (Type 0x5):**
	    - Sent by the server to initiate a server push.
	    - Specifies a promised request and the associated stream identifier.
	3. **PING Frame (Type 0x6):**
	    - Used to measure a round-trip time between endpoints.
	    - Primarily used as a keep-alive mechanism.
	4. **GOAWAY Frame (Type 0x7):**
	    - Sent by a server to indicate that further communication should not occur on a given connection.
	    - Includes information about the last stream ID processed.
	5. **WINDOW_UPDATE Frame (Type 0x8):**
	    - Used to adjust the size of the flow-control window.
	    - Can be sent by both the client and the server.
	6. **CONTINUATION Frame (Type 0x9):**
	    - Used to continue a sequence of header block fragments.
	    - Allows header information to be spread across multiple frames.
	7. **DATA Frame (Type 0x0):**
	    - Used to carry the payload of an HTTP message, such as the content of a request or response body.
	    - It can be associated with a specific stream.

- Flags: Flags providing additional information about the frame.
	 1. **END_STREAM (0x1):**
	    - Indicates that the frame represents the end of a stream.
	    - Applies to HEADERS, DATA, and CONTINUATION frames.
	2. **END_HEADERS (0x4):**
	    - Indicates that this frame contains the final chunk of a header set.
	    - Applies to HEADERS, CONTINUATION, and PUSH_PROMISE frames.
	3. **PADDED (0x8):**
	    - Indicates that the frame is followed by padding.
	    - The length of the padding is determined by the value of the Pad Length field in the frame.
	4. **PRIORITY (0x20):**
	    - Indicates that the frame includes stream priority information.
	    - Applies to HEADERS, PUSH_PROMISE, and CONTINUATION frames.
	5. **ACK (0x1):**
	    - Indicates that the SETTINGS frame acknowledges the receipt and application of the peer's settings.
	    - Applies to SETTINGS frames.
- Stream Identifier: Identifies the stream to which the frame belongs.

**Frame Payload:** Represents the specific content of the frame, which varies based on the frame type. For example:

- For a HEADERS frame, the payload includes header information.
- For a DATA frame, the payload includes the actual data being sent.

 **Rapid resets leading to denial of service**

The challenge occurs when a client rapidly cancels many requests in an HTTP/2 environment, and the server or intermediary (like a proxy) struggles to promptly handle these cancellations. This can result in a buildup of tasks, causing resource consumption issues, especially in scenarios where there's a delay in cleaning up in-process jobs.
refere [here](https://blog.apnic.net/2024/01/11/http-2-rapid-reset-deconstructing-the-record-breaking-attack/) 
#### HTTP3
 It uses QUIC in transport layer 
#### Security Header in HTTP

1. **Content-Security-Policy (CSP):**
    - Defines a policy to control resource loading, mitigating risks such as cross-site scripting (XSS) attacks.
    - `Content-Security-Policy: default-src 'self';` specifies that content such as scripts, stylesheets, and images should be loaded only from the same origin.
    - Example : let assume some how hacker inject the script tag to our website but if we have the CSP it will not be loaded by browser becasue we mentioned load only from self origin
2. **Access-Control-Allow-Origin:**
	- Specifies which origins are permitted to access the resource
	- `Access-Control-Allow-Origin: https://www.example.com`  this will inform the browser that only example.com can be allow to access using fetch or any API interface methods 
	- Similar to this there some other header presents like `Access-Control-Allow-Methods` , `Access-Control-Allow-Headers`, `Access-Control-Allow-Credentials` etc..
3. **Strict-Transport-Security (HSTS):**
    - Forces secure connections (HTTPS) by instructing browsers to interact with the website only over secure connections.
4. **X-Content-Type-Options:**
    - Prevents browsers from interpreting files as a different MIME type, reducing the risk of certain attacks.
5. **X-Frame-Options:**
    - Controls whether a browser should be allowed to render a page in a frame, mitigating clickjacking attacks.
    - `X-Frame-Options: DENY` This tell the browser that this website is not allowed to inject in iframe 
    - `SAMEORIGIN` This tell the browser allow iframe in the same origin 
6. **X-XSS-Protection:**
    - Enables or disables the browser's built-in Cross-Site Scripting (XSS) protection.
7. **SameSite** 
    - `'strict'` make sure the cookie will be send only the respective origin that set the cookie


#### HTTPS
is a secure version of HTTP.

 **How the HTTPS connection flow**
 
- The browser establishes a TCP (Transmission Control Protocol) connection with the web server's IP address on port 443, which is the default port for HTTPS.

- The browser initiates the SSL/TLS (Secure Sockets Layer/Transport Layer Security) handshake process by sending a `ClientHello` message to the server.

- The web server responds with its SSL/TLS certificate, which includes the server's public key, the server's identity, and the digital signature of a trusted Certificate Authority (CA).

- The browser verifies the authenticity of the server's certificate. It checks if the certificate is signed by a trusted CA, is not expired, and matches the domain in the URL.

- The browser generates a symmetric session key, encrypts it with the server's public key, and sends it back to the server. This session key will be used for encrypting and decrypting data during the session.

- The server sends a `Finished` message to signal that the initial handshake is complete.

- The browser sends its own `Finished` message, confirming the completion of the handshake.

### SSL flow
-  The client sends a ClientHello message to which the server must respond with a ServerHello message, or else a fatal error will occur and the connection will fail.  The ClientHello and ServerHello are used to establish security enhancement capabilities between client and server.
-  The ClientHello and ServerHello establish the following attributes: `Protocol Version, Session ID, Cipher Suite, and Compression Method.  Additionally, two random values are generated and exchanged: ClientHello.random and ServerHello.random.`
- Following the hello messages, the server will send its certificate in a Certificate message if it is to be authenticated.
- Client vaildate the certificate and use the server public key to encyrpt the content such as session key and initiate the session 
	Note : session key (which is symmentric key used to decrypt and encrypt the msg after TLS handshake done. symmentric is used because it is faster then asymmentric)
- After server recevie the session key and it ack .
-
Resource
- https://www.linuxbabe.com/security/ssltls-handshake-process-explained-with-wireshark-screenshot


 **What is Server Certificate**
 
The server sends its digital certificate to the client. The certificate contains:
- **Public Key:** Used for encrypting the session key.
- **Server's Identity (Common Name):** The domain name for which the certificate was issued.
- **Issuer:** The CA that issued the certificate.
- **Digital Signature:** A signature by the CA, confirming the authenticity of the certificate.

 **Who is CA**

A Certificate Authority is a trusted entity that issues digital certificates.The process involves the following steps:

 1. **Certificate Request:** A website owner generates a Certificate Signing Request (CSR) containing their public key and other identification information. The CSR is a file containing essential information about the website and its public key. 
	 1. `openssl genpkey -algorithm RSA -out private-key.pem` will  generate a public-private key pair
	 2. `openssl req -new -key private-key.pem -out mysite.csr` With the private key create a CSR
	 3. Note : LetsEncrypt scripts use OpenSSL to generate certificates and sign them with the LetsEncrypt service.

2. **CSR Submission:** The website owner submits the CSR to a CA.

 3. **Verification:** The CA verifies the identity of the entity requesting the certificate. it check does we really owing the domain. they give the file that we need to serve on our domain they check does the file is serving on our domain.

 4. **Certificate Issuance:** Upon successful verification, the CA issues a digital certificate, signing it with the CA's private key.

 5. **Distribution to the Website:**  The CA sends the issued certificate to the website owner.
 
 
 **What is SSL/TLS?**
 
 SSL/TLS relies on public-key cryptography for secure key exchange and data encryption.it has two key public and private key public to encrypt and private to decrypt the data.

`Note`: TLS is considered the more secure and modern protocol.


#### SMTP 
Simple Mail Transfer Protocol used in the Internet to transfer electronic mail (email).

```
	              +----------+                +----------+
      +------+    |          |                |          |
      | User |<-->|          |      SMTP      |          |
      +------+    |  Client- |Commands/Replies| Server-  |
      +------+    |   SMTP   |<-------------->|    SMTP  |    +------+
      | File |<-->|          |    and Mail    |          |<-->| File |
      |System|    |          |                |          |    |System|
      +------+    +----------+                +----------+    +------+
                   SMTP client                SMTP server
```

When an SMTP client has a message to transmit, it establishes a two-way transmission channel to an SMTP server.  The responsibility of an SMTP client is to transfer mail messages to one or more SMTP servers, or report its failure to do so.

 An SMTP client determines the address of an appropriate host running an SMTP server by resolving a destination domain name to either an intermediate Mail eXchanger host or a final target host.

**The SMTP Procedures: An Overview**

An SMTP session is initiated when a client opens a connection to a server and the server responds with an opening message.

 Once the server has sent the greeting (welcoming) message and the client has received it, the client normally sends the EHLO command to the server, indicating the client's identity.

 There are 3 steps to **SMTP mail transactions**.  
 1. The transaction starts with a MAIL command that gives the sender identification. ` MAIL FROM:<reverse-path> [SP <mail-parameters> ] <CRLF>` . This command tells the SMTP-receiver that a new mail transaction is starting and to reset all its state tables and buffers
 2.  The second step in the procedure is the RCPT command. `  RCPT TO:<forward-path> [ SP <rcpt-parameters> ] <CRLF>`  The first or only argument to this command includes a forward-path identifying one recipient.  If accepted, the SMTP server returns a "250 OK" reply and stores the forward-path.
 3. The third step in the procedure is the DATA command `DATA <CRLF>`  If SMTP accepted, the SMTP server returns a "250 OK" reply


**How Mail Routed to the destination**

SMTP will do DNS query to lookup for the Mail eXchanger (MX) records for the sender domain. MX records hold the IP for the mail servers that are designated to handle incoming emails for the domain.The MX records include priority values, indicating the order in which mail servers should be used. Lower values represent higher priority.

#### POP / IMAP
   
POP (Post Office Protocol) and IMAP (Internet Message Access Protocol) are two different protocols used for retrieving emails from a mail server to a local email client.

**POP(Post Offic Protocol):** Establish connection with SMTP server and send greeting once it accepted.client server exchange the mail.it downloads emails from the server to the local device.emails are removed from the server after being downloaded.

**[IMAP (Internet Message Access Protocol):](https://datatracker.ietf.org/doc/html/rfc3501)** allows a client to access and manipulate electronic mail messages on a server.  IMAP4rev1 permits manipulation of mailboxes (remote message folders) in a way that is functionally equivalent to local folders. IMAP4rev1 also provides the capability for an offline client to resynchronize with the server.


#### DNS 
Domain name system used to translate the domain to IP. It uses UDP protocol on port 57.
First browser check does the domain corresponding IP in local cache if so use if not do the DNS Query.

**How DNS Query get resolved**
- The resolver queries recursive DNS servers, which are typically provided by the Internet Service Provider (ISP) or a third-party service like Google's. These servers either have the IP address for the requested domain in their cache or initiate further queries to find it.
- If the recursive DNS servers don't have the information, they query root DNS servers.
- There are **13** sets of root DNS servers worldwide (labeled A to M), and they provide information about Top-Level Domains (TLDs) like **.com, .org, .net, etc.**
- The **Root DNS servers** direct the query to the authoritative DNS servers for the specific Top-Level Domain (TLD) of the requested domain.
- The **TLD DNS servers** provide information about the authoritative DNS servers for the second-level domain (e.g., "example.com").
- The **authoritative DNS servers** or **Name Server** (mostly our domain provider) respond to the query with the IP address associated with the requested domain.

**Records in DNS**
1. **A (Address) Record:** Associates a domain name with an IPv4 address. 
2. **AAAA (IPv6 Address) Record:** domain name with an IPv6 address
3. **CNAME (Canonical Name) Record:** alias for a domain name, pointing it to another domain's canonical (official) name  this can be used if you want a example.com domain to google.com we can add the CName record for it.
4. **MX (Mail Exchange) Record:** mail servers responsible for receiving emails on behalf of the domain.
5. **TXT (Text) Record:** text information associated with a domain. Often used for DNS-based verification or providing additional information.
6. **NS (Name Server) Record:** Specifies authoritative DNS servers for the domain
7. **Sender policy framework (SPF) record:** that lists all the servers authorized to send emails from a particular domain. when the recevier recevie mail from this domain it will check for the SPK record in sender domain address if the IP is in that list it will allow else block it or mark as spam.

`Note:` The DNS first Query for the A Record if it exist it use the IP else it look for the CName for the domain which will have a another domain which have A Record that will hold the IP of the server

#### DHCP

Dynamic Host Configuration Protocol is used to dynamically assign IP addresses and other network configuration information to devices on a network.

**How Client Communicate with DHCP**

- The client broadcasts a **DHCPDISCOVER** message on its local physical subnet.

- Each server may respond with a **DHCPOFFER** message that includes an available network address

- The client receives one or more **DHCPOFFER** messages from one or more servers.  The client may choose to wait for multiple responses. The client chooses one server from which to request configuration parameters, based on the configuration parameters offered in the DHCPOFFER messages.

- The client broadcasts a **DHCPREQUEST** message that MUST include the 'server identifier' option to indicate which server it has selected

- The server selected in the **DHCPREQUEST** message commits the binding for the client to persistent storage and responds with a DHCPACK message containing the configuration parameters are default gateway IP, subnetmask,DNS server address,least time (time for IP valid)
#### SSH 

Provides secure access to a remote device or server over a network, allowing for encrypted communication and command execution.

#### WebRTC

Peer to peer exchange video and audio 

STUN (session traversal util for NAT)
- A Server tell me my public ip address/port through NAT
- The client want to know his public ip and port (the ip of router)
- The clent will do STUN request to the STUN server where it send the response of the public ip and port

TURN (traversal using relay around NAT)
- 



#### VoIP
allows voice communication and multimedia sessions over the Internet.

VoIP devices register with a VoIP server using a protocol like SIP (Session Initiation Protocol). SIP is responsible for establishing, modifying, and terminating real-time sessions.

To connect with traditional phone networks, VoIP calls often go through an Internet Telephony Service Provider (ITSP). The ITSP serves as a gateway between the IP-based and traditional telephone networks.

**protocols commonly used in VoIP**

1. **SIP (Session Initiation Protocol):**
    - SIP is a signaling protocol used for initiating, modifying, and terminating communication sessions. It's widely used for setting up and tearing down VoIP calls.
2. **RTP (Real-Time Transport Protocol):**
    - RTP is used for transmitting audio and video data in real-time over the internet. It works in conjunction with RTCP (Real-Time Control Protocol) for quality monitoring.
3. **SDP (Session Description Protocol):**
    - SDP is used to describe multimedia sessions for the purpose of session announcement, invitation, and other forms of initiation.
4. **RTCP (Real-Time Control Protocol):**
    - RTCP works alongside RTP to provide control information during a VoIP session, including statistics on packet loss, jitter, and round-trip delay.
5. **UDP (User Datagram Protocol):**
    - UDP is a transport protocol commonly used for carrying voice packets in VoIP due to its low-latency characteristics. It's also used for SIP signaling.
6. **TCP (Transmission Control Protocol):**
    - While less common for voice transmission due to its connection-oriented nature, TCP is used in certain scenarios for SIP signaling and situations where retransmission of lost packets is preferred over real-time delivery.
7. **TLS (Transport Layer Security):**
    - TLS is used to secure the signaling (SIP) and media streams in VoIP, ensuring the confidentiality and integrity of communications.
8. **STUN (Session Traversal Utilities for NAT):**
    - STUN is used to discover the presence of network address translators (NATs) and to obtain the public IP address and port of a VoIP client.
9. **TURN (Traversal Using Relays around NAT):**
    - TURN is used to relay media when direct peer-to-peer communication is not possible due to NAT or firewall traversal issues.
10. **ICE (Interactive Connectivity Establishment):**
    - ICE is a framework that leverages STUN and TURN to facilitate the establishment of peer-to-peer connections in the presence of NAT and firewalls.
11. **MGCP (Media Gateway Control Protocol):**
    - MGCP is a protocol used for controlling media gateways in VoIP networks, often used in conjunction with the more modern SIP and H.323.

 **SIP Proxy Server**

A SIP proxy receives and processes SIP requests from a redirect server or software.

**Session Initiation Protocol (SIP)**

 SIP is an application-layer control protocol that can establish,modify, and terminate multimedia sessions (conferences) such as calls.
 
Responses are coded based on their message. Different preceding numbers in a three-digit sequence have different meanings.

For example, **1xx** response codes mean the device received and is processing the message. Codes starting with **2xx** mean completion, **3xx** is used for redirections, **4xx** is for authentication errors, etc.

The most common code is **200**, meaning the action was completed successfully without further details.

A SIP registrar is similar to an address book. It associates the various users with the access points on the IP network where one can reach them.

Most SIP addresses connect to a unique phone number

 underlying media streams for audio and video calls typically use separate protocols, like RTP (Real-time Transport Protocol), which ensures timely delivery of media data.

SIP URI 
It has a similar form to an email address, typically containing a username and a host name.  In this case, it is sip:bob@biloxi.com, where biloxi.com is the domain of Bob's SIP service provider.

**Codecs**

A codec, which stands for **coder-decoder**, converts an audio signal into compressed digital form for transmission and then back into an uncompressed audio signal for replay. 
#### Tools

**Nslookup**

tool for querying the Domain Name System to obtain the mapping between domain name and IP address,
- nslookup mydomain.com -> Will print the IP and other DNS details
- nslookup -type=AAAA mydomain.com -> will print only AAA record

**Domain Information Groper command (DIG)**

for Making DNS queries `dig example.com` , `dig NS com ` will print name server of the domain

## Transport Layer

It is responsible for ensuring end-to-end communication and data transfer between applications. In this layer the data is divided into small small segements and also it control the flow of the data transfer ,detecting packet losses (via sequence numbers) and errors (via per-segment checksums), as well as correction via retransmission

The two most commonly used transport layer protocols are:
1. TCP
2. UDP
#### TCP
Transmission Control Protocol is a connection-oriented protocol that provides reliable and ordered delivery of data between two devices. It establishes a connection before data transfer, breaks data into segments, manages acknowledgment and retransmission of data, and ensures that data is delivered without errors and in the correct order.

**sockets** doors through which data passes from the network to the process and through which data passes from the process to the network.

TCP use Port no and IP to bind the data to socket when multiple process running on the machine it uses the Port no and IP to bind with socket

TCP protocol runs only in the end systems and not in the intermediate network elements (routers and link-layer switches), the intermediate network elements do not maintain TCP connection state.

#### How TCP Works

- It first perfom 3 way hadshake. The client sends a TCP segment with the **SYN **(synchronize) flag set to the server, indicating an initiation of a connection.

- The server responds with a TCP segment containing **SYN-ACK**, acknowledging the request and indicating its readiness to establish a connection.

- The client sends a final TCP segment with an ACK (acknowledge) flag set, confirming the server's acknowledgment. The connection is now established.

```plaintext
TCP Peer A                                              TCP Peer B

1.  CLOSED                                               LISTEN

2.  SYN-SENT    --> <SEQ=100><CTL=SYN>               --> SYN-RECEIVED

3.  ESTABLISHED <-- <SEQ=300><ACK=101><CTL=SYN,ACK>  <-- SYN-RECEIVED

4.  ESTABLISHED --> <SEQ=101><ACK=301><CTL=ACK>       --> ESTABLISHED

5.  ESTABLISHED --> <SEQ=101><ACK=301><CTL=ACK><DATA> --> ESTABLISHED
```

- TCP Peer A begins by sending a **SYN** segment indicating that it will use sequence numbers starting with sequence number 100.

- TCP Peer B sends a SYN and acknowledges the SYN it received from TCP Peer A.

- TCP Peer A responds with an empty segment containing an ACK for TCP Peer B's SYN

- Now the connection is established and Peer A will start sending the data to B with SYNC number

- TCP Peer A sends some data. Note that the sequence number of the segment in line 5 is the same as in line 4 because the ACK does not occupy sequence number space

```
1) A --> B  SYN my sequence number is X
2) A <-- B  ACK your sequence number is X
3) A <-- B  SYN my sequence number is Y
4) A --> B  ACK your sequence number is Y
```

- **Sequence number** : The current sequence number
- **Acknowledgment number** is the sequence number of the next byte of data that the host is waiting for


**why 3 way handshake needed**
- Prevention of Duplicate Connection Initiations
- Security Against Spoofing
- Flow Control and Buffer Allocation


#### TCP  Format (Segment)

```
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |       |            |                                  |
| Offset| Rsrvd |control bits|             Window               |
|       |       |            |                                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                           [Options]                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               :
:                             Data                              :
:                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```


**Control bits**

The currently assigned control bits are CWR, ECE, URG, ACK, PSH, RST, SYN, and FIN.
- CWR: Congestion Window Reduced
- ECE: ECN-Echo
- URG: Urgent pointer field is significant
- ACK: Acknowledgment field is significant
- RST: Reset the connection
- SYN: Synchronize sequence numbers
- FIN: No more data from sender.

**Urgent Pointer** 

is used to provide a way for the sender to inform the receiver that certain data in the segment is urgent and should be prioritized

**Options**
- **Maximum Segment Size (MSS)**  is used to indicate the maximum size of a TCP segment that the sender can receive.
- **Window Scale:** allows the sender to advertise a larger window size than what can be represented in the standard 16-bit window field in the TCP header.
- **Timestamps:** used to measure the round-trip time and calculate the RTT (Round-Trip Time) between the sender and receiver.
- **Selective Acknowledgment (SACK):** allows a receiver to acknowledge out-of-order segments and gaps in the received data, providing more efficient error recovery and congestion control.
- **No-Operation (NOP):** is used as a placeholder for padding and does not carry any useful information.
- **Window Size:** is used to extend the window size field, providing a larger window for flow control. 
#### How TCP do reliable data transfer service

TCP sender uses timout to resend the segement if the recevier does not **ACK** the segement that send within the specific time period.

**Flow Control**

When the TCP connection receives bytes that are correct and in sequence, it places the data in the receive buffer. The associated application process will read data from this buffer.

The **receive window** is used to give the sender an idea of how much free buffer space is available at the receiver.

**TCP slow start**

 Servers only send a few packets (typically 10) in the initial round-trip while TCP is warming up (referred to as TCP slow start). After sending the first set of packets, it needs to wait for the client to acknowledge it received all those packets. server make sure at air it won't go above 10 that does not recevie ACK.
 
 The larger the initial window, the more we can transfer in the first roundtrip, the faster your site is on the initial page load. For a large roundtrip time .

The only way to estimate the available capacity between the client and the server is to measure it by exchanging data, and this is precisely what slow-start is designed to do. To start, the server initializes a new congestion window (cwnd) variable per TCP connection and sets its initial value to a conservative, system-specified value (initcwnd on Linux).

Congestion window size (cwnd)
- Sender-side limit on the amount of data the sender can have in flight before receiving an acknowledgment (ACK) from the client.

The maximum amount of data in flight for a new TCP connection is the minimum of the rwnd and cwnd values; hence a modern server can send up to ten network segments to the client, at which point it must stop and wait for an acknowledgment.

Then, for every received ACK, the slow-start algorithm indicates that the server can increment its cwnd window size by one segment — for every ACKed packet, two new packets can be sent. This phase of the TCP connection is commonly known as the "exponential growth" algorithm.

[Google](https://cloud.google.com/blog/products/networking/tcp-bbr-congestion-control-comes-to-gcp-your-internet-just-got-faster) introduced a new Algo for this which is called **BBR** (**B**ottleneck **B**andwidth and **R**ound-trip propagation time) which make YouTube network throughput by 4 percent on average globally

 **TCP Congestion Control**
 
 When sender send more at rate but recevier is not able to process the data at same speed the recevier buffer will be full and there is packet loss. To avoid that the recevier send the buffer size to sender i have only this much free space so send the data accordingly

**SACK**
Selective Acknowledgment, is an extension to the Transmission Control Protocol (TCP) designed to improve the performance of data transmission in networks with packet loss.

SACK introduces an option in the TCP header that allows the receiver to selectively acknowledge specific segments of data that have been received successfully. This enables the receiver to inform the sender about the non-contiguous blocks of data that have been successfully received.

SACK is an optional extension in TCP and is negotiated during the connection establishment phase. If both the sender and receiver support SACK, they can use it; otherwise, they fall back to regular TCP behavior.
#### UDP

User Datagram  Protocol provides  a procedure  for application  programs  to send
messages  to other programs  with a minimum  of protocol mechanism.

The protocol  is transaction oriented, and delivery and duplicate protection are not guaranteed.

UDP bind the socket with destination IP and Port

No handshaking is required on UDP mostly it used for live streaming data

**Format**

```
  
  0      7 8     15 16    23 24    31
 +--------+--------+--------+--------+
 |     Source      |   Destination   |
 |      Port       |      Port       |
 +--------+--------+--------+--------+
 |                 |                 |
 |     Length      |    Checksum     |
 +--------+--------+--------+--------+
 |
 |          data octets ...
 +---------------- ...---------------+
```

#### Tools

**Netcat**

used to create TCP or UDP connections.
- **nc -v hostname or IP address port**
- **nc -l port :** ->in listen mode to receive incoming connections.
- **nc -l port > output_file:** (listen on the port and store the data to file)
- **nc ip port < input_file:** send the file content to the host on port 

**Tcpdump**

Captures and analyzes network traffic. this will capture all network on the host we can use that and load to the wireshark to analyze it
- **tcpdump  host ip** -> capture only from this host by defaulr all 

**Nmap**

Scans and discovers devices on a network, providing information about open ports and services. `nmap [hostname or IP address]`

## Networking Layer

It is responsible for logical addressing, routing, and forwarding of data between different networks. It enables end-to-end communication by determining the best path for data packets to traverse through interconnected networks.

#### IP Address

An IP (Internet Protocol) address is a numerical label assigned to each device. IP addresses come in two versions, IPv4 (32-bit) and IPv6 (128-bit),

IP addresses are categorized into classes based on their leading bits
1. **Class A:**  Subnet Mask: 255.0.0.0 (or /8 in CIDR notation). Range: 1 to 126
2. **Class B:** Subnet Mask: 255.255.0.0 (or /16 in CIDR notation) 128 to 191
3. **Class C:** Subnet Mask: 255.255.255.0 (or /24 in CIDR notation) 192 to 223
4. **Class D (Multicast):** for multicast groups. 224 to 239
5. **Class E (Reserved):** for experimental purposes. 240 to 255
6. 127 -> loop back Ip (localhost)

#### Subnet Mask

A subnet mask is a 32-bit number that divides an IP address into network and host portions. It is used to specify which part of the IP address represents the network and which part identifies the specific device within that network.

It was introduced instead of class to make it easier to find network and host.

```

192.168.1.1  for this IP the subnetmask is 255.255.255.0

192.168.1.1   -> 11000000.10101000.00000001.00000001(bin)
255.255.255.0 -> 11111111.11111111.11111111.00000000
```

it means that the first 24 bits of the IP address are dedicated to the network, and the last 8 bits are for host addresses. so we can start from 00000000 to  11111111 (255)

So for the IP the network portion is 192.168.1 and host is 1  and we can add 1 to 255 computer (host) in this network

 **`Note`** **192.168.1** starting IP is act as router in the network. last IP **192.168.255** is used for broadcasting when we send data to that IP it will be sent all host belong to the network

#### CIDR Notation

CIDR notation represents IP addresses using a format that combines the base IP address and the number of bits used for the network portion. It follows the pattern: `IP_address/prefix_length`. For example, "192.168.1.0/24" means that the first 24 bits of the address are the network portion and balance 8 bit is host


#### Data Format (Packet)

``` plaintext
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Ver= 4 |IHL= 5 |Type of Service|        Total Length = 21      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      Identification = 111     |Flg=0|   Fragment Offset = 0   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Time = 123  |  Protocol = 1 |        header checksum        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         source address                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      destination address                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     data      |                                                
+-+-+-+-+-+-+-+-+
```
- Time: This field indicates the maximum time the datagram is allowed to remain in the internet system.The router that route the packet will decrease the Time if it became zero router will drop this helps to avoid packets get loops in router it will drop once it get 0.

#### IPV6
IPv6 increases the IP address size from 32 bits to 128 bits

**IPv6 Header Format**

```plaintext
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version| Traffic Class |           Flow Label                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Payload Length        |  Next Header  |   Hop Limit   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                         Source Address                        +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                      Destination Address                      +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

 - Version : 4-bit Internet Protocol version number = 6.
 - Hop Limit : like TTL in IPV4 (Decremented by 1 by each node that forwards the packet.)
 - Traffic Class : 
 
#### ICMP
#### Routing Protocol
Routing protocols determine how your data gets to its destination it mainly classify in two types 
1. Interior Gateway Protocols (IGP)
2. Exterior Gateway Protocols (EGP)

 **Interior Gateway Protocols (IGP)**
 These are used within an autonomous system (AS), which is a collection of routers under the same administrative control like our **ISP**. Examples include Routing Information Protocol (RIP) and Open Shortest Path First (OSPF).

 **Exterior Gateway Protocols (EGP)**
 These are used to exchange routing information between different autonomous systems. example: Border Gateway Protocol (BGP)

#### Router

Router are all over the internet helps to transmit the data from one network to another network.It has routing table which hold the IP of the known router that router and the destination.when ever router route the packet it add his IP and MAC address to the packet

Host communicate with router using MAC address and use **ARP**  protocol to find the MAC address of the router.

#### NAT 
Network Address Translation allows multiple devices within a local network to share a single public IP address for communication with external networks,

ISP uses NAT where some group of users comes under the same Router where it replace his IP when the request going out of internal.

Types
1. One to one (packet to external IP:port on the router always maps to internal ip:port )
2. Address restricted NAT 
3. Port restricted
4. Symmentric

#### Tools

**Traceroute**
determines the route to a destination by sending **Internet Control Message Protocol** (ICMP) echo packets to the destination. In these packets, TRACERT uses varying IP Time-To-Live (TTL) values. Because each router along the path is required to decrement the packet's TTL by at least 1 before forwarding the packet, the TTL is effectively a hop counter. When the TTL on a packet reaches zero (0), the router sends an ICMP "Time Exceeded" message back to the source computer.  
  
TRACERT sends the first echo packet with a TTL of 1 and increments the TTL by 1 on each subsequent transmission, until the destination responds or until the maximum TTL is reached.

- tracert domain -> will print how the packet is get transfered (the router will display * (asterisk) if no response is received.)


**My traceroute**
that combines the functionality of traceroute and ping. `mtr [destination]` will print all router with time it taken

**Netstat**
Displays network connections, routing tables, interface statistics, masquerade connections, and more.
- **netstat -a** -> Shows all active network connections.
- **netstat -l** -> list all open ports
- **netstat -r** -> print routing table
- **netstat -p** -> process ID (PID) and program name associated with each network socket.
- **netstat -s** -> summary of various network-related statistics, such as total packets transmitted and received.

## Data Link Layer
This layer is responsible for the framing, addressing, and error detection of data packets.The data link layer is concerned with local delivery of frames between devices on the same LAN

#### Ethernet
Defines the rules for how devices on a local area network (LAN) communicate with each other. It is the most common technology for wired LANs and is part of the IEEE 802.3 standard. 

Key features and aspects of Ethernet:

1. **Frame Format:**
    - Ethernet uses a frame format for data transmission. A frame consists of various fields, including destination and source MAC addresses, a type field indicating the type of payload, the payload itself, and a Frame Check Sequence (FCS) for error detection.
2. **MAC Address:**
    - Ethernet uses Media Access Control (MAC) addresses to uniquely identify devices on a network. These addresses are assigned to network interface cards (NICs) and are crucial for addressing and forwarding Ethernet frames.
3. **Carrier Sense Multiple Access with Collision Detection (CSMA/CD):**
    - Ethernet uses a network access method called CSMA/CD. Devices on an Ethernet network listen for the presence of a carrier (other devices transmitting) before attempting to transmit. If a collision is detected, devices use a backoff algorithm before retransmitting.
4. **Hub, Switch, and Bridge:**
    - Ethernet networks can be extended using devices such as hubs, switches, and bridges.
        - **Hub:** A basic networking device that broadcasts data to all devices in the network.
        - **Switch:** A more intelligent device that uses MAC addresses to selectively forward frames only to the device for which the frame is intended.
        - **Bridge:** Connects multiple network segments and uses MAC addresses for filtering traffic.
5. **Speed and Duplex:**
    - Ethernet supports different data rates, including 10 Mbps (Ethernet), 100 Mbps (Fast Ethernet), 1 Gbps (Gigabit Ethernet), 10 Gbps, 40 Gbps, and 100 Gbps. The duplex mode can be half-duplex (communication in one direction at a time) or full-duplex (simultaneous two-way communication).
6. **Twisted Pair Cabling:**
    - Ethernet commonly uses twisted pair cables for data transmission. Categories of twisted pair cables, such as Cat5e, Cat6, and Cat6a, support different data rates and distances.
7. **Fiber Optic Cabling:**
    - In addition to copper cables, Ethernet can operate over fiber optic cables, providing higher bandwidth and longer-distance capabilities.
8. **Ethernet Frame Types:**
    - Ethernet supports various frame types, including Ethernet II, IEEE 802.2, and IEEE 802.3. The most commonly used frame type today is Ethernet II.
9. **Ethernet over TCP/IP:**
    - Ethernet is often used as the underlying technology for TCP/IP networks. It is the foundation for local networking and Internet connectivity.

#### Media Access Control (MAC)
MAC is a unique identifier assigned to network interfaces for communication on the physical network. It is a hardware address burned into the network interface card (NIC).

We cannot change MAC address mostly but can be using software.MAC are used to communicate with LAN

MAC is 48 bit. 24 bit  for vendor number and 24 bit will unique for the NIC.
#### ARP (Address Resolution Protocol)

ARP resolves an IP address to a MAC address.Each host and router has an ARP table in its memory, which contains mappings of IP addresses to MAC addresses in there LAN.

If the host does not have the mapping it will send **ARP packet** to all host in his LAN and host will return there MAC.ARP operates when a host wants to send a datagram to another host on the same subnet.

#### Tools

**ARP**

Displays the current ARP table, which maps IP addresses to MAC addresses.
- **arp -a**
- **arp -s IP address MAC address** ->Adds a static entry to the ARP table

## Physical Layer

Responsible for the physical transmission of raw bits over a physical medium. It deals with the actual hardware connections, transmission media, and electrical or optical signaling.


## RFC
RFC stands for "Request for Comments." It is a series of documents and notes about the specifications, protocols, procedures, and conventions related to the operation and development of the internet. The RFC series is maintained by the Internet Engineering Task Force (IETF), a large open international community of network designers, operators, vendors, and researchers concerned with the evolution and smooth operation of the Internet architecture.

The RFC documents cover a wide range of topics, including protocols, procedures, programs, and meeting notes. Some of the most well-known and widely used internet standards, protocols, and technologies have been documented in RFCs.

Above Notes are mostly refered from RFC doc



## Resources

1. [RFC Spec for SMTP](https://datatracker.ietf.org/doc/html/rfc5321) 
2. [Understanding websocket indepth]([https://vishalrana9915.medium.com/understanding-websockets-in-depth-6eb07ab298b3](https://vishalrana9915.medium.com/understanding-websockets-in-depth-6eb07ab298b3))
3. [Computer Networking Introduction: Ethernet and IP (Heavily Illustrated)]([https://iximiuz.com/en/posts/computer-networking-101/](https://iximiuz.com/en/posts/computer-networking-101/))
4. [DNS]([https://jvns.ca/categories/dns/](https://jvns.ca/categories/dns/))
5. [Journey from request to response](https://medium.com/@felipemantillagomez/http-journey-from-request-to-response-7b122b8e74e9)
6. [Visulaize how we reach the website using traceroute]([https://how-did-i-get-here.net/](https://how-did-i-get-here.net/))
7. Computer networking A top down approach (best book)
#### Advanced

#### [Increase HTTP Performance by Fitting In the Initial TCP Slow Start Window](https://sirupsen.com/napkin/problem-15)
-  How TCP slow start affect page load
 - Try make the page size small as possible that can fit in 10 TCP segment roughly 12kb
 -  Increase the  initial congestion window for the server by default for linux it has 10 . it will send 10 TCP segment parallely and it will wait for ACK for each one if it recevied it send next segment.
#### [TCP Tuning for HTTP](https://datatracker.ietf.org/doc/html/draft-stenberg-httpbis-tcp)
- List of methods to tunning TCP for HTTP 


## Book
1. [High Performance Browser Networking](https://hpbn.co/)
	- Provides a hands-on overview of what every web developer needs to know about the various types of networks
2. 
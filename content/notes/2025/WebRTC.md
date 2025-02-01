---
title : WebRTC
date : 2025-01-25T07:17:19.1919+05:30
draft : true
tags : 
---



##### Signaling and Client Discovery

- Initially, **Client A** and **Client B** connect to a **signaling server**. This server acts as an intermediary for exchanging connection setup information (not media data) between the clients.
- Both clients confirm their presence and availability to each other through this signaling server. This ensures that both parties are ready to establish a connection.
##### Discovering Public IP and Port with STUN

- To establish a direct connection, each client needs to know its **public IP address** and port as seen from outside their private network.
- Both clients reach out to a publicly available **STUN (Session Traversal Utilities for NAT) server**. The STUN server replies with the public IP and port for each client.
    - For example:
        - Client A discovers its public IP: `203.0.113.10` and port: `40000`.
        - Client B discovers its public IP: `198.51.100.20` and port: `45000`.

##### Exchanging ICE Candidates

- Each client bundles its connectivity information (including public IP, port, and any additional network candidates) into what is called an **ICE (Interactive Connectivity Establishment) candidate**.
- These ICE candidates are exchanged between Client A and Client B via the signaling server. This allows each client to know the other’s public-facing network information.
    - For example:
        - Client A shares its candidate (`203.0.113.10:40000`) with Client B.
        - Client B shares its candidate (`198.51.100.20:45000`) with Client A.

##### NAT Traversal and Connection Establishment

Once ICE candidates are exchanged, the type of NAT each client is behind determines how the connection proceeds:
###### **a) Full Cone NAT**

- **Characteristics**: This type of NAT allows any external IP and port to send traffic to the client’s public IP and port, as long as the mapping has been created by the client’s outgoing traffic.
- **How It Works**:
    - After exchanging ICE candidates, Client A can directly send a message to Client B’s public IP and port (`198.51.100.20:45000`), and Client B can do the same for Client A.
    - No additional steps are required because Full Cone NAT is the least restrictive.
###### **b) Port-Restricted Cone NAT**

- **Characteristics**: Incoming traffic is allowed only if the source IP and port match the destination of a previously sent packet.
- **How It Works**:
    - After exchanging ICE candidates, both Client A and Client B send small "punch-through" packets to each other’s public IP and port.
        - For example:
            - Client A sends a packet to `198.51.100.20:45000`.
            - Client B sends a packet to `203.0.113.10:40000`.
    - These packets create NAT mappings on both sides, allowing subsequent traffic to flow freely.
###### **c) Symmetric NAT**

- **Characteristics**: A new NAT mapping is created for each unique destination IP and port. This makes direct communication nearly impossible because the mapping for the STUN server differs from the mapping for the peer.
- **How It Works**:
    - Even after exchanging ICE candidates, the NAT mappings for Client A and Client B do not align, making direct communication impossible.
    - In this case, a **TURN (Traversal Using Relays around NAT) server** is used as a relay.
        - Both Client A and Client B send their media traffic to the TURN server.
        - The TURN server forwards the traffic between the two clients, ensuring connectivity.
##### 5. **Final C onnection**

- Depending on the NAT type:
    - **Full Cone NAT or Port-Restricted Cone NAT**: A direct peer-to-peer connection is established, enabling efficient and low-latency communication.
    - **Symmetric NAT**: The connection relies on the TURN server to relay data, which adds some latency but ensures the communication works.


Peer to peer exchange video and audio 

STUN (session traversal util for NAT) (public server)
- A Server tell me my public ip address/port through NAT
- The client want to know his public ip and port (the ip of router)
- The clent will do STUN request to the STUN server where it send the response of the public ip and port


 **SDP (Session Description Protocol)**: SDP is a format used in WebRTC for describing multimedia communication sessions. It includes information about the codec, media type (audio/video), transport addresses, encryption keys, and more. It is exchanged during the signaling phase to ensure both clients agree on how to communicate.

TURN (traversal using relay around NAT)
- when user not able to connect directly we use TURN server which is private

#### ICE

Interactive Connectivity Establishment is used to find the best network path between two peers. It helps establish a direct peer-to-peer (P2P) connection despite challenges like **firewalls, NAT (Network Address Translation), and different network types**.

Steps
- **Candidate Gathering** : Each peer collects a list of **ICE candidates**—potential network paths that can be used for communication. These candidates come from three main sources:
	- Ethernet (wired)
    - WiFi
    - Cellular (4G/5G)
    - VPNs
- **Server-Reflexive Candidates (STUN)**: - Used when one or both peers are behind a NAT router.The peer contacts a STUN server, which tells the peer its public IP address.The peer then sends this public IP candidate to the other peer.
- Relay Candidates (TURN): Used when direct P2P connections **fail** due to strict NATs or firewalls The peers relay all traffic through a TURN (Traversal Using Relays around NAT) server.

Once a peer gathers ICE candidates, it sends them to the other peer using **Session Description Protocol (SDP)** inside a signaling protocol (like WebRTC’s offer-answer process via WebSockets, SIP, etc.).

After exchanging candidates, ICE runs **connectivity checks** using **STUN Binding Requests** to determine which candidate pair (local + remote) works best.

How ICE Checks Work
- The local peer sends a **STUN request** to the remote candidate.
- The remote peer **replies** with a STUN response.
- This process is repeated for **each candidate pair** until the best working connection is found.

## RTP

**RTP** (Real-Time Transport Protocol) is a protocol designed to deliver real-time data, such as audio and video, over IP networks. It is widely used in streaming media, VoIP, video conferencing, and WebRTC applications. RTP does not guarantee delivery, ordering, or error correction; instead, it focuses on delivering packets in real-time with the lowest possible latency.

### RTP Header Structure

The RTP packet consists of two main parts: the **header** and the **payload** (the actual media data). The header contains several fields that provide vital information to manage the transmission of media.

#### RTP Header Fields

- **Version (V)**: 2 bits, indicates the RTP version (currently version 2 is used).
- **Padding (P)**: 1 bit, if set to 1, it indicates that the packet contains additional padding at the end of the payload.
- **Extension (X)**: 1 bit, if set to 1, it indicates that an extension header is present.
- **CSRC Count (CC)**: 4 bits, specifies the number of CSRC identifiers present.
- **Marker (M)**: 1 bit, used to signal important events, like the end of a frame or a keyframe.
- **Payload Type (PT)**: 7 bits, identifies the type of media in the payload (e.g., audio or video codec type).
- **Sequence Number**: 16 bits, increments by 1 for each RTP packet sent, used to detect packet loss and reordering.
- **Timestamp**: 32 bits, reflects the sampling instant of the first byte of the media data. It allows the receiver to reconstruct the timing of the media stream.
- **SSRC (Synchronization Source)**: 32 bits, a unique identifier for the source of the RTP stream. This ensures that packets from different streams can be differentiated.
- **CSRC (Contributing Source)**: 32 bits (variable number), used when multiple sources are contributing to a single RTP stream, such as in a conference call.
### RTP Packet Flow

Here's how RTP works step by step in terms of packet flow:

1. **Media Capture & Encoding**:
    
    - The sender captures the media (audio/video) from the microphone, camera, or any other source.
    - This data is then **encoded** into the appropriate format (e.g., audio compressed with Opus or video compressed with H.264).
2. **Packetization**:
    
    - The encoded media data is broken down into smaller **packets** to fit into the RTP protocol.
    - Each packet is prepended with the **RTP header** (containing important information like sequence numbers, timestamps, and the media type).
    - For audio, this could mean a chunk of compressed audio data, and for video, it could mean an I-frame or a portion of a video frame.
3. **Sending the Packet**:
    
    - The RTP packet is sent over the network (typically via UDP) to the receiving endpoint (another client or server).
    - **UDP (User Datagram Protocol)** is often used with RTP because it offers low latency and avoids the overhead of error checking or retransmission, which are not required for real-time media. However, RTP can be run over TCP in some scenarios.
4. **Receiving the Packet**:
    
    - The receiver receives the RTP packet, extracts the media from the payload, and uses the **sequence number** to detect if any packets are lost or out of order.
    - The **timestamp** is used to ensure that the media is played back at the correct time, compensating for any network jitter.
5. **Reordering Packets**:
    
    - Since the packets might not always arrive in the correct order, the **sequence number** helps the receiver reorder them.
    - If packets are dropped due to network issues, the receiver might notice the gap in sequence numbers, which is useful for detecting packet loss.
6. **Playback of Media**:
    
    - After receiving enough packets, the receiver can use the **timestamp** to place each packet in the correct timing context and **reconstruct the media stream** (audio/video).
    - For audio or video, playback involves buffering a short time of media to compensate for any jitter (network delay variation).
## Javascript



In WebRTC, communication between two peers typically follows these key steps:

1. **Create the Peer Connection**
2. **Signaling (Offer/Answer Exchange)**
3. **Set up ICE (Interactive Connectivity Establishment)**
4. **Establish Media Streams**
5. **Exchange Data**
6. **Close the Connection**


### Step 1: **Create the Peer Connection**

The first thing you need is a `RTCPeerConnection` object, which handles the peer-to-peer connection.

```javascript
const peerConnection = new RTCPeerConnection(configuration);
```

- `configuration`: This can be an object that specifies ICE server details (STUN/TURN servers) to handle NAT traversal.

Example:

```javascript
const configuration = {
  iceServers: [
    { urls: "stun:stun.l.google.com:19302" } // Google's public STUN server
  ]
};
```

This configuration helps in establishing connectivity between peers even if they are behind firewalls or NATs.

### Step 2: **Signaling (Offer/Answer Exchange)**

WebRTC does not define signaling (the process by which peers communicate with each other before establishing a connection). It assumes that you will use your own method for signaling (like WebSockets, HTTP, or any custom server-based communication).

The signaling process typically involves two main actions:

- **Offer**: A peer creates an offer to initiate a connection.
- **Answer**: The other peer responds with an answer, accepting or rejecting the offer.

#### **Creating an Offer**

The initiating peer creates an SDP (Session Description Protocol) offer using the `createOffer` method. The offer includes information about codecs, media capabilities, and transport options.

```javascript
peerConnection.createOffer()
  .then(offer => {
    // Set local description with the offer
    return peerConnection.setLocalDescription(offer);
  })
  .then(() => {
    // Send the offer to the signaling server (e.g., WebSocket or HTTP request)
    signalingChannel.send({ type: "offer", sdp: peerConnection.localDescription });
  })
  .catch(error => console.error('Error creating offer:', error));
```

- **SDP Offer**: The offer is an object that includes information like the type of media (audio/video/data), codecs to be used, and other parameters needed for the peer-to-peer connection.

Example of an SDP offer:

```json
{
  type: "offer",
  sdp: "v=0\r\no=- 1686587949 1686587950 IN IP4 0.0.0.0\r\ns=WebRTC Example\r\nc=IN IP4 192.168.1.2\r\nt=0 0\r\na=rtpmap:96 H264/90000\r\n..."
}
```

#### **Receiving the Offer**

Once the offer is received, the receiving peer sets the offer as the remote description and responds with an answer.

```javascript
peerConnection.setRemoteDescription(new RTCSessionDescription(offer))
  .then(() => peerConnection.createAnswer())
  .then(answer => {
    return peerConnection.setLocalDescription(answer);
  })
  .then(() => {
    signalingChannel.send({ type: "answer", sdp: peerConnection.localDescription });
  })
  .catch(error => console.error('Error setting remote description or creating answer:', error));
```

#### **SDP Answer**: Similar to the offer, the answer includes details like media codecs and connection parameters.

Example of an SDP answer:

```json
{
  type: "answer",
  sdp: "v=0\r\no=- 1686587949 1686587950 IN IP4 0.0.0.0\r\ns=WebRTC Example\r\nc=IN IP4 192.168.1.2\r\nt=0 0\r\na=rtpmap:96 H264/90000\r\n..."
}
```

---

### Step 3: **Set up ICE (Interactive Connectivity Establishment)**

ICE is a framework used to find the best path between peers. It deals with network traversal, NAT (Network Address Translation) issues, and firewall restrictions.

During the connection process, WebRTC exchanges **ICE candidates**. These are potential network addresses that can be used for the connection. The `RTCPeerConnection` object automatically gathers candidates as it tries to find the best possible route.

```javascript
peerConnection.onicecandidate = function(event) {
  if (event.candidate) {
    signalingChannel.send({
      type: "candidate",
      candidate: event.candidate
    });
  }
};
```

The receiving peer will also listen for incoming candidates and add them to its connection:

```javascript
peerConnection.addIceCandidate(new RTCIceCandidate(candidate))
  .catch(error => console.error('Error adding ice candidate:', error));
```

Candidates might look like this:

```json
{
  "candidate": "candidate:0 1 UDP 2122252543 192.168.1.2 12345 typ host",
  "sdpMLineIndex": 0,
  "sdpMid": "video"
}
```

- **candidate**: A network address that the peer can use to connect.
- **sdpMLineIndex**: The media line index, telling which media type the candidate belongs to (audio/video).
- **sdpMid**: The media stream identifier.

---

### Step 4: **Establish Media Streams**

Once the offer/answer exchange is complete and the ICE candidates are gathered, you can start sending and receiving media streams.

To send media, you need to capture it from the local device, typically using `getUserMedia` for video/audio:

```javascript
navigator.mediaDevices.getUserMedia({ video: true, audio: true })
  .then(stream => {
    // Add the media stream to the peer connection
    peerConnection.addStream(stream);
  })
  .catch(error => console.error('Error accessing media devices:', error));
```

On the receiving peer, you can access the incoming media stream through the `onaddstream` event:

```javascript
peerConnection.onaddstream = function(event) {
  // Attach the remote stream to an HTML element (e.g., <video> tag)
  remoteVideoElement.srcObject = event.stream;
};
```

---

### Step 5: **Exchange Data**

WebRTC allows not only media but also data communication between peers using `RTCDataChannel`.

```javascript
const dataChannel = peerConnection.createDataChannel("chat");

dataChannel.onmessage = function(event) {
  console.log("Received message:", event.data);
};

dataChannel.send("Hello!");
```

---

### Step 6: **Close the Connection**

Once communication is finished, the connection should be closed. This can be done by calling `close()` on both the `RTCPeerConnection` and the `RTCDataChannel`:

```javascript
peerConnection.close();
dataChannel.close();
```

---

### Summary of Key Components:

- **RTCPeerConnection**: The main object for managing the connection.
- **getUserMedia**: Used to capture media (audio, video).
- **RTCDataChannel**: Enables data exchange.
- **SDP**: Describes the media session and connection details.
- **ICE**: Helps in network traversal to establish the connection.
- **Signaling**: The exchange of offers, answers, and candidates (via a custom signaling mechanism like WebSockets).

This is a high-level overview of how WebRTC works in JavaScript. Each step involves several interactions, particularly with the signaling and ICE candidate processes, but the general flow is about establishing peer-to-peer communication.




### wireshark
capture network traffic while a WebRTC call is active, then filter the packets to identify the RTP (Real-time Transport Protocol) packets which carry the WebRTC data, using the "rtp" protocol in the filter, and decode them as RTP to see detailed information like SSRC and payload type within Wireshark



Audio format 
- Opus (highly efficient and the default for WebRTC audio).

Streaming audio in Base64


The captured audio must be encoded:

- **Raw PCM (Pulse Code Modulation)**: Directly encodes raw audio samples into Base64.
- **Compressed Codec (Opus, MP3)**: Compresses audio before encoding into Base64.

If using raw PCM, the audio is processed in chunks using `AudioBuffer` or `MediaRecorder`. For compressed formats, `MediaRecorder` outputs already-encoded blobs (e.g., Opus in WebM), which can then be Base64-encoded.

**WebRTC**: For real-time, peer-to-peer audio and video streaming, WebRTC is much more efficient because it streams raw binary data without Base64 overhead.

Instead, WebRTC **encodes** the PCM data into a more efficient compressed audio codec, such as:

- **Opus**: The default codec for audio in WebRTC. It's highly efficient, supports variable bitrates, and provides excellent quality at low bandwidths.


PCM
**Pulse Code Modulation (PCM)** is a method used to digitally represent analog signals, particularly in audio processing. It is a foundational technique in digital audio systems, enable

-  PCM converts continuous analog signals into discrete digital signals by sampling the amplitude of the analog signal at regular intervals.
- The process involves three main steps: **sampling**, **quantization**, and **encoding**.
## JS

Mediastream
A **MediaStream** in JavaScript is an object that represents a stream of media content, such as audio and/or video.

- A MediaStream is composed of one or more `MediaStreamTrack` objects.
- Each `MediaStreamTrack` represents a single media input (e.g., audio or video track).
- Tracks can be added, removed, or replaced dynamically in a MediaStream.

A MediaStreamTrack object represents a single audio track within a MediaStream


. If the MediaStreamTrack is a video track, the chunks exposed by the stream will be VideoFrame objects.



MediaStream Consumers: The document identifies several MediaStream consumers, including media elements , WebRTC (RTCPeerConnection), media recording (MediaRecorder), image capture (ImageCapture), and web audio (MediaStreamAudioSourceNode)






## SIP

Alice "calls" Bob using his SIP identity, a type of Uniform Resource Identifier (URI) called a SIP URI
- INVITE request send first

```
 INVITE sip:bob@biloxi.com SIP/2.0
  Via: SIP/2.0/UDP pc33.atlanta.com;branch=z9hG4bK776asdhds
  Max-Forwards: 70
  To: Bob <sip:bob@biloxi.com>
  From: Alice <sip:alice@atlanta.com>;tag=1928301774
  Call-ID: a84b4c76e66710@pc33.atlanta.com
  CSeq: 314159 INVITE
  Contact: <sip:alice@pc33.atlanta.com>
  Content-Type: application/sdp
  Content-Length: 142
```

- When Bob's User Agent Server (UAS) receives the INVITE, it may send a 100 (Trying) response back to Alice
- 180 (Ringing) response to indicate that Bob's phone is ringing
- When Bob answers the call, his UAS sends a 200 (OK) response20. This response includes a message body (e.g., in SDP format) describing Bob’s media capabilities and preferences
- Alice’s UAC sends an ACK to confirm that the 200 (OK) response has been received21. The ACK is sent directly to Bob's UAS, bypassing any proxies that may have been in the path of the INVITE request
- With the session established, media can flow directly between Alice and Bob using protocols such as RTP




## Audio worklet

Audio worklets process audio in blocks of 128 samples at a time3. This is a very short frame (2 and 2/3 milliseconds at a 48kHz sample rate)


## Debugging in chrome

check chrome://webrtc-internal  where we can see  all webrtc call grouped by domain 
- first it have the config used for the webrtc and API traces on the left side 


Tools
- https://fippo.github.io/webrtc-dump-importer/



## Resources
- https://cyara.com/blog/webrtc-internals-parameters/
- https://audioply.vercel.app/
- https://webaudiodemos.appspot.com/
# Networking

### OSI ✅

- Abstraction or Concept of the Internet
- Layer 1 to Layer 7
    - Layer 1 is physical
    - Layer 2 is data frames etc
    - Layer 3 is IP
    - Layer 4 is TCP/UDP. ports
    - Layer 5 - connection/session layer (linkerD?)
    - Layer 6 - Presentation
    - Layer 7 - App layer, HTTP, HTTPS etc
- TCP/IP model
    - This model is another version that focusses on these layers Layer 3/4 & 7

### TCP ✅ (Layer 4, ports and stuff ineh)

- Transmission control protocol
- “Controls” the transmission unlike UDP which is a firehose
- Connection
- Requires handshake
- 20 bytes headers segment
- Stateful
- TCP >> Reliable comms, Remote shell (SSH), database connections, web comms, any bidirectional comms
- TCP Connection
    - Connection is layer (coz its a session)
    - Connection is an AGREEMENT between client and server
    - Must create connection to send data
    - Connection is identified by 4 properties
        - SourceIP-SourcePort
        - DestinationIP-DestinationPort
    - Can’t send data outside connection
    - Sometimes called socket or file descriptor
    - Requires 3 way TCP handshake (ACK. SYN-ACK, ACK)
    - Segments are sequenced and ordered
    - Segments are acknowledge
    - Lost segments are retransmitted
- Multiplexing and demultiplexing
    - *Sender multiplexes all its apps into TCP connections*
    - R*eceiver demultiplex TCP segments to each app based on connection pairs*
- Connection Establishment
    - *App1 on 10.0.0.1 want to send data to AppX on 10.0.0.2*
    - *App1 sends SYN to AppX to synchronous sequence numbers*
    - *AppX sends SYN/ACK to synchronous its sequence number*
    - *App1 ACKs AppX SYN.*
    - *Three way handshake*
- Sending data
    - App1 sends data to App 2
    - App1 encapsulates the data in a segment and send it
    - App 2 acknowledges the segment
    - Hint:*: Can App1 send new segment before ack of old segment arrives?*
- Lost data >> App1 sends segment 1,2 and 3 to App 2, Seg 3 is lost, App 2 acknowledge 3, App1 resend seq 3
- Closing connection
    - App 1 wants to close the connection
    - App1 sends FIN, App2 ACK
    - App2 sends FIN, App1 ACK
    - 4 way handshake > >FIN, ACK, FIN, ACK
- Summary
    - Layer 4
    - “Controls” the transmission unlike UDP which is a firehose
    - Introduces connection concept
    - Retransmission, acknowledgement, guaranteed delivery
    - Stateful, connection has a state
- Pros & Cons of TCP
    - Pros
        - Guaranteed delivery
        - No one can send data without prior knowledge
        - Flow control and congestion control
        - Ordered packets, no corruption or app level work
        - More secure than UDP and can’t easily be spoofed
    - Cons
        - large overhead compared to UDO
        - more bandwith
        - stateful: consumes memory on server and client
        - considered high latecny for certain workloads (slow start/congestion/acks)
        - Does too much at a low level (hence QUIC protocol intro)
            - Single connection to send multiple streams of data (HTTP reqs)
            - Stream 1 has nothing to do with stream 2
            - Both steam 1 and steam 2 packets must arrive)
        - TCP meltdown
            - Not a good candidate for VPN
    

### **UDP ✅ (Layer 4, ports and stuff too ineh)**

- User Diagram Protocol
- Simple protocol to send and recieve data
- Prior communication not required (yes it can be easy but also a double edged sword due to security)
- Stateless - no knowledge is stored on the host
- 8 byte header datagram
- Use cases >> video streaming, VPN, DNS, webRTC
- Mutliplexing & demultiplexing
    - >> sender multiplexes all its app into UDP
    - receiever demultideplx UDP datagrams to each app
- Source & Dest port
    - *App1 on 10.0.0.1 sends data to AppX on 10.0.0.2*
    - *Destination Port = 53*
    - *AppX responds back to App1*
    - *We need Source Port so we know how to send back data*
    - *Source Port = 5555*
- UDP Pros & Cons
    - Pros:
        - Simple protocol
        - header size is small so datagrams are small
        - uses less bandwith
        - stateless
        - consumes less memory (non state stored in server/client
    - Cons:
        - No Ack (acknowledgement)
        - No guaranteed delivery
        - Connection-less - anyone can send data without prior knowledge
        - No flow control
        - No congestion control
        - No ordered packets
        - Security - can be easily spoofed
- Summary
    - UDP is layer 4 protocol
    - uses ports to address processes
    - Stateless

### TLS (can possibly put in Layer 5 coz its stateful)

- Transport Layer Security (lives in layer 5, not written in stone but can be flexible)
    - mentioned layer 5 coz it has a session etc

- Vanilla HTTP (aka pure HTTP)
    - Client sends a GET / request (GET does’t have body) to the Server (a sgement is sent)
        - Moves into a TCP segment
        - Then IP packet
    - The server has port 80 open
    - Segment is received and req is understood
    - App will respond back with headers/response headers (index.html)

- HTTPS (HTTP over TLS basically)
    - Open connection and first do a handshake
    - The goal of the handshake is to share symmetric keys, share encryption key - same key should exist on both the client and the server
    - Use keys to encrypt our GET / request from the client
    - server will receive the data that is encrypted and decrypt the data with the key
    - We encrpyt with symmetric key algorithms or asymmetric keys
    - symmetric key >> the key to encrypt and decrypt are both the same. Easy to do
    - asymmetric keys >> the keys to decrypt and ecnrypt are different
- TLS 1.2
    - The private key is used to encrypt the symmetric key
    - There was a heartbleed openSSL bug that happened in 2012, bug fixed in 2014 - interesting stuff tbh - people avoided RSA as an algorithm coz of this reason. It lacks forward secrecy

- Diffie Hellman (one of the best algos to exchange keys)
    - There are 2 private keys and 1 symmetric key
    - 1 private key for the client and 1 for the server

- TLS 1.3 improvements (currently the best candidate for all)
    - Improved version using Diffie Hellman algorithm (the maths behind it is interesting)
    - (g^x % n)^y = g^xy & n (left part of equation is done by the server because the server has the Y)
    - (g^y % n)^x = g^xy %n (left part of equation is done by the client because the server has the X)

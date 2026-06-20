# Lab 2: Simulation of Transport Layer Protocol Mechanics (TCP & UDP) 

## 1. Lab Summary
This practical lab explored the **Transport Layer** (Layer 4 of the OSI model), investigating the operational differences between the internet’s two foundational transport protocols: **TCP (Transmission Control Protocol)** and **UDP (User Datagram Protocol)**. Executed inside **Cisco Packet Tracer's Simulation Mode**, the project involved building and analyzing a multi-client network interacting with a centralized server (`MultiServer`) running concurrent HTTP, FTP, DNS, and SMTP (Email) services. The objective was to observe multiplexing, transient port allocation policies, connection-oriented socket lifecycles (TCP handshakes), and connectionless streaming (UDP) at a packet-by-packet layer.

---

## 2. Evidence and Explanation

![Transport Layer Simulation](transport_layer.png)


*Figure 1: MultiServer Socket Active Session Registry Diagnostic*

* **Traffic Generation and Multiplexing:** Generated distinct application traffic streams from independent client machines (including HTTP, FTP, DNS, and SMTP requests) targeting the centralized server to analyze multi-stream multiplexing over a single interface.
* **Server-Side Socket Diagnostics:** Executed the `netstat` command directly within the `MultiServer` Desktop command prompt to audit active network sockets. As documented in **Figure 1**, the active network layer identifies the explicit transport protocol as **TCP**.
* **Port Mapping and Handshake Identification:** The diagnostic command output confirms that the server is communicating via its local port **21**, which is the standardized default channel reserved for control commands on an FTP daemon.
* **Persistent Connection State Analysis:** The trace shows that the connection remains locked in an **ESTABLISHED** state. While short-lived stateless protocols (like HTTP) immediately close their sockets after transmission, the FTP client socket remains active. This behavior occurs because the server daemon requires a continuous state loop, waiting for further user credentials or data channel requests before executing a tear-down sequence.
* **UDP Protocol Execution Comparison:** Contrasted TCP behavior against UDP by tracking DNS lookups. The simulation confirmed that UDP strips away connection-setup overhead and relies on independent, unacknowledged datagrams to achieve high-speed transmission, bypassing port tracking states entirely.
---

## 3. Reflection

### What I Learned
* Physically tracking a TCP handshake across a simulation timeline clarified why different tasks use different protocols. Seeing the structural overhead required to maintain an open state explains why reliable web traffic relies on **TCP** while quick lookup actions use UDP.
* Seeing a single server card successfully route four different data streams using nothing but destination port flags (like Port 21 for FTP and Port 53 for DNS) gave me a deep appreciation for the underlying engineering of modern operating systems.
* Linking **Packet Tracer’s visual** envelopes with terminal commands like `netstat` helped bridge theory and practice. It proved that the abstract session blocks we draw in textbooks perfectly map onto real, active sockets inside our physical computers.

### Areas for Improvement
* When multiple clients fired requests simultaneously, the event list became crowded with overlapping packet envelopes. I need to practice utilizing specific protocol color-coding rules and event filters within **Cisco Packet Tracer** to cleanly isolate single streams from complex background traffic.
* While I successfully traced sequence and acknowledgment numbers, I did not fully explore dynamic window scaling or congestion control variations. I plan to manually modify simulated packet drops to observe how TCP dynamically throttles data streams in real time.

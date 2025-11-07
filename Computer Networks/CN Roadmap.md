# Computer Networks (CN) Roadmap for Placement Preparation

## **1. Introduction to Computer Networks**
- Definition and Goals of Networking  
- Types of Networks (LAN, MAN, WAN, PAN)  
- Network Topologies (Bus, Star, Ring, Mesh, Hybrid)  
- OSI Model (7 Layers) vs TCP/IP Model (4/5 Layers)  
- Encapsulation and Decapsulation  

---

## **2. Physical Layer**
- Transmission Media (Guided: Twisted Pair, Coaxial, Fiber Optic; Unguided: Radio, Microwave, Infrared)  
- Data Encoding and Modulation Basics  
- Bandwidth, Throughput, Latency  
- Switching Techniques: Circuit Switching, Packet Switching, Message Switching  

---

## **3. Data Link Layer**
- **Framing** (Character Count, Byte Stuffing, Bit Stuffing)  
- **Error Detection & Correction**  
  - Parity, Checksum, CRC (Cyclic Redundancy Check), Hamming Code  
- **Flow Control**  
  - Stop-and-Wait, Sliding Window  
- **Protocols**  
  - HDLC, PPP  
- **MAC (Medium Access Control)**  
  - ALOHA, CSMA, CSMA/CD, CSMA/CA  
- Ethernet (Wired LAN basics, MAC addressing)  

---

## **4. Network Layer**
- IPv4 vs IPv6 addressing  
- Subnetting, Supernetting, CIDR  
- Routing Algorithms  
  - Distance Vector, Link State, Bellman-Ford, Dijkstra  
- Routing Protocols  
  - RIP, OSPF, BGP  
- IP Protocol Concepts  
  - ARP, RARP, ICMP, IGMP  
- NAT (Network Address Translation)  

---

## **5. Transport Layer**
- Responsibilities: End-to-End Delivery, Error Control, Flow Control, Multiplexing  
- **Protocols**  
  - TCP (features: reliable, connection-oriented, flow control, congestion control)  
  - UDP (features: unreliable, connectionless, lightweight)  
- TCP Concepts  
  - 3-Way Handshake (Connection Establishment)  
  - Connection Termination (4-Step Teardown)  
  - Sliding Window Protocol in TCP  
  - Congestion Control (Slow Start, AIMD, Fast Retransmit, Fast Recovery)  

---

## **6. Application Layer**
- DNS (Domain Name System)  
- HTTP vs HTTPS  
- SMTP, POP3, IMAP (Email Protocols)  
- FTP, TFTP  
- DHCP  
- SSH, Telnet  

---

## **7. Network Security Basics**
- Symmetric vs Asymmetric Encryption  
- SSL/TLS Handshake  
- Firewalls, IDS, IPS  
- VPN Basics  
- Common Attacks: DoS, DDoS, Man-in-the-Middle, Packet Sniffing, ARP Spoofing  

---

## **8. Wireless & Modern Networking**
- Wi-Fi Standards (802.11 a/b/g/n/ac/ax)  
- Bluetooth, NFC  
- Mobile Networks (2G → 5G)  
- Cloud Networking and Virtualization Basics  
- SDN (Software Defined Networking) Basics  

---

## **9. Important Tools & Practical Knowledge**
- **Linux Networking Commands**  
  - `ping`, `traceroute`, `netstat`, `ifconfig` / `ip addr`, `nslookup`, `dig`  
- Packet Analysis with Wireshark  
- Basic Socket Programming (C/C++ or Python)  

---

## **10. Interview Focus**
- Conceptual Questions:  
  - Difference between TCP and UDP?  
  - Explain OSI vs TCP/IP model.  
  - How does DNS resolution work?  
  - What happens when you type a URL in a browser?  
  - TCP 3-way handshake explanation.  
- Coding/Practical Questions:  
  - Implement simple TCP/UDP socket communication.  
  - Find subnet masks and number of hosts given an IP/CIDR.  
  - Simulate sliding window protocol.  

---

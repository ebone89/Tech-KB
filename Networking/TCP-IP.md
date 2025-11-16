# TCP/IP Protocol Suite

The Internet Protocol Suite, commonly known as TCP/IP, is the foundation of modern internet communications.

## 📚 TCP/IP Model Layers

### Application Layer
Combines OSI layers 5-7. Handles application-specific protocols.

**Common Protocols**:
- HTTP/HTTPS (Port 80/443) - Web traffic
- FTP (Port 20/21) - File transfer
- SMTP (Port 25) - Email sending
- POP3 (Port 110) - Email receiving
- IMAP (Port 143) - Email access
- DNS (Port 53) - Name resolution
- SSH (Port 22) - Secure shell
- Telnet (Port 23) - Remote access
- DHCP (Port 67/68) - IP address assignment

### Transport Layer
Provides end-to-end communication services.

#### TCP (Transmission Control Protocol)
**Characteristics**:
- Connection-oriented
- Reliable delivery
- Ordered delivery
- Error checking
- Flow control
- Congestion control

**Three-Way Handshake**:
```
Client          Server
  |---SYN-------->|
  |<--SYN-ACK-----|
  |---ACK-------->|
  (Connection Established)
```

**TCP Header Fields**:
- Source Port (16 bits)
- Destination Port (16 bits)
- Sequence Number (32 bits)
- Acknowledgment Number (32 bits)
- Flags (SYN, ACK, FIN, RST, PSH, URG)
- Window Size (16 bits)
- Checksum (16 bits)

#### UDP (User Datagram Protocol)
**Characteristics**:
- Connectionless
- Unreliable delivery
- No flow control
- Faster than TCP
- Lower overhead

**Use Cases**:
- DNS queries
- Video streaming
- VoIP
- Online gaming
- DHCP

### Internet Layer
Handles logical addressing and routing.

#### IP (Internet Protocol)

**IPv4**:
- 32-bit addresses
- Format: xxx.xxx.xxx.xxx (0-255)
- Total addresses: ~4.3 billion
- Classes: A, B, C, D (Multicast), E (Reserved)

**IPv4 Address Classes**:
```
Class A: 1.0.0.0    - 126.255.255.255  (/8)  - Large networks
Class B: 128.0.0.0  - 191.255.255.255  (/16) - Medium networks
Class C: 192.0.0.0  - 223.255.255.255  (/24) - Small networks
Class D: 224.0.0.0  - 239.255.255.255        - Multicast
Class E: 240.0.0.0  - 255.255.255.255        - Experimental
```

**Private IP Ranges** (RFC 1918):
```
10.0.0.0     - 10.255.255.255   (10/8)
172.16.0.0   - 172.31.255.255   (172.16/12)
192.168.0.0  - 192.168.255.255  (192.168/16)
```

**IPv6**:
- 128-bit addresses
- Format: xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx (hexadecimal)
- Total addresses: 340 undecillion
- Features: Built-in security, auto-configuration, no NAT needed

**Example**: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
**Compressed**: `2001:db8:85a3::8a2e:370:7334`

#### ICMP (Internet Control Message Protocol)
**Purpose**: Error reporting and diagnostic functions

**Common ICMP Messages**:
- Echo Request/Reply (ping)
- Destination Unreachable
- Time Exceeded (traceroute)
- Redirect
- Source Quench

**Ping Example**:
```bash
ping 8.8.8.8
# ICMP Echo Request → Google DNS
# ICMP Echo Reply ← Response
```

#### ARP (Address Resolution Protocol)
**Purpose**: Maps IP addresses to MAC addresses

**Process**:
1. Host needs MAC address for IP
2. Broadcasts ARP request: "Who has 192.168.1.1?"
3. Target responds: "I have it, my MAC is XX:XX:XX:XX:XX:XX"
4. Requesting host caches the mapping

**ARP Cache**:
```bash
arp -a
# Displays IP to MAC address mappings
```

### Network Access Layer
Combines OSI layers 1-2. Handles physical network interface.

**Technologies**:
- Ethernet
- Wi-Fi (802.11)
- PPP
- Token Ring (legacy)

## 🔄 Data Encapsulation

```
Application Data
    ↓
[TCP/UDP Header | Application Data]        ← Segment/Datagram
    ↓
[IP Header | TCP/UDP Header | Data]        ← Packet
    ↓
[Frame Header | IP Packet | Frame Trailer] ← Frame
    ↓
Bits on the wire                           ← Physical transmission
```

## 🌐 Common Port Numbers

### Well-Known Ports (0-1023)

| Port | Protocol | Service                    |
|------|----------|----------------------------|
| 20   | FTP      | Data Transfer              |
| 21   | FTP      | Control                    |
| 22   | SSH      | Secure Shell               |
| 23   | Telnet   | Remote Access              |
| 25   | SMTP     | Email Sending              |
| 53   | DNS      | Domain Name System         |
| 67   | DHCP     | Server                     |
| 68   | DHCP     | Client                     |
| 80   | HTTP     | Web Traffic                |
| 110  | POP3     | Email Retrieval            |
| 143  | IMAP     | Email Access               |
| 161  | SNMP     | Network Management         |
| 443  | HTTPS    | Secure Web Traffic         |
| 445  | SMB      | Windows File Sharing       |
| 3389 | RDP      | Remote Desktop             |

## 📊 TCP vs UDP Comparison

| Feature              | TCP                  | UDP                    |
|----------------------|----------------------|------------------------|
| Connection           | Connection-oriented  | Connectionless         |
| Reliability          | Reliable             | Unreliable             |
| Ordering             | Ordered              | Unordered              |
| Speed                | Slower               | Faster                 |
| Header Size          | 20 bytes minimum     | 8 bytes                |
| Error Checking       | Extensive            | Basic checksum         |
| Use Cases            | Web, Email, FTP      | Streaming, DNS, VoIP   |

## 🔍 Troubleshooting Commands

### Linux/Unix
```bash
# View network interfaces
ip addr show
ifconfig

# Test connectivity
ping 8.8.8.8

# Trace route
traceroute google.com

# DNS lookup
nslookup google.com
dig google.com

# View routing table
ip route show
route -n

# Network statistics
netstat -tuln
ss -tuln

# Packet capture
tcpdump -i eth0
```

### Windows
```powershell
# View network configuration
ipconfig /all

# Test connectivity
ping 8.8.8.8

# Trace route
tracert google.com

# DNS lookup
nslookup google.com

# View routing table
route print

# Network statistics
netstat -ano

# Display ARP cache
arp -a

# Flush DNS cache
ipconfig /flushdns
```

## Related Topics

- [[OSI Model]]
- [[IP Addressing]]
- [[Subnetting]]
- [[Network Troubleshooting Guide]]
- [[Cheat Sheets/Networking Commands|Networking Commands]]

---

#networking #tcpip #protocols #fundamentals

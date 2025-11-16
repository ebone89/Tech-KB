# OSI Model

The Open Systems Interconnection (OSI) model is a conceptual framework that standardizes network communication into 7 layers.

## 📚 The Seven Layers

### Layer 7: Application Layer
**Purpose**: User interface and application services

**Protocols**: 
- HTTP/HTTPS
- FTP/SFTP
- SMTP
- DNS
- SSH
- Telnet

**Devices**: Application gateways, proxies

**Data Unit**: Data

**Examples**:
- Web browsers
- Email clients
- File transfer applications

---

### Layer 6: Presentation Layer
**Purpose**: Data translation, encryption, compression

**Functions**:
- Data formatting
- Encryption/Decryption
- Compression
- Character encoding (ASCII, EBCDIC)

**Data Unit**: Data

**Examples**:
- SSL/TLS encryption
- JPEG, GIF, PNG format handling
- MPEG, QuickTime

---

### Layer 5: Session Layer
**Purpose**: Manages sessions and connections between applications

**Functions**:
- Session establishment
- Session maintenance
- Session termination
- Synchronization

**Protocols**:
- NetBIOS
- PPTP
- RPC

**Data Unit**: Data

---

### Layer 4: Transport Layer
**Purpose**: End-to-end communication and data delivery

**Protocols**:
- **TCP** (Transmission Control Protocol)
  - Connection-oriented
  - Reliable delivery
  - Flow control
  - Error checking
- **UDP** (User Datagram Protocol)
  - Connectionless
  - Fast but unreliable
  - No error checking

**Functions**:
- Segmentation
- Flow control
- Error detection and recovery
- Port addressing

**Data Unit**: Segment (TCP) / Datagram (UDP)

**Port Ranges**:
- Well-known: 0-1023
- Registered: 1024-49151
- Dynamic/Private: 49152-65535

---

### Layer 3: Network Layer
**Purpose**: Logical addressing and routing

**Protocols**:
- IP (Internet Protocol)
- ICMP (Internet Control Message Protocol)
- IGMP (Internet Group Management Protocol)
- IPsec

**Devices**: Routers, Layer 3 switches

**Functions**:
- Logical addressing (IP addresses)
- Routing
- Packet forwarding
- Fragmentation and reassembly

**Data Unit**: Packet

**Key Concepts**:
- IPv4: 32-bit addresses (e.g., 192.168.1.1)
- IPv6: 128-bit addresses (e.g., 2001:0db8::1)

---

### Layer 2: Data Link Layer
**Purpose**: Physical addressing and frame transmission

**Sub-layers**:
- **LLC** (Logical Link Control): Error checking, flow control
- **MAC** (Media Access Control): Physical addressing

**Protocols**:
- Ethernet
- PPP (Point-to-Point Protocol)
- Frame Relay
- ATM

**Devices**: Switches, bridges, NICs

**Functions**:
- Physical addressing (MAC addresses)
- Frame formatting
- Error detection (CRC)
- Access control

**Data Unit**: Frame

**MAC Address**: 48-bit address (e.g., 00:1A:2B:3C:4D:5E)

---

### Layer 1: Physical Layer
**Purpose**: Physical transmission of raw bits

**Components**:
- Cables (fiber, copper)
- Connectors (RJ45, fiber connectors)
- Hubs
- Repeaters
- Network adapters

**Specifications**:
- Voltage levels
- Cable specifications
- Pin layouts
- Physical topologies

**Data Unit**: Bits

**Media Types**:
- Twisted pair (Cat5e, Cat6, Cat6a)
- Fiber optic (single-mode, multi-mode)
- Wireless (radio frequencies)

---

## 🔄 Data Flow Example

**Sending Data (Top to Bottom)**:
```
Application Layer    → Create data (HTTP request)
Presentation Layer   → Encrypt data (SSL/TLS)
Session Layer        → Establish session
Transport Layer      → Add TCP/UDP header, segment data
Network Layer        → Add IP header, create packets
Data Link Layer      → Add MAC header/trailer, create frames
Physical Layer       → Convert to bits, transmit
```

**Receiving Data (Bottom to Top)**:
```
Physical Layer       → Receive bits
Data Link Layer      → Read frame, check CRC
Network Layer        → Read packet, check destination IP
Transport Layer      → Reassemble segments, check ports
Session Layer        → Manage session
Presentation Layer   → Decrypt data
Application Layer    → Deliver to application
```

## 🎯 Mnemonic Devices

**Top to Bottom**: "All People Seem To Need Data Processing"
- Application
- Presentation
- Session
- Transport
- Network
- Data Link
- Physical

**Bottom to Top**: "Please Do Not Throw Sausage Pizza Away"
- Physical
- Data Link
- Network
- Transport
- Session
- Presentation
- Application

## 📊 Quick Reference Table

| Layer | Name         | PDU      | Addressing      | Devices                | Key Protocols      |
|-------|--------------|----------|-----------------|------------------------|--------------------|
| 7     | Application  | Data     | Process-level   | Gateways, Proxies      | HTTP, FTP, DNS     |
| 6     | Presentation | Data     | -               | -                      | SSL/TLS, JPEG      |
| 5     | Session      | Data     | -               | -                      | NetBIOS, RPC       |
| 4     | Transport    | Segment  | Port numbers    | -                      | TCP, UDP           |
| 3     | Network      | Packet   | IP addresses    | Routers                | IP, ICMP, OSPF     |
| 2     | Data Link    | Frame    | MAC addresses   | Switches, Bridges      | Ethernet, PPP      |
| 1     | Physical     | Bits     | -               | Hubs, Cables, NICs     | Ethernet, 802.11   |

## Related Topics

- [[TCP-IP|TCP/IP Protocol Suite]]
- [[Network Devices]]
- [[Routing Protocols]]

---

#networking #fundamentals #osi

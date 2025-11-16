# Cisco Commands Cheat Sheet

Essential Cisco IOS commands for router and switch configuration.

## 🔧 Basic Navigation

```cisco
# Enter privileged mode
enable
en

# Exit privileged mode
disable

# Enter global configuration mode
configure terminal
conf t

# Exit configuration mode
exit
end

# Return to user mode
end
Ctrl+Z

# Show current mode
# User mode:        Router>
# Privileged mode:  Router#
# Config mode:      Router(config)#
# Interface mode:   Router(config-if)#
```

## 📝 Basic Configuration

### Hostname and Banner
```cisco
# Set hostname
Router(config)# hostname R1

# Set banner
R1(config)# banner motd # Unauthorized access prohibited #
R1(config)# banner login # Welcome to R1 #
```

### Passwords
```cisco
# Enable secret password (encrypted)
R1(config)# enable secret MySecretPass123

# Console password
R1(config)# line console 0
R1(config-line)# password ConsolePass123
R1(config-line)# login
R1(config-line)# logging synchronous
R1(config-line)# exec-timeout 5 0

# VTY password (Telnet/SSH)
R1(config)# line vty 0 4
R1(config-line)# password VtyPass123
R1(config-line)# login
R1(config-line)# transport input ssh      # SSH only
R1(config-line)# transport input telnet   # Telnet only
R1(config-line)# transport input all      # Both

# Encrypt passwords
R1(config)# service password-encryption
```

## 🌐 Interface Configuration

### Basic Interface Setup
```cisco
# Enter interface configuration
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# description Link to LAN
R1(config-if)# no shutdown
R1(config-if)# exit

# Configure serial interface
R1(config)# interface serial 0/0/0
R1(config-if)# ip address 10.1.1.1 255.255.255.252
R1(config-if)# clock rate 64000              # DCE side only
R1(config-if)# bandwidth 64
R1(config-if)# no shutdown

# Configure subinterface (for VLANs)
R1(config)# interface gigabitEthernet 0/0.10
R1(config-subif)# encapsulation dot1q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0
```

### Interface Shortcuts
```cisco
# Configure range of interfaces
Switch(config)# interface range fa0/1-24
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10

# Show interfaces
R1# show ip interface brief
R1# show interfaces
R1# show interfaces gigabitEthernet 0/0
R1# show ip interface gigabitEthernet 0/0
```

## 🔀 Routing

### Static Routing
```cisco
# Static route
R1(config)# ip route 192.168.2.0 255.255.255.0 10.1.1.2
# Format: ip route [destination] [mask] [next-hop or exit-interface]

# Default route
R1(config)# ip route 0.0.0.0 0.0.0.0 10.1.1.2

# Show routing table
R1# show ip route
R1# show ip route static
```

### RIP (Routing Information Protocol)
```cisco
# Configure RIP v2
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# network 192.168.1.0
R1(config-router)# network 10.0.0.0
R1(config-router)# no auto-summary
R1(config-router)# passive-interface gigabitEthernet 0/0

# Verify RIP
R1# show ip protocols
R1# show ip rip database
```

### OSPF (Open Shortest Path First)
```cisco
# Configure OSPF
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0
R1(config-router)# network 10.1.1.0 0.0.0.3 area 0
R1(config-router)# passive-interface gigabitEthernet 0/0

# Verify OSPF
R1# show ip ospf
R1# show ip ospf interface
R1# show ip ospf neighbor
R1# show ip ospf database
```

### EIGRP (Enhanced Interior Gateway Routing Protocol)
```cisco
# Configure EIGRP
R1(config)# router eigrp 100
R1(config-router)# eigrp router-id 1.1.1.1
R1(config-router)# network 192.168.1.0 0.0.0.255
R1(config-router)# network 10.1.1.0 0.0.0.3
R1(config-router)# passive-interface gigabitEthernet 0/0
R1(config-router)# no auto-summary

# Verify EIGRP
R1# show ip eigrp neighbors
R1# show ip eigrp topology
R1# show ip protocols
```

## 🔌 VLAN Configuration

### Basic VLAN Setup
```cisco
# Create VLAN
Switch(config)# vlan 10
Switch(config-vlan)# name SALES
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name ENGINEERING

# Assign interface to VLAN
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

# Configure trunk port
Switch(config)# interface gi0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 99
Switch(config-if)# switchport trunk allowed vlan 10,20,30

# Show VLAN information
Switch# show vlan brief
Switch# show vlan id 10
Switch# show interfaces trunk
```

### VTP (VLAN Trunking Protocol)
```cisco
# Configure VTP
Switch(config)# vtp mode server
Switch(config)# vtp domain COMPANY
Switch(config)# vtp password MyVTPPass

# VTP modes: server, client, transparent
# Verify VTP
Switch# show vtp status
Switch# show vtp password
```

## 🌉 Spanning Tree Protocol (STP)

```cisco
# Set root bridge (priority must be multiple of 4096)
Switch(config)# spanning-tree vlan 1 priority 4096
Switch(config)# spanning-tree vlan 1 root primary    # Alternative

# Configure PortFast (access ports only)
Switch(config)# interface fa0/1
Switch(config-if)# spanning-tree portfast

# Enable BPDU Guard
Switch(config-if)# spanning-tree bpduguard enable

# Global PortFast and BPDU Guard
Switch(config)# spanning-tree portfast default
Switch(config)# spanning-tree portfast bpduguard default

# Show STP status
Switch# show spanning-tree
Switch# show spanning-tree vlan 1
Switch# show spanning-tree summary
```

## 🔒 Security Features

### Port Security
```cisco
# Configure port security
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# switchport port-security violation shutdown
# Violation modes: shutdown, restrict, protect

# Show port security
Switch# show port-security
Switch# show port-security interface fa0/1
Switch# show port-security address
```

### SSH Configuration
```cisco
# Configure SSH
R1(config)# ip domain-name company.com
R1(config)# crypto key generate rsa
# Choose 1024 or 2048 bits
R1(config)# ip ssh version 2
R1(config)# username admin privilege 15 secret AdminPass123

R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# login local

# Verify SSH
R1# show ip ssh
R1# show ssh
```

### Access Control Lists (ACLs)
```cisco
# Standard ACL (filter by source IP only)
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)# access-list 1 deny any

# Apply to interface
R1(config)# interface gi0/0
R1(config-if)# ip access-group 1 out

# Extended ACL (filter by source, destination, protocol, port)
R1(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
R1(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
R1(config)# access-list 100 deny ip any any

# Named ACL
R1(config)# ip access-list extended BLOCK_TELNET
R1(config-ext-nacl)# deny tcp any any eq 23
R1(config-ext-nacl)# permit ip any any

# Apply to interface
R1(config)# interface gi0/1
R1(config-if)# ip access-group BLOCK_TELNET in

# Show ACLs
R1# show access-lists
R1# show ip access-lists
R1# show ip interface gi0/0
```

## 🔍 Verification Commands

### General Information
```cisco
# Show version and hardware info
R1# show version
R1# show running-config
R1# show startup-config
R1# show flash
R1# show clock

# Show protocols
R1# show protocols
R1# show ip protocols
```

### Interface Status
```cisco
R1# show ip interface brief
R1# show interfaces
R1# show interfaces status          # Switch only
R1# show interfaces description
R1# show controllers serial 0/0/0   # Check DCE/DTE cable
```

### Routing and Switching
```cisco
R1# show ip route
R1# show ip route summary
R1# show cdp neighbors
R1# show cdp neighbors detail
R1# show mac address-table          # Switch only
R1# show arp
```

### Troubleshooting
```cisco
R1# ping 8.8.8.8
R1# traceroute 8.8.8.8
R1# show logging
R1# show processes
R1# show memory
```

## 💾 Configuration Management

### Save Configuration
```cisco
# Save running config to startup config
R1# copy running-config startup-config
R1# write memory
R1# wr

# Backup to TFTP
R1# copy running-config tftp
R1# copy startup-config tftp

# Restore from TFTP
R1# copy tftp running-config
R1# copy tftp startup-config
```

### Reset Configuration
```cisco
# Erase startup config
R1# write erase
R1# erase startup-config

# Delete VLAN database (Switch)
Switch# delete flash:vlan.dat

# Reload device
R1# reload
R1# reload in 10              # Schedule reload in 10 minutes
R1# reload cancel             # Cancel scheduled reload
```

## 🛠️ Troubleshooting Commands

```cisco
# Debug commands (use with caution in production)
R1# debug ip routing
R1# debug ip ospf events
R1# debug ip rip
R1# undebug all               # Turn off all debugging

# Test interface
R1# test cable-diagnostics tdr interface gi0/1
R1# show cable-diagnostics tdr interface gi0/1

# Clear counters
R1# clear counters
R1# clear arp-cache
R1# clear mac address-table dynamic
```

## 🔐 Common Configurations

### Basic Router Setup
```cisco
enable
configure terminal
hostname R1
enable secret MySecret123
service password-encryption
no ip domain-lookup
banner motd # Authorized Access Only #

interface gi0/0
 ip address 192.168.1.1 255.255.255.0
 description LAN Interface
 no shutdown

interface gi0/1
 ip address 10.1.1.1 255.255.255.0
 description WAN Interface
 no shutdown

ip route 0.0.0.0 0.0.0.0 10.1.1.2

line console 0
 password ConsolePass123
 login
 logging synchronous

line vty 0 4
 password VtyPass123
 login
 transport input ssh

ip domain-name company.com
crypto key generate rsa
ip ssh version 2
username admin privilege 15 secret AdminPass123

end
write memory
```

### Basic Switch Setup
```cisco
enable
configure terminal
hostname SW1
enable secret MySecret123
service password-encryption
no ip domain-lookup

vlan 10
 name SALES
vlan 20
 name ENGINEERING

interface range fa0/1-10
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 spanning-tree portfast

interface gi0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20

interface vlan 1
 ip address 192.168.1.2 255.255.255.0
 no shutdown

ip default-gateway 192.168.1.1

line console 0
 password ConsolePass123
 login
 logging synchronous

line vty 0 4
 password VtyPass123
 login

end
write memory
```

## 💡 Tips and Shortcuts

```cisco
# Shortcuts
do                               # Execute privileged commands from config mode
# Example: R1(config)# do show ip interface brief

Tab                              # Auto-complete command
?                                # Context-sensitive help
Ctrl+Shift+6                     # Break sequence (stop ping/traceroute)
Ctrl+C                           # Exit configuration mode
Ctrl+Z                           # Return to privileged mode
```

## Related Topics

- [[Networking/OSI Model|OSI Model]]
- [[Networking/TCP-IP|TCP/IP]]
- [[Networking/Routing Protocols|Routing Protocols]]
- [[Networking/VLANs|VLANs]]

---

#cisco #networking #cheatsheet #ios

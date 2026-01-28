# 🏦 Enterprise Bank Network Design & Implementation

### 🖥️ Topology Overview

This project features a high-availability, multi-floor network architecture designed for a modern bank branch. The network is distributed across four floors with a hierarchical design to ensure security, redundancy, and scalability.

* Zemin KAT (Ground Floor):Customer Service, Private Banking, and Manager's Office.
* 1. KAT (1st Floor):Risk & Compliance, Operations, and Marketing.
* 2. KAT (2nd Floor):Info Security (IT), HR, and Finance.
* 3. KAT (3rd Floor):Executive Offices, Meeting Rooms, and the Data Center (Server Room).
* Infrastructure:** 4 Core Routers, 4 Layer-3 Switches (L3SW), 12 Access Switches, and Wireless LWAPs.



### ✅ Project Objectives

1. **Redundancy:Implementing dual-links between L3 switches and core routers for zero downtime.
2. **Network Segmentation:Using VLANs to isolate sensitive financial departments.
3. **Dynamic Routing:Configuring OSPF Area 0 for fast convergence between core devices.
4. **Security:Implementing Port Security on the Finance department and SSH for management.
5. **Centralized Management: Deploying a dedicated Server Farm (DHCP, DNS, Email) with IP Helper-Address relay.

---

### 📊 IP Addressing Schema (Subnetting)

#### 🏢 Departmental LANs (/24)

| Floor | VLAN | Department | Network Address | Gateway |
| --- | --- | --- | --- | --- |
|Ground| 10 | Customer Service | 192.168.10.0 | 192.168.10.1 |
|Ground| 20 | Private Banking | 192.168.20.0 | 192.168.20.1 |
|1st| 40 | Risk & Compliance | 192.168.40.0 | 192.168.40.1 |
|2nd| 90 | Finance | 192.168.90.0 | 192.168.90.1 |
|3rd| 120 | Server Room | 192.168.120.0 | 192.168.120.1 |

Core Infrastructure Links (/30)

| Connection | Network | IP Assignments |
| --- | --- | --- |
|Zemin-L3SW ⭢ Zemin-R| 10.10.10.0/30 | .1 ⭢ .2 |
|Zemin-R ⭢ 1.KAT-R| 10.10.10.16/30 | .17 ⭢ .18 |
|2.KAT-R ⭢ 3.KAT-R| 10.10.10.36/30 | .37 ⭢ .38 |


Step-by-Step Configuration

1️⃣ Basic Settings & SSH Management

Secure remote access is enabled using SSH instead of Telnet to encrypt management traffic.


# Basic Security
Switch(config)# hostname zemin-L3SW
zemin-L3SW(config)# banner motd #Only authorized access!!#
zemin-L3SW(config)# service password-encryption

# SSH Configuration
zemin-L3SW(config)# ip domain-name cisco.net
zemin-L3SW(config)# username cisco password cisco
zemin-L3SW(config)# crypto key generate rsa
# Key modulus size: 1024
zemin-L3SW(config)# line vty 0 15
zemin-L3SW(config-line)# transport input ssh
zemin-L3SW(config-line)# login local


2️⃣ Finance Department Port Security

To prevent unauthorized devices from connecting to the Finance network (VLAN 90).


Finance-SW(config)# interface range fa0/3 - 24
Finance-SW(config-if-range)# switchport mode access
Finance-SW(config-if-range)# switchport port-security
Finance-SW(config-if-range)# switchport port-security maximum 1
Finance-SW(config-if-range)# switchport port-security mac-address sticky
Finance-SW(config-if-range)# switchport port-security violation shutdown


3️⃣ VLANs & Trunking

VLANs are created on L3 switches, and Trunk ports are configured to carry traffic between devices.

Creating VLANs:

vlan 10
 name MHS
vlan 90
 name FIN
vlan 120
 name SERVERS


Trunk Configuration:


Switch(config)# interface range fa0/1 - 7
Switch(config-if-range)# switchport mode trunk



4️⃣ OSPF Dynamic Routing

OSPF is configured across all Layer 3 Switches and Core Routers in Area 0 for full network reachability.


# L3 Switch OSPF
L3SW(config)# ip routing
L3SW(config)# router ospf 10
L3SW(config-router)# router-id 1.1.1.1
L3SW(config-router)# network 192.168.0.0 0.0.255.255 area 0
L3SW(config-router)# network 10.10.10.0 0.0.0.3 area 0

# Convert L2 ports to L3 for Router links
L3SW(config)# interface fa0/1
L3SW(config-if)# no switchport
L3SW(config-if)# ip address 10.10.10.1 255.255.255.252



5️⃣ Inter-VLAN Routing & DHCP Helper

Layer 3 switches act as the default gateways for their respective floors. Since the DHCP server is located in VLAN 120, a helper address is required.


interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 192.168.120.5
 no shutdown

interface vlan 90
 ip address 192.168.90.1 255.255.255.0
 ip helper-address 192.168.120.5
 no shutdown



📂 Files Included

* Bank_Project.pkt – Full Cisco Packet Tracer source file.
* Topology.png – High-resolution network diagram.
* README.md – Documentation.



✅ Final Connectivity Verification

1. DHCP Success

All end-user PCs successfully received automatic IP addresses from the central DHCP server across different VLANs.

2. Ping Test Results

✅ Intra-floor: PC1 (VLAN 10) ↔ Manager PC (VLAN 30) — Success
✅ Inter-floor: Zemin PC (VLAN 10) ↔ 1st Floor PC (VLAN 60) — Success
✅ Server Access: Finance PC (VLAN 90) ↔ Email Server (VLAN 120) — Success


> Test Log: Reply from 192.168.60.10: bytes=32 time=1ms TTL=125

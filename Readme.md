## Enterprise Networking System
![Network](https://github.com/chandrakamalsingh007/LU2025_Networking-Project__Bank-Network/blob/main/Enterprise%20Networking%20system.png)
## PROJECT OVERVIEW
This project represents a fully designed and configured enterprise network consisting of four floors.
Each floor contains different departments assigned to different VLANs.
The network supports:

*** Secure wired & wireless connectivity ***

*** Inter-VLAN routing ***

*** Redundant routing (OSPF) ***

*** Centralized server services ***

*** DHCP-based IP assignment ***

*** Trunking across switches ***

*** Proper segmentation for performance & security ***

*** The central routing core uses four routers, connected in a redundant ring topology with OSPF. ***

 ## 📁 Table of Contents

- Network Topology

- Devices Used

- VLAN Structure

- IP Addressing & Subnetting

- Switch Configuration Summary

- Inter-VLAN Routing

- OSPF Configuration

- DHCP Server Setup

- Server Room Details

- Wireless Configuration

- Security Configuration

- Verification & Testing

- Future Enhancements

## 🗺️ Network Topology

The network is divided into four floors:

*** Floor 1 (Blue Section) ***

Management – VLAN 10

Research – VLAN 20

Human Resources – VLAN 30

*** Floor 2 (Pink Section) *** 

Marketing – VLAN 40

Accounting – VLAN 50

Finance – VLAN 60

*** Floor 3 (Cyan Section) *** 

Logistics & Store – VLAN 70

Customer Care – VLAN 80

Guest Area – VLAN 90

*** Floor 4 (Green Section) *** 

Administration – VLAN 100

ICT – VLAN 110

Server Room – VLAN 120

Each VLAN includes PCs, printers, and wireless access points connected to Layer-2 and Layer-3 switches.

At the center, multiple routers form a redundant OSPF network.

## 🖥️ Devices Used
- Switches (13 Total)

- Cisco 2960-24TT

- Cisco 2960-24PS

- Cisco 2960-48TT

- Routers (4 Total)

- Cisco 2911 Series

- Servers

- DHCP Server

- Email Server

- HTTPS Server

- Wireless Devices

- Access Points (One per VLAN/department)

- End Devices

PCs

Printers

## 🔀 VLAN Structure
Floor	Department	        VLAN    Subnet
1	    Management	        10	    192.168.10.0/26
1	    Research	        20	    192.168.10.64/26
1	    Human Resource	    30	    192.168.10.128/26
2	    Marketing	        40	    192.168.10.192/26
2	    Accounting	        50	    192.168.11.0/26
2	    Finance	            60	    192.168.11.64/26
3	    Logistics & Store	70	    192.168.11.128/26
3	    Customer Care	    80	    192.168.11.192/26
3	    Guest Area	        90	    192.168.12.0/26
4	    Administration	    100	    192.168.12.64/26
4	    ICT	                110	    192.168.12.128/26
4	    Server Room	        120	    192.168.12.192/26


## 📡 IP Addressing & Subnetting

- All VLANs use /26 subnets (64 hosts each).

- Each VLAN interface (SVI) uses the first usable IP.

- Router-to-router links use 10.10.10.x/30 networks.

- Example SVI assignments:

VLAN 10 → 192.168.10.1
VLAN 20 → 192.168.10.65
VLAN 30 → 192.168.10.129
VLAN 40 → 192.168.10.193 and so on

## 🔧 Switch Configuration Summary
1. Basic Configuration
``` bash
hostname Floor-1-MGT-SW
no ip domain-lookup
enable secret cisco
service password-encryption
ip ssh version 2
```
2. VLAN Creation
``` bash
vlan 10
name MANAGEMENT
vlan 20
name RESEARCH
vlan 30
name HR
```

3. Access Ports
``` bash
interface fa0/1
switchport mode access
switchport access vlan 10
```

4. Trunk Ports
``` bash
interface gi0/1
switchport mode trunk
switchport trunk allowed vlan all
```

🔁 Inter-VLAN Routing (L3 Switch)
``` bash
interface vlan 10
 ip address 192.168.10.1 255.255.255.192

interface vlan 20
 ip address 192.168.10.65 255.255.255.192

interface vlan 30
 ip address 192.168.10.129 255.255.255.192

ip routing
```

🌐 OSPF Configuration

Enabled on routers and L3 switches.
``` bash
router ospf 10
network 10.10.10.0 0.0.0.255 area 0
network 192.168.0.0 0.0.255.255 area 0

passive-interface default
no passive-interface gi0/0
no passive-interface gi0/1
```

This enables:

Full network route sharing

Automatic learning of VLAN networks

High availability and redundancy

## 🏷️ DHCP Server Setup

DHCP server located in VLAN 120.

Example Pool
``` bash
ip dhcp pool MANAGEMENT
 network 192.168.10.0 255.255.255.192
 default-router 192.168.10.1
 dns-server 8.8.8.8
```
DHCP Relay / Helper Address
``` bash
interface vlan 10
 ip helper-address 192.168.12.196
 ```

## 🖥️ Server Room Details
Server	Static IP
DHCP Server	    192.168.12.194
Email Server	192.168.12.195
HTTPS Server	192.168.12.196
📡 Wireless Configuration

Each department has a dedicated SSID.

Example AP configuration:

ssid MANAGEMENT
authentication open
authentication key-management wpa
wpa-psk ascii 0 management123


Each AP is placed in its respective VLAN.

## 🔐 Security Configuration
``` bash
Switchport Security
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict

SSH Access
username admin secret class
ip domain-name enterprise.local
crypto key generate rsa

line vty 0 4
 transport input ssh
 login local
```

🧪 Verification & Testing
Connectivity
``` bash
ping 192.168.10.1
ping 192.168.11.65
ping 192.168.12.195

Check Trunks
show interfaces trunk

Check DHCP Bindings
show ip dhcp binding

Check OSPF Routes
show ip route ospf
```

## 🚀 Future Enhancements

- Add enterprise firewall (Cisco ASA/Firepower)

- Implement OSPF authentication

- Add Quality of Service (QoS) for VoIP/Video

- Deploy Network Monitoring (Zabbix / SolarWinds)

- Add VPN for remote access

- Wireless controller for centralized AP management

- Implement dual-stack IPv6

- Add redundant core switches
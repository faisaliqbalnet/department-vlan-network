# 🏢 Department VLAN Network — Cisco Packet Tracer

A department-based VLAN network built in Cisco Packet Tracer for a company with 3 departments (HR, IT, Sales) using VLANs, trunking, and inter-VLAN routing.

## 📌 Project Overview

| Item | Detail |
|------|--------|
| **Project** | Department-Based VLAN Network |
| **Tool** | Cisco Packet Tracer |
| **Difficulty** | Intermediate |
| **Network** | IPv4 with 4 VLANs |

## 🎯 Requirements

- 1 Cisco Router (ISR 4331) — Router-on-a-Stick
- 2 Cisco Switches (2960-24TT)
- 12 PCs (4 HR + 4 IT + 4 Sales)
- 1 Server (static IP)
- Each department on its own VLAN and subnet
- DHCP for all PCs (per VLAN)
- Trunk links between switches
- Inter-VLAN routing
- Server reachable from all departments

## 🗺️ VLAN & IP Plan

| VLAN | Department | Subnet | Gateway |
|------|-----------|--------|---------|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Sales | 192.168.30.0/24 | 192.168.30.1 |
| 40 | Server | 192.168.40.0/24 | 192.168.40.1 |

**Server Static IP:** 192.168.40.10

## ⚙️ Router Configuration (Router-on-a-Stick)

```cisco
enable
configure terminal
hostname Office-Router

interface GigabitEthernet0/0/0
 no shutdown
 exit

interface GigabitEthernet0/0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 exit

interface GigabitEthernet0/0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 exit

interface GigabitEthernet0/0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
 exit

interface GigabitEthernet0/0/0.40
 encapsulation dot1Q 40
 ip address 192.168.40.1 255.255.255.0
 exit

ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10
ip dhcp excluded-address 192.168.40.1 192.168.40.10

ip dhcp pool HR-POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.40.10
 exit

ip dhcp pool IT-POOL
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 192.168.40.10
 exit

ip dhcp pool SALES-POOL
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 192.168.40.10
 exit

ip routing
end
write memory

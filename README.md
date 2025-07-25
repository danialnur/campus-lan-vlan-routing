# campus-lan-vlan-routing
A Cisco Packet Tracer simulation of a campus network using VLANs, trunking, and inter-VLAN routing.

# VLAN Configuration and Inter-VLAN Routing (Router-on-a-Stick)

This project simulates a small office network using Cisco Packet Tracer. It demonstrates VLAN segmentation, inter-VLAN routing using Router-on-a-Stick, and DHCP services via router.

## 📁 Topology Overview

- 1 Router
- 1 Layer 2 Switch
- 2 VLANs (IT & Admin)
- 2 PCs (one in each VLAN)
- Trunk link between switch and router
- DHCP provided by router

## 🔧 Switch Configuration

```bash
enable
configure terminal

! Create VLANs
vlan 10
name IT
exit
vlan 20
name Admin
exit

! Assign ports to VLANs
interface fa0/1
switchport mode access
switchport access vlan 10
exit

interface fa0/2
switchport mode access
switchport access vlan 20
exit

! Configure trunk link to router
interface fa0/24
switchport mode trunk
exit

end
write memory
```

### 🔧 Router Configuration
```bash
enable
configure terminal

! Configure subinterfaces for inter-VLAN routing
interface g0/0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface g0/0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

! Activate main interface
interface g0/0/0
no shutdown
exit

! Exclude router IPs from DHCP scope
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10

! Configure DHCP pools
ip dhcp pool VLAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
exit

ip dhcp pool VLAN20
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
exit

end
write memory

```

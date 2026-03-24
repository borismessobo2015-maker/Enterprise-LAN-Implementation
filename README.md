# Enterprise-LAN-Implementation
Designed and implemented a multi-floor enterprise LAN for TechNova Ltd. using Cisco Packet Tracer.  The objective was to build a scalable, segmented, and redundant network infrastructure supporting multiple departments while ensuring efficient Layer 2 and Layer 3 communication.

Business Requirements
- Departmental segmentation using VLANs
- Inter-VLAN communication via Router-on-a-Stick (ROAS)
- High availability using EtherChannel
- Loop prevention using STP / RSTP
- Efficient IP subnetting for current and future growth

Scenario
Designed a network for a 2-floor office with the following departments:
Floor 1: IT Support, HR
Floor 2: Finance, Sales
Each department operates within its own broadcast domain (VLAN) while maintaining controlled interconnectivity.

1. IP Addressing & Subnetting
 IP block 220.0.0.0/24 was subnetted using /28 (255.255.255.240) to optimize address usage and allow scalability.
- Sales department - VLAN10 - (Network 220.0.0.0/28 ; Hosts 220.0.0.1-14 ; Gateway 220.0.0.14 ; broadcast 220.0.0.15)
- Finance department - VLAN20 - (Network 220.0.0.16/28 ; Hosts 220.0.0.17-30 ; Gateway 220.0.0.30 ; broadcast 220.0.0.31)
- HR department - VLAN30 - (Network 220.0.0.32/28 ; Hosts 220.0.0.33-46 ; Gateway 220.0.0.46 ; broadcast 220.0.0.47)
- IT support department - VLAN40 - (Network 220.0.0.48/28 ; Hosts 220.0.0.49-62 ; Gateway 220.0.0.62 ; broadcast 220.0.0.63)
Leaves room for 12 additional subnets for future expansion.

2. Network Topology
- 1 Router (ROAS)
- 2 Distribution Switches (DSW1, DSW2)
- 2 Access Switches (ASW1, ASW2)
- 20 End Devices
Design follows a hierarchical model:
Access layer → end devices
Distribution layer → aggregation + redundancy

3. VLAN & VTP Configuration
VTP Domain: TechnovaVTP
DSW1 configured as VTP Server
Other switches as VTP Clients

VLANs created:
vlan 10
vlan 20
vlan 30
vlan 40

4. Access Port Assignment
Proper VLAN segmentation achieved

5. Trunking Configuration
interface fa0/1
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 10,20,30,40

6. EtherChannel Implementation
EtherChannel configured using bundled FastEthernet links for:
Switch-to-switch redundancy
Increased bandwidth

7. Inter-VLAN Routing (ROAS)
Router subinterfaces configured:
interface g0/0.10
encapsulation dot1Q 10
ip address 220.0.0.14 255.255.255.240

Same logic applied for all VLANs

8. Spanning Tree Optimization
Rapid Spanning Tree enabled:
  spanning-tree mode rapid-pvst
Load balancing strategy:
  ASW1 → root for VLAN 30 & 40
  ASW2 → root for VLAN 10 & 20
Prevents loops + optimizes traffic paths

9. End Device Configuration
Each host manually configured with:
IP address
Subnet mask
Default gateway (router subinterface)

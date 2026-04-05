🇪🇸 [Español](README.md) | 🇬🇧 **English**

# 🌐 Multi-Site Enterprise Network: OSPF, VLANs, DHCP, LACP & RSTP

<div align="center">
 
[![Cisco](https://img.shields.io/badge/Cisco-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)](https://www.cisco.com/)
[![OSPF](https://img.shields.io/badge/OSPF-Dynamic_Routing-005C8A?style=for-the-badge&logo=cisco)](https://www.cisco.com/c/en/us/support/ip/open-shortest-path-first-ospf/tsd-products-support-series-home.html)
[![VLAN](https://img.shields.io/badge/VLAN-Segmentation-008000?style=for-the-badge&logo=cisco)](https://www.cisco.com/c/en/us/support/lan-switching/virtual-lan-vlan/tsd-products-support-series-home.html)
[![EtherChannel](https://img.shields.io/badge/EtherChannel-LACP-FF6600?style=for-the-badge&logo=cisco)](https://www.cisco.com/c/en/us/tech/lan-switching/etherchannel/index.html)
[![RSTP](https://img.shields.io/badge/RSTP-Rapid_PVST-4B0082?style=for-the-badge&logo=cisco)](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9300/software/release/16-9/configuration_guide/layer2/b_169_lay2_9300_cg/configuring_rapid_pvst.html)
[![DHCP](https://img.shields.io/badge/DHCP-Centralized-2E8B57?style=for-the-badge&logo=dhcp)](https://www.cisco.com/c/en/us/support/ip/dynamic-host-configuration-protocol-dhcp/tsd-products-support-series-home.html)
[![IPv4](https://img.shields.io/badge/IPv4-VLSM-FFD700?style=for-the-badge&logo=ipfs)](https://en.wikipedia.org/wiki/Variable-length_subnet_mask)
[![PortFast](https://img.shields.io/badge/PortFast-BPDU_Guard-8A2BE2?style=for-the-badge&logo=cisco)](https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol/30957-portfast-bpduguard.html)
[![Security](https://img.shields.io/badge/Security-Layer_2-FF0000?style=for-the-badge&logo=cisco)](https://www.cisco.com/c/en/us/products/security/what-is-network-security.html)
[![Status](https://img.shields.io/badge/Status-Completed-00AA00?style=for-the-badge&logo=checkmarx)](https://github.com/fredcastillo)
[![Documentation](https://img.shields.io/badge/Documentation-Full-4CAF50?style=for-the-badge&logo=readthedocs)](https://github.com/fredcastillo)
![Topology](diagrams/topology.png)

</div>
   
---

## 🧾 Description

This project involves the design and implementation of a distributed enterprise network across four sites (Dominican Republic, Jamaica, Colombia, and Venezuela), simulated in **Cisco Packet Tracer**.

Modern networking technologies and best practices are applied, including OSPF dynamic routing, VLAN segmentation, link redundancy with LACP, centralized DHCP, and Layer 2 security measures.

---

## ⚙️ Technologies used (actual configurations)

- **OSPF** (Area 0 on all routers and Layer 3 switches)
- **VLANs** (Departmental segmentation)
- **VTP** (Not used; VLANs are statically configured)
- **Centralized DHCP** (R-DO as DHCP server for all networks)
- **EtherChannel (LACP)** (Redundancy and link aggregation)
- **Router-on-a-Stick** (Subinterfaces on R-Colombia)
- **Rapid-PVST** (Fast Spanning Tree convergence)
- **Layer 2 security** (PortFast + BPDU Guard, shutdown interfaces)

---

## Main features

✅ Full connectivity between all sites (ring topology with R-DO as core)  
✅ Departmental segmentation using VLANs  
✅ **VLSM** implementation for IP address optimization  
✅ Redundancy with **EtherChannel (LACP)** on core and access switches  
✅ **Centralized DHCP** from the Dominican Republic (R-DO)  
✅ Isolation VLAN (`Unused`) for unused ports  
✅ Native VLAN (300) on trunk links  
✅ Fast convergence with **Rapid-PVST**  
✅ Inactive interfaces shut down  
✅ `ip helper-address` for DHCP relay on SVIs  

---

## 🏢 VLAN structure per country

### 🇻🇪 Venezuela (Base network: 10.75.8.0/20)
| VLAN | Name         | Network       | Mask           | Gateway       |
|------|--------------|---------------|----------------|---------------|
| 10   | Engineering  | 10.75.8.0     | /24            | 10.75.8.1     |
| 20   | Finance      | 10.75.9.0     | /25            | 10.75.9.1     |
| 30   | Unused       | -             | -              | -             |
| 300  | Native       | -             | -              | -             |

### 🇨🇴 Colombia (Base network: 10.75.11.0/20)
| VLAN | Name         | Network        | Mask           | Gateway         |
|------|--------------|----------------|----------------|-----------------|
| 40   | Sales        | 10.75.11.0     | /25            | 10.75.11.1      |
| 50   | Purchasing   | 10.75.11.128   | /25            | 10.75.11.129    |
| 60   | Unused       | -              | -              | -               |
| 300  | Native       | -              | -              | -               |

### 🇯🇲 Jamaica (Base network: 10.75.16.0/20)
| VLAN | Name         | Network       | Mask           | Gateway       |
|------|--------------|---------------|----------------|---------------|
| 70   | Admin        | 10.75.16.0    | /24            | 10.75.16.1    |
| 80   | Network      | 10.75.17.0    | /24            | 10.75.17.1    |
| 90   | Unused       | -             | -              | -             |
| 300  | Native       | -             | -              | -             |

---

## 🇩🇴 Dominican Republic (Network core)

- **Internal network:** `192.21.75.0/24`
- **Main gateway:** `192.21.75.1` (R-DO)
- **Role:** DHCP Server for all networks and VLANs

---

## 🌐 WAN links

Each link is a `/30`. Base network: `200.21.75.0/24`.

| Link                          | Network         | IP R1          | IP R2         |
|-------------------------------|-----------------|----------------|----------------|
| R-DO ↔ R-Venezuela            | 200.21.75.0/30  | .1 (R-DO)      | .2 (Venezuela) |
| R-Venezuela ↔ R-Colombia      | 200.21.75.4/30  | .5 (Venezuela) | .6 (Colombia)  |
| R-Colombia ↔ R-Jamaica        | 200.21.75.8/30  | .9 (Colombia)  | .10 (Jamaica)  |
| R-Jamaica ↔ R-DO              | 200.21.75.12/30 | .13 (Jamaica)  | .14 (R-DO)     |

---

## Router ↔ Layer 3 Switch Links
| Country   | Network           | Router IP | Core Switch IP |
|-----------|-------------------|-----------|----------------|
| Venezuela | 172.20.75.0/30    | .1        | .2             |
| Colombia  | Not applicable    | -         | -              |
| Jamaica   | 172.10.75.0/30    | .1        | .2             |

> **Note:** R-Colombia does not have a Layer 3 core switch; its subinterfaces (`G0/0.40` and `G0/0.50`) connect directly to an access switch. In Venezuela and Jamaica, there is a core switch with routing enabled.

---

## 🔧 Actual configuration snippets (extracted from devices)

### Centralized DHCP on R-DO

```cisco
ip dhcp excluded-address 192.21.75.1 192.21.75.10
ip dhcp excluded-address 10.75.8.1 10.75.8.10
ip dhcp excluded-address 10.75.9.1 10.75.9.10
ip dhcp excluded-address 10.75.11.1 10.75.11.10
ip dhcp excluded-address 10.75.11.129 10.75.11.139

ip dhcp pool VENEZUELA-ENGINEERING
 network 10.75.8.0 255.255.255.0
 default-router 10.75.8.1
 dns-server 8.8.8.8

ip dhcp pool COLOMBIA-SALES
 network 10.75.11.0 255.255.255.128
 default-router 10.75.11.1
 dns-server 8.8.8.8

ip dhcp pool JAMAICA-ADMIN
 network 10.75.16.0 255.255.255.0
 default-router 10.75.16.1
 dns-server 8.8.8.8
```

## OSPF on R-DO (serial links)

```cisco
router ospf 1
 router-id 1.1.1.1
 network 192.21.75.0 0.0.0.255 area 0
 network 200.21.75.0 0.0.0.3 area 0
 network 200.21.75.12 0.0.0.3 area 0
```

 ## Subinterfaces on R-Colombia (Router-on-a-Stick)
 
```cisco
interface GigabitEthernet0/0.40
 description VLAN 40 SALES
 encapsulation dot1Q 40
 ip address 10.75.11.1 255.255.255.128
 ip helper-address 200.21.75.1
 ip helper-address 200.21.75.14

interface GigabitEthernet0/0.50
 description VLAN 50 PURCHASING
 encapsulation dot1Q 50
 ip address 10.75.11.129 255.255.255.128
 ip helper-address 200.21.75.1
 ip helper-address 200.21.75.14

```

 ## LACP on Sw-Central-Venezuela (Active EtherChannel)

```cisco
 interface Port-channel1
 switchport trunk native vlan 300
 switchport trunk allowed vlan 10,20,300
 switchport mode trunk
 spanning-tree link-type point-to-point

interface FastEthernet0/2
 channel-protocol lacp
 channel-group 1 mode active
```

 ## Security on access ports (Sw-vene3)

 ```cisco
interface FastEthernet0/6
 switchport access vlan 10
 spanning-tree portfast
 spanning-tree bpduguard enable
```

## IP Helper-Address on Sw-Central-Venezuela

 ```cisco
interface Vlan10
 ip address 10.75.8.1 255.255.255.0
 ip helper-address 200.21.75.1
```

## Spanning Tree Rapid-PVST and priority (Sw-Central-Jamaica)

 ```cisco
spanning-tree mode rapid-pvst
spanning-tree vlan 1,70,80,300 priority 24576
```

## Real verifications (EtherChannel, trunk, STP)

 ```cisco
Sw-Central-Venezuela#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      300
Fa0/2       on           802.1q         trunking      300

Sw-vene1#show etherchannel port-channel
Group: 1
Port-channel: Po1    (Primary Aggregator)
Number of ports = 2
Protocol = LACP

Sw-Central-Jamaica#show spanning-tree vlan 70
VLAN0070
  Root ID    Priority    24646
             This bridge is the root
```

## 📸 Lab validations

| Test | Screenshot |
|--------|---------|
| Ping between VLANs | ![Ping VLANs](screenshots/01_ping_intervlan.png) |
| Vecinos OSPF | ![OSPF Neighbors](screenshots/02_ospf_neighbors.png) |
| DHCP Bindings | ![DHCP](screenshots/03_dhcp_bindings.png) |
| EtherChannel Summary | ![EtherChannel](screenshots/04_etherchannel_summary.png) |
| VTP Status | ![VTP Status](screenshots/05_vtp_status.png) |
| Trunk ports | ![Trunk](screenshots/06_trunk_ports.png) |
| PortFast + BPDUGuard | ![PortFast](screenshots/07_portfast_bpduguard.png) |
| STP rapid | ![STP rapid](screenshots/08_stp_rapid.png) |
| VLAN Brief | ![VLAN Brief](screenshots/09_vlan_brief.png) |

> **Nota:** PortFast and BPDU Guard were manually applied on access ports”.

## 📁 Project structure

```
multi-site-enterprise-network/
├── pkt/
│   └── multi-site-enterprise-network.pkt
├── configs/
│   ├── routers/
│   │   ├── R-DO.txt
│   │   ├── R-Venezuela.txt
│   │   ├── R-Colombia.txt
│   │   └── R-Jamaica.txt
│   └── switches/
│       ├── Sw-Central-Venezuela.txt
│       ├── Sw-Central-Colombia.txt
│       ├── Sw-Central-Jamaica.txt
│       ├── Sw-vene1.txt
│       ├── Sw-vene2.txt
│       ├── Sw-vene3.txt
│       └── Sw-vene4.txt
│       └── Sw-Colombia1.txt
│       └── Sw-Colombia2.txt
│       └── Sw-Jamaica1.txt
│       └── Sw-Jamaica2.txt
├── diagrams/
│   └── topology.png
├── screenshots/
│   ├── 01_ping_intervlan.png
│   ├── 02_ospf_neighbors.png
│   ├── 03_dhcp_bindings.png
│   ├── 04_etherchannel_summary.png
│   ├── 05_vtp_status.png
│   ├── 06_trunk_ports.png
│   └── 07_portfast_bpduguard.png
│   └── 08_stp_rapid.png
│   └── 09_vlan_brief.png
├── README.md
└── README-EN.md
```

## How to Use This Project

This lab can be used as a learning environment, configuration reference, or hands-on practice for enterprise networking.

## Requirements

Cisco Packet Tracer 8.2 or later

## Steps

- Open the .pkt file located in /pkt
- Explore the topology and devices
- Review configurations in /configs
- Validation Commands
- ping → Verify connectivity across VLANs and sites
- show ip ospf neighbor → Check OSPF neighbors
- show ip route → Verify learned routes
- show ip dhcp binding → Confirm DHCP leases
- show etherchannel summary → Verify LACP
- show interfaces trunk → Check trunk links
- show spanning-tree summary → Validate STP

### Objective

Understand how multiple networking technologies interact in a distributed enterprise environment, simulating real-world infrastructure.

## Results

Full inter-VLAN communication
Complete connectivity across all sites
Centralized DHCP functioning correctly
Redundant links operational (LACP)
Fast convergence using Rapid-PVST
Layer 2 security properly implemented
Unused interfaces disabled
DHCP relay functioning via ip helper-address


### 👨‍💻 Author

Fred Castillo
Information Security Student
Aspiring Red Team | Offensive Security

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Fred%20Castillo-0077B5?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/fredcastillo11/)
[![GitHub](https://img.shields.io/badge/GitHub-fredcastillo-100000?style=for-the-badge&logo=github)](https://github.com/fredcastillo)








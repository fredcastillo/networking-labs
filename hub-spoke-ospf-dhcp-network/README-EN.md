🇬🇧 **English** | 🇪🇸 [Español](README.md)

# Hub-and-Spoke OSPF Network with DHCP

<div align="center">
   
![Cisco](https://img.shields.io/badge/Cisco-Networking-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![Packet Tracer](https://img.shields.io/badge/Simulator-Cisco%20Packet%20Tracer-blue?style=for-the-badge)
![Routing](https://img.shields.io/badge/Routing-OSPF-orange?style=for-the-badge)
![DHCP](https://img.shields.io/badge/Service-DHCP-green?style=for-the-badge)
![Topology](https://img.shields.io/badge/Topology-Hub%20and%20Spoke-purple?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-black?style=for-the-badge)

<img width="583" height="494" alt="image" src="https://github.com/user-attachments/assets/a0a477b2-ce50-4311-81db-7e6d40b59624" />
</div>

## 📝 Project Description

This repository contains the simulation and configuration of a **multi-site enterprise network** designed in **Cisco Packet Tracer**, part of my networking labs collection. The project implements a hub-and-spoke (star) topology where a central router interconnects eight provincial routers, each providing service to its own local network.

The main objective is to demonstrate the implementation of fundamental networking protocols (OSPF, DHCP) and configuration best practices in a simulated corporate environment, showcasing key skills for a **Network Engineering** or **Cybersecurity** profile.

## Objectives and Skills Demonstrated

* **Network Design:** Implementation of a hierarchical **Hub-and-Spoke** topology to centralize routing and simplify management.
* **Dynamic Routing:** Configuration of **OSPF (Open Shortest Path First)** in a single area (Area 0) for automatic route discovery and fast convergence.
* **Network Services:** Configuration of **DHCPv4** on the central router for automatic and centralized IP address assignment to all hosts in the provinces.
* **Device Configuration:** Assignment of static IP addresses to router interfaces, configuration of serial links (DCE/DTE), and standardization of access credentials (VTY, console).
* **Network Segmentation:** Use of multiple **independent IP subnets** to separate traffic from each province and its departments within the enterprise network.
* **Verification and Troubleshooting:** Use of `show` commands and connectivity tests (`ping`) to validate the overall network functionality.
* **Technical Documentation:** Creation of clear and professional documentation in multiple languages.

## 🏗️ Network Topology

The network follows a classic hub-and-spoke design:

* **Central Router (SEDE_CENTRAL_0):** Acts as the "hub". It concentrates all WAN connections from provincial routers, runs the main OSPF process, and hosts DHCP pools for all local networks.
* **Provincial Routers (PROVINCIA1 to PROVINCIA8):** Act as "spokes". Each connects to the hub through point-to-point serial links and provides connectivity to its respective LAN networks.
* **Switches and Hosts:** Each province has access layer switches connecting multiple PCs, which obtain their IP configuration dynamically from the central DHCP server.

### IP Addressing Scheme

* **WAN Links (Serial):** `/30` networks used to maximize addressing space.
    * Connection to PROVINCIA #1: `192.168.21.0/30`
    * Connection to PROVINCIA #2: `192.168.22.0/30`
    * ... and so on (see [complete documentation](docs/network-description.md)).
* **Local Networks (LAN):** `/24` networks for each department/province.
    * PROVINCIA #1: `192.168.9.0/24` and `192.168.10.0/24`
    * PROVINCIA #2: `192.168.11.0/24` and `192.168.12.0/24`
    * ... (See [network description](docs/network-description.md) for full details).

## 🛠️ Technologies and Protocols Used

* **Cisco Packet Tracer:** Network simulation tool.
* **OSPFv2 Protocol:** Dynamic routing (Process ID 1, Area 0).
* **DHCPv4:** Dynamic IP address assignment.
* **Serial Links:** WAN connections with default HDLC encapsulation.
* **Basic Security Standards:**
  * Standard passwords (`cisco123`) for console and VTY lines.
  * `enable secret` for privileged mode.
  * **MOTD (Message of the Day)** banner with access warning.

## ⚙️ Key Configurations (Summary)

The complete configurations for all devices are available in the [`/configs`](./configs/) folder.

### Central Router (SEDE_CENTRAL_0) - Extract
```cisco
!
hostname SEDE_CENTRAL_0
!
ip dhcp excluded-address 192.168.9.1 192.168.9.10
ip dhcp pool LAN_PROVINCIA1_RED9
 network 192.168.9.0 255.255.255.0
 default-router 192.168.9.1
!
interface Serial0/0/0
 description Connection to PROVINCIA #1
 ip address 192.168.21.1 255.255.255.252
 clock rate 56000
!
router ospf 1
 router-id 0.0.0.1
 network 192.168.21.0 0.0.0.3 area 0
 network 192.168.22.0 0.0.0.3 area 0
 ! ... (other WAN networks)
!
banner motd ^CFRED SNEYDER CASTILLO APOLINAR - Restricted Access^C
!
line con 0
 password cisco123
 login
!
```

### Provincial Router (PROVINCIA#1) - Extract
```cisco
!
hostname PROVINCIA#1
!
interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 192.168.21.1
!
interface Serial0/0/0
 description Connection to SEDE_CENTRAL_0
 ip address 192.168.21.2 255.255.255.252
!
router ospf 1
 router-id 1.1.0.0
 network 192.168.21.0 0.0.0.3 area 0
 network 192.168.9.0 0.0.0.255 area 0
 network 192.168.10.0 0.0.0.255 area 0
!
```

Note: The `ip helper-address` command is crucial, as it allows the provincial router to forward DHCP requests from its local clients to the central DHCP server.

## 🖥️ Connectivity Verification

The correct operation of all services and end-to-end connectivity has been verified.

```bash
# On the central router: View OSPF neighbors
SEDE_CENTRAL_0# show ip ospf neighbor

# On a provincial router: View routes learned via OSPF
PROVINCIA#1# show ip route ospf

# On a PC: Verify DHCP assigned IP
PC> ipconfig

# Connectivity test between PCs in different provinces
PC> ping 192.168.15.10
```

## 📂 Repository Structure

```
networking-labs/
└── hub-spoke-ospf-dhcp-network/
    ├── README.md
    ├── README-EN.md
    ├── LICENSE
    ├── docs/
    │   ├── network-description.md
    │   └── analysis.md
    ├── configs/
    │   ├── sede_central_0_startup-config.cfg
    │   ├── provincia1_startup-config.cfg
    │   └── ...
    └── assets/
        ├── screenshots/
        └── diagrams/
```

## 🕵️ How to Use This Project

1. Clone the complete networking labs repository:

```bash
git clone https://github.com/fredcastillo/networking-labs.git
cd networking-labs/hub-spoke-ospf-dhcp-network
```

3. Open the `.pkt` file (if included) in Cisco Packet Tracer.

4. Review the configurations in the `/configs` folder. You can load them into the simulated devices to replicate the lab.

5. Experiment: Try disconnecting links, modifying OSPF costs, or adding new networks to observe the protocol's behavior.

## 📜 License

This project is licensed under the MIT License. See the LICENSE file for more details.

#### 👨‍💻 Author

**Fred Castillo**  
*Aspirant Red Team | Offensive Security*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Fred%20Castillo-0077B5?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/fredcastillo11/)
[![GitHub](https://img.shields.io/badge/GitHub-fredcastillo-100000?style=for-the-badge&logo=github)](https://github.com/fredcastillo)

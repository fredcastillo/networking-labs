🇪🇸 **Español** | 🇬🇧 [English](README-EN.md)

<div align="center">

# 🖧 Multiarea OSPF + DMZ Network Lab
![Topología](diagrams/topology.png)

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco)
![OSPF](https://img.shields.io/badge/OSPF-Multiarea-FF6F00?style=for-the-badge&logo=openstreetmap)
![VLAN](https://img.shields.io/badge/VLAN-802.1Q-0052CC?style=for-the-badge&logo=)
![DHCP](https://img.shields.io/badge/DHCP-Centralizado-4B8BBE?style=for-the-badge&logo=)
![DMZ](https://img.shields.io/badge/Zona-DMZ-important?style=for-the-badge&logo=)

</div>

---

## Descripción

Este laboratorio simula una red corporativa segmentada en **VLANs**, con enrutamiento **OSPF multiárea**, un **servidor DHCP centralizado** y una **zona DMZ** aislada para servicios públicos. La topología está diseñada para demostrar conceptos avanzados de redes Cisco, como:

- Inter-VLAN routing con un switch multicapa (Layer 3 Switch).
- OSPF Área 0 (backbone) y Áreas 1 y 2.
- DHCP centralizado en el router R1.
- Segmentación por departamentos y funciones.
- Trunking y VLAN nativa.
- Zona DMZ con VLANs dedicadas para servidores web y cloud.

> **Objetivo:** Servir como material de estudio, práctica o referencia para implementaciones reales de redes empresariales.

---

## 🗺️ Topología

- **Router R1** (Cisco 2911): Gateway principal, servidor DHCP, conexión a DMZ y backbone OSPF.
- **Switch Multicapa SW-MULTICAPA** (Layer 3): Enrutamiento inter-VLAN, OSPF Área 1, troncal hacia R1.
- **Switch ZONA-DMZ**: Switch de acceso para la DMZ, con VLANs 100 y 200.
- **Switches de acceso** (no configurados en este extracto): conectan los hosts de cada departamento.

---

## 📋 VLANs y Direccionamiento IP

| VLAN | Nombre          | Subred           | Área OSPF |
|------|-----------------|------------------|-----------|
| 10   | Comercio        | 192.168.10.0/24  | 1         |
| 20   | Marketing       | 192.168.20.0/24  | 1         |
| 30   | Publicidad      | 192.168.30.0/24  | 1         |
| 40   | IT              | 192.168.40.0/24  | 1         |
| 50   | NOC-TEAM        | 192.168.50.0/24  | 1         |
| 60   | SOC-TEAM        | 192.168.60.0/24  | 1         |
| 70   | Administración  | 192.168.70.0/24  | 1         |
| 80   | Producción      | 192.168.80.0/24  | 1         |
| 99   | NATIVE          | N/A              | –         |
| 100  | DMZ-1 (Web)     | 192.168.100.0/24 | 2         |
| 200  | DMZ-2 (Cloud)   | 192.168.200.0/24 | 2         |

**Backbone (Área 0):** `200.10.1.0/24` entre R1 y SW-MULTICAPA.

---

## 🔧 Funcionalidades Implementadas

### 🔹 OSPF Multiárea
- **R1** es ABR (Area Border Router) entre Área 0 y Área 2.
- **SW-MULTICAPA** es ABR entre Área 0 y Área 1.
- Todas las VLANs de Área 1 y Área 2 son inyectadas en OSPF.
- Verificación de vecinos: `show ip ospf neighbor`.

### 🔹 DHCP Centralizado
- El router R1 tiene pools DHCP para cada VLAN (10–80 y 100, 200).
- Exclusiones de direcciones para evitar conflictos (primeras 9 IPs de cada subred).
- Cada SVI (Switch Virtual Interface) en SW-MULTICAPA usa `ip helper-address 200.10.1.1` para reenviar solicitudes DHCP al router R1.

### 🔹 Enrutamiento Inter-VLAN
- Las interfaces VLAN (SVI) en SW-MULTICAPA actúan como gateways para cada departamento.
- El switch multicapa tiene `ip routing` habilitado.

### 🔹 Zona DMZ
- El switch ZONA-DMZ separa los servidores web y cloud en VLANs 100 y 200.
- La conexión a R1 es un enlace troncal (dot1q) con VLANs 100 y 200.
- La DMZ está en OSPF Área 2, aislada de las VLANs internas.

### 🔹 Trunking y VLAN Nativa
- El enlace entre R1 y ZONA-DMZ: trunk con VLANs 100 y 200, VLAN nativa 99.
- El enlace entre SW-MULTICAPA y sus switches de acceso (no mostrado en configs) también debe usar trunk con VLANs permitidas y nativa 99.

---

## 📂 Archivos de Configuración

Todos los dispositivos tienen sus configuraciones completas en la carpeta `Configs/`. Los archivos incluyen:

- `R1-config.txt`
- `SW-MULTICAPA-config.txt`
- `ZONA-DMZ-config.txt`
- `Switch-Acceso-XX-config.txt`

Estos archivos pueden ser copiados a dispositivos reales o importados en Packet Tracer.

---

## Cómo Usar

1. **Descarga** el archivo `.pkt` de la carpeta `pkt/`.
2. **Ábrelo** con Cisco Packet Tracer (versión 8.2 o superior recomendada).
3. **Verifica** las configuraciones con los comandos `show` (ej. `show ip route`, `show ip ospf neighbor`).
4. **Prueba conectividad** entre VLANs y hacia la DMZ usando ping.
5. **Revisa la asignación DHCP** en los PCs (deben obtener IP dentro de su VLAN).

---

## 📸 Capturas de Pantalla

En la carpeta `screenshots/` encontrarás evidencias de:

- Tablas de enrutamiento OSPF.
- Vecinos OSPF establecidos.
- Pools DHCP y concesiones.
- VLANs activas y puertos asignados.

---

## Tecnologías Utilizadas

| Tecnología       | Descripción                               |
|------------------|-------------------------------------------|
| Cisco Packet Tracer | Simulador de redes Cisco               |
| OSPF             | Protocolo de enrutamiento de estado de enlace |
| VLAN (802.1Q)    | Segmentación de red en capa 2            |
| DHCP             | Asignación dinámica de IP                |
| Layer 3 Switching | Enrutamiento inter-VLAN                  |
| DMZ              | Zona desmilitarizada para servicios expuestos |

---

## 📖 Referencias

- Cisco IOS Configuration Guides
- Packet Tracer Documentation
- OSPF RFC 2328

---

#### 👨‍💻 Autor

**Fred Castillo**  
*Estudiante de Tecnólogo en Seguridad Informática*  
*Aspirante a Red Team | Seguridad Ofensiva*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Fred%20Castillo-0077B5?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/fredcastillo11/)  
[![GitHub](https://img.shields.io/badge/GitHub-fredcastillo-100000?style=for-the-badge&logo=github)](https://github.com/fredcastillo)

---

> **Nota:** Este laboratorio ha sido probado en Cisco Packet Tracer 8.2.1. Si encuentras algún error o mejora, no dudes en contribuir.

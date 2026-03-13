🇪🇸 **Español** | 🇬🇧 [English](README-EN.md)

# 🌐 Hub-and-Spoke OSPF Network with DHCP

<div align="center">
<img width="583" height="494" alt="image" src="https://github.com/user-attachments/assets/a0a477b2-ce50-4311-81db-7e6d40b59624" />

![Cisco](https://img.shields.io/badge/Cisco-Networking-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![Packet Tracer](https://img.shields.io/badge/Simulator-Cisco%20Packet%20Tracer-blue?style=for-the-badge)
![Routing](https://img.shields.io/badge/Routing-OSPF-orange?style=for-the-badge)
![DHCP](https://img.shields.io/badge/Service-DHCP-green?style=for-the-badge)
![Topology](https://img.shields.io/badge/Topology-Hub%20and%20Spoke-purple?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-black?style=for-the-badge)

</div>

## 📝 Descripción del Proyecto

Este repositorio contiene la simulación y configuración de una **red empresarial multi-sitio** diseñada en **Cisco Packet Tracer**, ubicada dentro de mi colección de laboratorios de networking. El proyecto implementa una topología hub-and-spoke (estrella) donde un router central interconecta ocho routers provinciales, cada uno dando servicio a su propia red local.

El objetivo principal es demostrar la implementación de protocolos de red fundamentales (OSPF, DHCP) y buenas prácticas de configuración en un entorno corporativo simulado, mostrando habilidades clave para un perfil de **Ingeniería de Redes** o **Ciberseguridad**.

## 🎯 Objetivos y Habilidades Demostradas

*   **Diseño de Red:** Implementación de una topología jerárquica **Hub-and-Spoke** para centralizar el enrutamiento y simplificar la gestión.
*   **Enrutamiento Dinámico:** Configuración de **OSPF (Open Shortest Path First)** en área única (Area 0) para el descubrimiento automático de rutas y convergencia rápida.
*   **Servicios de Red:** Configuración de **DHCPv4** en el router central para la asignación automática y centralizada de direcciones IP a todos los hosts de las provincias.
*   **Configuración de Dispositivos:** Asignación de direcciones IP estáticas a interfaces de routers, configuración de enlaces seriales (DCE/DTE) y estandarización de credenciales de acceso (VTY, consola).
*   **Segmentación de Red:** Uso de múltiples **subredes IP independientes** para separar el tráfico de cada provincia y sus departamentos dentro de la red empresarial.
*   **Verificación y Troubleshooting:** Uso de comandos `show` y pruebas de conectividad (`ping`) para validar el funcionamiento integral de la red.
*   **Documentación Técnica:** Elaboración de documentación clara y profesional en múltiples idiomas.

## 🏗️ Topología de Red

La red sigue un diseño clásico de concentrador (hub) y radios (spoke):

*   **Router Central (SEDE_CENTRAL_0):** Actúa como el "hub". Concentra todas las conexiones WAN de los routers provinciales, ejecuta el proceso OSPF principal y aloja los pools de DHCP para todas las redes locales.
*   **Routers Provinciales (PROVINCIA1 a PROVINCIA8):** Actúan como "spokes". Cada uno se conecta al hub mediante enlaces seriales punto a punto. Proveen conectividad a sus respectivas redes LAN.
*   **Switches y Hosts:** Cada provincia cuenta con switches de capa de acceso que conectan múltiples PCs, las cuales obtienen su configuración IP dinámicamente del servidor DHCP central.

### Esquema de Direccionamiento IP

*   **Enlaces WAN (Seriales):** Redes `/30` para maximizar el espacio de direccionamiento.
    *   Conexión a PROVINCIA #1: `192.168.21.0/30`
    *   Conexión a PROVINCIA #2: `192.168.22.0/30`
    *   ... y así sucesivamente (ver [documentación completa](docs/network-description.md)).
*   **Redes Locales (LAN):** Redes `/24` para cada departamento/provincia.
    *   PROVINCIA #1: `192.168.9.0/24` y `192.168.10.0/24`
    *   PROVINCIA #2: `192.168.11.0/24` y `192.168.12.0/24`
    *   ... (Ver [descripción de red](docs/network-description.md) para el detalle completo).

## 🛠️ Tecnologías y Protocolos Utilizados

*   **Cisco Packet Tracer:** Herramienta de simulación de redes.
*   **Protocolo OSPFv2:** Enrutamiento dinámico (Process ID 1, Área 0).
*   **DHCPv4:** Asignación dinámica de direcciones IP.
*   **Enlaces Seriales:** Conexiones WAN con encapsulación HDLC por defecto.
*   **Estándares de Seguridad Básica:**
    *   Contraseñas estándar (`cisco123`) para consola y líneas VTY.
    *   `enable secret` para el modo privilegiado.
    *   Mensaje **MOTD (Message of the Day)** con advertencia de acceso.

## ⚙️ Configuraciones Clave (Resumen)

Las configuraciones completas de todos los dispositivos están disponibles en la carpeta [`/configs`](./configs/).

### Router Central (SEDE_CENTRAL_0) - Extracto
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
 description Conexion a PROVINCIA #1
 ip address 192.168.21.1 255.255.255.252
 clock rate 56000
!
router ospf 1
 router-id 0.0.0.1
 network 192.168.21.0 0.0.0.3 area 0
 network 192.168.22.0 0.0.0.3 area 0
 ! ... (otras redes WAN)
!
banner motd ^CFRED SNEYDER CASTILLO APOLINAR - Acceso Restringido^C
!
line con 0
 password cisco123
 login
!
```
### Router Provincial (PROVINCIA#1) - Extracto
```cisco
!
hostname PROVINCIA#1
!
interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 192.168.21.1
!
interface Serial0/0/0
 description Conexion a SEDE_CENTRAL_0
 ip address 192.168.21.2 255.255.255.252
!
router ospf 1
 router-id 1.1.0.0
 network 192.168.21.0 0.0.0.3 area 0
 network 192.168.9.0 0.0.0.255 area 0
 network 192.168.10.0 0.0.0.255 area 0
!
```

Nota: El comando ip helper-address es crucial, ya que permite que el router provincial reenvíe las solicitudes DHCP de sus clientes locales al servidor DHCP central.

##🖥️ Verificación de Conectividad

Se ha verificado el correcto funcionamiento de todos los servicios y la conectividad de extremo a extremo.

```bash
# En el router central: Ver vecinos OSPF
SEDE_CENTRAL_0# show ip ospf neighbor

# En un router provincial: Ver rutas aprendidas por OSPF
PROVINCIA#1# show ip route ospf

# En una PC: Verificar IP obtenida por DHCP
PC> ipconfig

# Prueba de conectividad entre PCs de diferentes provincias
PC> ping 192.168.15.10
```
##📂 Estructura del Repositorio

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

## 🕵️ Cómo Usar Este Proyecto 

1. Clona el repositorio completo de laboratorios:
   
2.   
```bash
git clone https://github.com/fredcastillo/networking-labs.git
cd networking-labs/hub-spoke-ospf-dhcp-network
```

3. Abre el archivo .pkt (si se incluye) en Cisco Packet Tracer.

4. Revisa las configuraciones en la carpeta /configs. Puedes cargarlas en los dispositivos simulados para replicar el laboratorio.

5. Experimenta: Prueba a desconectar enlaces, modificar costos OSPF o agregar nuevas redes para observar el comportamiento del protocolo.


## 📜 Licencia
Este proyecto está bajo la licencia MIT. Ver el archivo LICENSE para más detalles.

#### 👨‍💻 Autor

**Fred Castillo**  
*Aspirante a Red Team | Seguridad Ofensiva*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Fred%20Castillo-0077B5?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/fredcastillo11/)
[![GitHub](https://img.shields.io/badge/GitHub-fredcastillo-100000?style=for-the-badge&logo=github)](https://github.com/fredcastillo)




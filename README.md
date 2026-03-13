# Proyecto de Red: Infraestructura Ecommerce 🛒

Este repositorio contiene el diseño y la configuración técnica de una red corporativa para una empresa de Ecommerce, desarrollada en **Cisco Packet Tracer**.

## 1. Introducción
El objetivo de este proyecto es implementar una red segmentada y segura que dé soporte a las operaciones de una tienda online. Se ha diseñado una topología jerárquica que separa los departamentos críticos para optimizar el tráfico y proteger la información sensible de la empresa.

---

## 2. Especificaciones Técnicas

### Modelo de Red
* **Enrutamiento:** Router-on-a-Stick mediante subinterfaces en el Router principal.
* **Segmentación:** Creación de 7 VLANs para organizar departamentos y servicios.
* **Direccionamiento:** IPv4 con direccionamiento dinámico (DHCP).

### Tabla de VLANs y Direccionamiento
| VLAN | Departamento | Red IP | Gateway | Uso Principal |
| :--- | :--- | :--- | :--- | :--- |
| **10** | ADMIN | `192.168.10.0/24` | `192.168.10.1` | Gestión administrativa |
| **20** | DIR | `192.168.20.0/24` | `192.168.20.1` | Gerencia y Dirección |
| **30** | DEV | `192.168.30.0/24` | `192.168.30.1` | Desarrollo de la Tienda |
| **40** | SOPORTE | `192.168.40.0/24` | `192.168.40.1` | Atención al Cliente |
| **50** | FORMACION | `192.168.50.0/24` | `192.168.50.1` | Aula de formación |
| **60** | SERVIDORES | `192.168.60.0/24` | `192.168.60.1` | Servidores Web y DB (PCs) |
| **99** | GESTIÓN | `192.168.99.0/24` | `192.168.99.1` | Administración de Red (SSH) y WiFi Gestión |

---

## 3. Configuración del Router

Se ha configurado el puerto `GigabitEthernet 0/0` para permitir el paso de múltiples VLANs mediante encapsulamiento **802.1Q**.

### Gestión y WiFi
La **VLAN 99 (Gestión)** centraliza la administración de la red. El Punto de Acceso WiFi se ha integrado en esta VLAN para permitir la gestión inalámbrica segura mediante **SSH**.

### Listas de Control de Acceso (ACLs)
Se mantienen las políticas de seguridad estrictas:
1.  **Aislamiento del Aula:** La VLAN 50 tiene denegado el acceso a la red de Dirección (VLAN 20).
2.  **Seguridad de Servidores:** Solo departamentos autorizados pueden realizar peticiones a la VLAN 60.

### Ejemplo de Subinterfaz y DHCP:
```bash
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 exit

ip dhcp pool ADMIN_POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.60.10

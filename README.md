# Proyecto SAD
## 1. Introducción
Proyecto para el módulo de Seguridad y Alta Disponibilidad de 2º de ASIR para simular la infraestructura y seguridad de una PYME
## 2. Estructura
![](/capturas/capturared.png)
### 1. Gateway y enrutador (gw)
Actúa como router central, cortafuegos (iptables/nftables) y nodo VPN. Separa físicamente (mediante redes internas de VirtualBox) todas las subredes.
- SO: Ubuntu 24.04
- Hostname: gw-pmj
- Interfaces de red:
    - eth0 (NAT): Salida a Internet básica
    - eth1 (Bridge): Conexión puente a la red física del aula (para Site-to-Site VPN).
    - eth2 (DMZ): 172.1.6.1
    - eth3 (Empleados): 172.2.6.1
    - eth4 (Gestión): 172.3.6.1
### 2. LAN de Gestión/Intranet (172.3.6.0/24)
Red para los servidores criticos internos y la administración. No tiene acceso directo desde Internet. Salida a Internet enrutada por el gw.
- Proveedor de identidades (idp)
    - SO: Ubuntu 24.04
    - Hostname: idp-pmj
    - IP: 172.3.6.2
    - Rol: Servidor OpenLDAP
- Servidor de Backups (backup-srv)
    - SO: Alpine Linux
    - Hostname: backup-srv-pmj
    - IP: 172.3.6.20
    - Rol: Tira de los datos de los demás servidores hacia su almacenamiento local de forma segura
### 3. LAN de empleados (172.2.6.0/24)
Red de usuarios estandar. Navegacion restringida a través del proxy.
- Equipo de Administración (adminpc)
    - SO:
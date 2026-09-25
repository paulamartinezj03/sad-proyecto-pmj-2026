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
    - SO: Alpine Linux
    - Hostname: adminpc-pmj
    - IP: 172.2.6.10
    - Rol: Maquina de salto y gestion. Desde aqui el administrador despliega scripts, se conecta por SSH a los demás equipos usando claves, etc.
- Equipo Empleado (empleado)
    - SO: Alpine Linux
    - Hostname: empleadopc-pmj
    - IP: 172.2.6.100
    - Rol: Simula un empleado de la PYME
### 4. DMZ - Zona desmilitarizada
Servicios expuestos o que intermedian con el exterior
- Servidor proxy (proxy)
    - SO: Ubuntu 24.04
    - Hostname: proxy-pmj
    - IP: 172.1.6.2
    - Rol: Proxy web (Squid) para filtrar trafico de los empleados
- Servidor web (www)
    - SO: ALpine Linux
    - Hostname: www.pmj
    - IP: 172.1.6.3
    - Rol: Aloja los servicios web expuestos de la PYME y DVWA para las practicas de Pentesing.
## 3.Instrucciones para el despliegue
### 3.1 Requisitos previos
Tener instalado lo siguiente:
- Git
- Virtualbox
- Vagrant
### 3.2 Despliegue
1. Clonar este repositorio
git clone https://github.com/pes130/vagrantsad.git
2. Levantar con vagrant
```
cd vagrantsad
vagrant up
```
3. Una vez levantado, comprobamos el estado:
```
    vagrant status
```
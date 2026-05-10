# TP Integrador Computación Aplicada

Repositorio grupal del trabajo práctico integrador.


## Integrantes

- Agustin Romero


## Avance del Trabajo

- [x] Entorno VS Code + GitHub configurado
- [x] VM descargada y descomprimida
- [x] VM importada en VirtualBox
- [x] Snapshot inicial creado
- [x] Contraseña root restablecida
- [x] Hostname configurado
- [x] Conectividad de red verificada
- [x] Punto 1 completado
- [ ] Punto 2 completado
- [ ] Punto 3 completado
- [ ] Punto 4 completado


## Desarrollo del Trabajo


### 1. Importación de la máquina virtual

Luego de descargar y descomprimir los archivos de la máquina virtual, se procedió a importar el servicio virtualizado en VirtualBox.

Se modificó la configuración de red de la máquina virtual de “Adaptador Puente” a “NAT”, debido a incompatibilidad con la interfaz física configurada originalmente en la imagen importada.

También se creó un snapshot inicial denominado “Estado inicial limpio”, con el objetivo de disponer de un punto de restauración antes de realizar modificaciones en el sistema.



### 2. Recuperación de contraseña root

Debido a que la contraseña del usuario root era desconocida inicialmente, se inició el sistema en modo recuperación mediante GRUB.

Desde el modo recovery se accedió a una shell de mantenimiento con privilegios de administrador.

Se utilizó el comando `passwd` para realizar el blanqueo y cambio de contraseña del usuario root.

La nueva contraseña configurada fue: `palermo`.

Posteriormente se reinició el sistema e inició sesión correctamente con el nuevo password.


#### Comandos utilizados

```bash
passwd
```

#### Evidencia de recuperación de contraseña

![Password root restablecida](Capturas/passwd-root-reset.png)


### 3. Configuración de hostname

Se configuró el hostname solicitado en la consigna como `TPServer`.

Para aplicar correctamente los cambios en la sesión actual se ejecutó nuevamente una shell de bash.


#### Comandos utilizados

```bash
hostnamectl set-hostname TPServer

exec bash

hostname
```

### 4. Verificación de conectividad de red

Se verificó el correcto funcionamiento de la interfaz de red y el acceso a internet mediante comandos de diagnóstico de red.

Se comprobó la correcta asignación dinámica de dirección IP, gateway y conectividad hacia internet utilizando NAT.

Inicialmente se detectaron errores al ejecutar `apt update`, debido a repositorios obsoletos configurados en la máquina virtual importada.

Se verificó el funcionamiento de conectividad IP utilizando `ping` hacia direcciones públicas, confirmando acceso a internet pero detectando fallas de resolución hacia los mirrors configurados originalmente.

Para solucionar el problema se editó manualmente el archivo `/etc/apt/sources.list`, reemplazando los repositorios antiguos por mirrors oficiales de Debian Bullseye.

Luego de realizar las modificaciones correspondientes, se ejecutó nuevamente `apt update`, verificando el correcto funcionamiento del gestor de paquetes APT y la conectividad DNS del sistema.


#### Comandos utilizados

```bash
ip a

ip route

ping -c 4 google.com

ping -c 4 8.8.8.8

cat /etc/apt/sources.list

vi /etc/apt/sources.list

apt update
```

#### Evidencia de actualización correcta de repositorios

![apt update funcionando](Capturas/apt-update-ok.png)


### 5. Actualización de paquetes del sistema

Luego de verificar la conectividad y corregir los repositorios del sistema, se procedió a actualizar los paquetes instalados en Debian 11 Bullseye.

Se ejecutó el comando `apt upgrade -y`, permitiendo descargar e instalar las actualizaciones disponibles para los paquetes ya instalados en el sistema operativo.

Durante el proceso se actualizaron librerías, herramientas del sistema, certificados y componentes internos de Debian, dejando la máquina virtual preparada para continuar posteriormente con la migración a Debian 12.


#### Comandos utilizados

```bash
cat /etc/os-release

apt upgrade -y
```

#### Evidencia de actualización de paquetes

![apt upgrade Debian 11](Capturas/apt-upgrade-debian11-ok.png)


### 6. Migración de Debian 11 a Debian 12

Luego de actualizar completamente los paquetes de Debian 11 Bullseye, se procedió a realizar la migración de distribución hacia Debian 12 Bookworm.

Para ello se modificó el archivo `/etc/apt/sources.list`, reemplazando las referencias de `bullseye` por `bookworm`.

Posteriormente se ejecutó `apt update` para actualizar los índices de paquetes correspondientes a Debian 12.

Finalmente se realizó la actualización completa del sistema mediante `apt full-upgrade -y`, instalando nuevas dependencias, librerías, kernel y componentes internos del sistema operativo.

Durante el proceso se reconfiguró GRUB seleccionando el disco principal `/dev/sda` como dispositivo de instalación del gestor de arranque.

Luego del reinicio del sistema se verificó correctamente el funcionamiento de Debian 12 Bookworm junto con el nuevo kernel Linux 6.1.


#### Comandos utilizados

```bash
cp /etc/apt/sources.list /etc/apt/sources.list.bak

vi /etc/apt/sources.list

apt update

apt full-upgrade -y

reboot

cat /etc/os-release

uname -r
```

#### Evidencia de migración exitosa a Debian 12

![Debian 12 y kernel 6.1](Capturas/debian12-kernel61-ok.png)

### 7. Instalación y verificación del servicio SSH

Se instaló el servicio OpenSSH Server con el objetivo de permitir la administración remota de la máquina virtual mediante el protocolo SSH.

El servicio SSH permite establecer conexiones remotas seguras utilizando autenticación y cifrado entre cliente y servidor.

Luego de la instalación se verificó el correcto funcionamiento del servicio mediante `systemctl`, comprobando que el daemon `sshd` se encontraba activo y en ejecución.

También se verificó que el servicio se encontrara escuchando conexiones sobre el puerto 22/TCP, correspondiente al puerto estándar utilizado por SSH.


#### Comandos utilizados

```bash
apt install openssh-server -y

systemctl status ssh

ss -tulnp | grep ssh
```

#### Evidencia de servicio SSH activo

![Servicio SSH activo](Capturas/ssh-service-active.png)

### 8. Configuración de IP estática

Con el objetivo de cumplir los requisitos de conectividad de la consigna, se configuró la interfaz de red de la máquina virtual con una dirección IP estática perteneciente al mismo rango de red de la máquina física.

Inicialmente la interfaz obtenía una dirección dinámica mediante DHCP. Posteriormente se modificó manualmente el archivo `/etc/network/interfaces`, reemplazando la configuración dinámica por parámetros estáticos.

Se configuraron los campos `address`, `netmask`, `gateway` y `dns-nameservers`, estableciendo la dirección IP `192.168.1.50` para la interfaz `enp0s3`.

Luego de aplicar los cambios mediante el reinicio del servicio de red, se verificó el correcto funcionamiento de la conectividad local y el acceso a internet.


#### Comandos utilizados

```bash
ip a

ip route

cat /etc/resolv.conf

vi /etc/network/interfaces

systemctl restart networking

ping -c 4 8.8.8.8
```

#### Evidencia de configuración de IP estática

![IP estática configurada](Capturas/ip-estatica-ok.png)



# TP Integrador Computación Aplicada

Repositorio grupal del trabajo práctico integrador.


## Integrantes:

- Agustin Romero


## Avance del Trabajo:

- [x] Entorno VS Code + GitHub configurado
- [x] VM descargada y descomprimida
- [x] VM importada en VirtualBox
- [x] Snapshot inicial creado
- [x] Contraseña root restablecida
- [x] Hostname configurado
- [x] Conectividad de red verificada
- [X] Punto 1 completado
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


### 3. Configuración de hostname

Se configuró el hostname solicitado en la consigna como `TPServer`.

Para aplicar correctamente los cambios en la sesión actual se ejecutó nuevamente una shell de bash.


### 4. Verificación de conectividad de red

Se verificó el correcto funcionamiento de la interfaz de red y el acceso a internet mediante comandos de diagnóstico de red.

Se comprobó la correcta asignación dinámica de dirección IP, gateway y conectividad hacia internet utilizando NAT.

Inicialmente se detectaron errores al ejecutar `apt update`, debido a repositorios obsoletos configurados en la máquina virtual importada.

Se verificó el funcionamiento de conectividad IP utilizando `ping` hacia direcciones públicas, confirmando acceso a internet pero detectando fallas de resolución hacia los mirrors configurados originalmente.

Para solucionar el problema se editó manualmente el archivo `/etc/apt/sources.list`, reemplazando los repositorios antiguos por mirrors oficiales de Debian Bullseye.

Luego de realizar las modificaciones correspondientes, se ejecutó nuevamente `apt update`, verificando el correcto funcionamiento del gestor de paquetes APT y la conectividad DNS del sistema.

### Comandos utilizados

```bash
passwd

hostnamectl set-hostname TPServer
exec bash
hostname

ip a
ip route
ping -c 4 google.com
ping -c 4 8.8.8.8
cat /etc/apt/sources.list
vi /etc/apt/sources.list
apt update

#### Evidencia de actualización correcta de repositorios

![apt update funcionando](Capturas/apt-update-ok.png)
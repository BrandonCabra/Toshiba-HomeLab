# 03. Configuración y administración de OpenSSH

## 1. Objetivo

Documentar la configuración del servicio OpenSSH en el servidor
Toshiba HomeLab, permitiendo su administración remota desde otros
equipos mediante conexiones seguras.

## 2. Tecnología utilizada

- Servicio: OpenSSH Server
- Versión del cliente: OpenSSH 10.2p1
- Sistema operativo: Ubuntu Server 26.04.1 LTS
- Protocolo: SSH
- Puerto: 22
- Administrador: systemd

## 3. Arquitectura del servicio

La instalación utiliza activación por sockets (Socket Activation)
mediante systemd.

Se identifican dos componentes principales:

- ssh.socket: escucha las conexiones entrantes.
- ssh.service: ejecuta el servidor SSH y gestiona las conexiones.

## 4. Configuración del socket

Archivo: /usr/lib/systemd/system/ssh.socket

Parámetros principales:

| Parámetro | Configuración |
|---|---|
| IPv4 | 0.0.0.0:22 |
| IPv6 | [::]:22 |
| Accept | no |
| FreeBind | yes |
| Inicio automático | enabled |

## 5. Configuración del servicio

Archivo: /usr/lib/systemd/system/ssh.service

Se destacan las siguientes directivas:

- ExecStartPre: valida la configuración antes del inicio.
- ExecStart: ejecuta el servidor sshd.
- Restart: permite reiniciar el servicio ante fallos.
- Type: notify.

## 6. Verificación del servicio

Se utilizaron los siguientes comandos:

bash
sudo systemctl status ssh.socket

Consulta el estado del socket.
sudo ss -tlnp | grep ':22'

Verifica los procesos que escuchan en el puerto 22.
ssh -V

Consulta la versión de OpenSSH.
systemctl cat ssh.socket

Muestra la configuración del socket.
systemctl cat ssh.service

Muestra la configuración del servicio.
sudo sshd -t

Valida la configuración del servidor SSH.

7. Seguridad
- Se utiliza autenticación SSH para la administración remota.
- El acceso remoto se realiza a través de la red privada Tailscale.
- No se deben publicar contraseñas ni claves privadas.
- No se recomienda exponer directamente el puerto 22 a Internet.

8. Resultado
El servidor cuenta con OpenSSH operativo y configurado mediante
systemd, permitiendo su administración remota desde el equipo
principal y otros dispositivos autorizados.

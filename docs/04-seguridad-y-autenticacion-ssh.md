# 04. Seguridad y autenticación SSH

## 1. Objetivo

Implementar mecanismos de autenticación segura para administrar
remotamente el servidor Toshiba HomeLab mediante OpenSSH.

La configuración utiliza autenticación por claves públicas Ed25519,
deshabilita el acceso mediante contraseñas y bloquea el inicio de
sesión SSH directo del usuario root.

## 2. Arquitectura de autenticación

Los dispositivos autorizados se conectan al servidor mediante
una red privada Tailscale.

Dispositivos configurados:

- Portátil Ubuntu: autenticación mediante clave Ed25519.
- Celular Android: autenticación mediante clave Ed25519 desde Termius.
- Servidor Toshiba: validación de claves públicas autorizadas.

Cada dispositivo dispone de su propio par de claves SSH.

## 3. Generación de claves Ed25519

Para generar un par de claves en un equipo Linux:

    ssh-keygen -t ed25519 -C "nombre-del-dispositivo"

Se generan dos archivos:

| Archivo | Descripción |
|---|---|
| id_ed25519 | Clave privada del dispositivo. |
| id_ed25519.pub | Clave pública que se registra en el servidor. |

La clave privada debe permanecer protegida en el dispositivo
que la generó. Nunca debe publicarse ni compartirse.

## 4. Autorización de dispositivos

Las claves públicas autorizadas se almacenan en:

    /home/devadmin/.ssh/authorized_keys

Cada dispositivo autorizado puede tener una entrada independiente.

Para incorporar un equipo Linux se puede utilizar:

    ssh-copy-id -i ~/.ssh/id_ed25519.pub devadmin@SERVIDOR

En este proyecto, el acceso mediante contraseña SSH está
deshabilitado. Por ello, la incorporación de nuevos dispositivos
se realiza desde una sesión administrativa ya autorizada.

## 5. Configuración de seguridad

Se creó el archivo:

    /etc/ssh/sshd_config.d/00-seguridad.conf

Con el siguiente contenido:

    PubkeyAuthentication yes
    PasswordAuthentication no
    PermitRootLogin no

Descripción:

| Parámetro | Configuración | Función |
|---|---|---|
| PubkeyAuthentication | yes | Habilita autenticación mediante claves públicas. |
| PasswordAuthentication | no | Deshabilita autenticación SSH mediante contraseñas. |
| PermitRootLogin | no | Impide el acceso SSH directo como root. |

## 6. Validación de la configuración

Antes de aplicar los cambios se validó la sintaxis:

    sudo sshd -t

Posteriormente se verificaron los parámetros efectivos:

    sudo sshd -T | grep -E 'pubkeyauthentication|passwordauthentication|permitrootlogin'

Resultado:

    permitrootlogin no
    pubkeyauthentication yes
    passwordauthentication no

La configuración se aplicó mediante:

    sudo systemctl reload ssh

## 7. Protección de archivos SSH

Los permisos utilizados para proteger los archivos son:

    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/authorized_keys
    chmod 600 ~/.ssh/id_ed25519
    chmod 644 ~/.ssh/id_ed25519.pub

La clave privada nunca debe almacenarse en el repositorio Git.

## 8. Administración de dispositivos

Cada dispositivo utiliza una clave independiente.

Esto permite:

- Incorporar nuevos equipos sin compartir claves privadas.
- Revocar el acceso de un dispositivo individual.
- Mantener las autorizaciones de los demás equipos.
- Administrar el acceso de manera centralizada.

Para revocar un dispositivo se debe retirar exclusivamente
su entrada de authorized_keys.

## 9. Acceso remoto mediante Tailscale

El servidor utiliza Tailscale para establecer conexiones
privadas entre los dispositivos autorizados.

El acceso SSH se realiza a través de la dirección Tailscale
del servidor, utilizando el puerto 22.

No es necesario exponer directamente el servicio SSH
a Internet mediante redireccionamiento de puertos.

## 10. Estado de implementación

| Característica | Estado |
|---|---|
| OpenSSH instalado y operativo | Implementado |
| Autenticación Ed25519 desde portátil | Verificado |
| Autenticación Ed25519 desde Android | Verificado |
| Autenticación SSH por contraseña | Deshabilitada |
| Acceso SSH directo como root | Deshabilitado |
| Acceso remoto mediante Tailscale | Verificado |
| Validación de configuración SSH | Sin errores |

## 11. Recomendaciones de seguridad

- No compartir claves privadas.
- Utilizar claves independientes por dispositivo.
- Proteger las claves privadas con una frase de contraseña.
- Revocar las claves de dispositivos perdidos o comprometidos.
- Mantener actualizado OpenSSH y Ubuntu Server.
- No publicar direcciones privadas, credenciales ni archivos
  de configuración que contengan secretos.
- Mantener una sesión administrativa abierta al modificar
  la configuración de acceso remoto.

---

**Proyecto:** Toshiba HomeLab - Infraestructura Linux  
**Sistema operativo:** Ubuntu Server 26.04.1 LTS  
**Servicio:** OpenSSH  
**Autenticación:** Ed25519  
**Red privada:** Tailscale

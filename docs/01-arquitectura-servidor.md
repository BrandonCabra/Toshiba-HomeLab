# Arquitectura de infraestructura – Toshiba HomeLab

## 1. Descripción general

Toshiba HomeLab es un laboratorio personal de infraestructura tecnológica construido mediante la reutilización de un portátil Toshiba Satellite C655 como servidor doméstico.

El propósito es implementar, administrar y documentar servicios de infraestructura utilizando tecnologías de código abierto, aplicando conocimientos de Linux, redes, administración de sistemas y conectividad remota.

## 2. Infraestructura física

| Componente | Especificación |
|---|---|
| Fabricante | Toshiba |
| Modelo | Satellite C655 |
| Procesador | Intel Core i3-2350M |
| Memoria RAM | 4 GB físicos aproximadamente; Ubuntu detecta 3.3 GiB |
| Almacenamiento | HDD Toshiba de 320 GB |
| Sistema operativo | Ubuntu Server 26.04.1 LTS |
| Nombre del equipo | toshiba-server |

El equipo funciona como servidor de almacenamiento y administración remota.

## 3. Sistema operativo

Se instaló Ubuntu Server 26.04.1 LTS como sistema operativo principal.

La administración del servidor se realiza mediante terminal, utilizando herramientas nativas de Linux y servicios de red.

Principales componentes:

- systemd: administración de servicios y procesos de inicio.
- OpenSSH: acceso remoto mediante terminal.
- Samba: compartición de archivos.
- Tailscale: conectividad privada entre dispositivos.
- Git: control de versiones.

## 4. Servicios implementados

### 4.1. SSH

Se configuró OpenSSH para permitir la administración remota del servidor desde otros dispositivos.

La conexión se realiza mediante autenticación SSH y permite ejecutar comandos, administrar archivos y controlar los servicios del sistema.

### 4.2. Samba

Se implementó Samba para compartir el directorio de almacenamiento del servidor mediante el protocolo SMB.

Recurso compartido:

- Nombre: Almacenamiento.
- Usuario autorizado: devadmin.
- Directorio: /home/devadmin/servidor.

La autenticación se realiza mediante credenciales propias de Samba.

### 4.3. Tailscale

Se instaló Tailscale en el servidor y en el portátil principal.

Esta herramienta permite establecer conectividad privada entre ambos equipos sin necesidad de exponer directamente los servicios del servidor a Internet mediante redireccionamiento de puertos.

### 4.4. CIFS

Se configuró el cliente CIFS en el portátil Ubuntu para acceder al recurso compartido de Samba.

Punto de montaje:

`/mnt/toshiba`

Se configuró el montaje automático mediante `/etc/fstab` y unidades de systemd.

La conexión utiliza la dirección privada de Tailscale del servidor.

## 5. Arquitectura lógica

```text
                  INTERNET
                     |
              RED PRIVADA
                TAILSCALE
                     |
          +----------+----------+
          |                     |
          |                     |
    PORTÁTIL UBUNTU       TOSHIBA SERVER
      Ubuntu 24.04       Ubuntu Server
          |                26.04.1 LTS
          |                     |
     Cliente CIFS          +----+----+
          |                |         |
      /mnt/toshiba       OpenSSH   Samba
          |                |         |
          +----------------+---------+
                           |
                  /home/devadmin/
                      servidor/
                           |
                 Recurso compartido
                   Almacenamiento
```

## 6. Organización del almacenamiento

El servidor dispone de una estructura de directorios organizada por categorías:

```text
/home/devadmin/servidor/
│
├── datos/
│   ├── documentos/
│   ├── imagenes/
│   ├── videos/
│   ├── musica/
│   ├── instaladores/
│   └── cursos/
│
├── proyectos/
│   ├── desarrollo/
│   ├── bases_datos/
│   └── practica-git/
│
├── respaldos/
│
└── scripts/
```

Esta organización permite separar los datos personales, proyectos de desarrollo, copias de seguridad y herramientas de automatización.

## 7. Seguridad y consideraciones

- El acceso administrativo remoto se realiza mediante SSH.
- La conectividad remota utiliza una red privada de Tailscale.
- Samba requiere autenticación de usuarios.
- Las credenciales CIFS se almacenan fuera del repositorio Git.
- No se publican contraseñas, claves privadas ni tokens.
- Los servicios SMB no deben exponerse directamente a Internet.
- Las direcciones IP privadas y los datos sensibles se omiten de la documentación pública.

## 8. Estado actual del proyecto

| Componente | Estado |
|---|---|
| Ubuntu Server | Implementado |
| SSH | Implementado |
| Samba | Implementado |
| Tailscale | Implementado |
| Cliente CIFS | Implementado |
| Montaje automático | Implementado |
| Git y GitHub | Implementado |
| Automatización Bash | Pendiente |
| Docker | Pendiente |
| Monitorización | Pendiente |

## 9. Próximas mejoras

- Implementar copias de seguridad automatizadas.
- Crear scripts de administración y mantenimiento.
- Configurar Docker y Docker Compose.
- Desplegar aplicaciones web.
- Incorporar monitorización de recursos.
- Documentar procedimientos de recuperación ante fallos.

---

**Proyecto:** Toshiba HomeLab – Linux Server & Infrastructure  
**Enfoque:** Administración Linux, redes e infraestructura DevOps.

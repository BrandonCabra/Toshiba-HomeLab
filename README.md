# Toshiba HomeLab – Linux Server & Infrastructure

Laboratorio personal de infraestructura tecnológica desarrollado a partir de un portátil Toshiba Satellite C655 reutilizado como servidor doméstico.

El proyecto tiene como objetivo aprender y aplicar conceptos de administración de sistemas Linux, redes, almacenamiento remoto, acceso seguro y automatización, documentando cada implementación y sus resultados.

## Objetivos

- Reutilizar hardware antiguo para construir un servidor funcional.
- Administrar servicios y recursos desde Ubuntu Server.
- Implementar acceso remoto entre dispositivos.
- Compartir archivos mediante una red privada.
- Practicar control de versiones con Git y GitHub.
- Documentar procedimientos, configuraciones y aprendizajes técnicos.

## Hardware

| Componente | Características |
|---|---|
| Equipo | Toshiba Satellite C655 |
| Procesador | Intel Core i3-2350M |
| Memoria RAM | Aproximadamente 4 GB detectados por el sistema |
| Almacenamiento | HDD de 320 GB |
| Sistema operativo | Ubuntu Server 26.04.1 LTS |

## Tecnologías implementadas

| Tecnología | Uso |
|---|---|
| Ubuntu Server | Sistema operativo del servidor |
| OpenSSH | Administración remota mediante terminal |
| Samba | Compartición de archivos en red |
| CIFS / cifs-utils | Montaje de carpetas compartidas |
| Tailscale | Conectividad privada entre dispositivos |
| Git | Control de versiones |
| GitHub | Repositorio y documentación del proyecto |
| systemd | Administración de servicios y montaje automático |

## Arquitectura actual

```text
                 INTERNET
                     |
              TAILSCALE VPN
                     |
          +----------+----------+
          |                     |
   PORTÁTIL UBUNTU         TOSHIBA SERVER
   Ubuntu 24.04            Ubuntu Server
          |                26.04.1 LTS
          |                     |
          |                +----+----+
          |                |         |
          |              OpenSSH   Samba
          |                |         |
          +----------------+---------+
                           |
                   /home/devadmin/
                       servidor/
                           |
                  Almacenamiento
                  compartido
                           |
                    CIFS / SMB
                           |
                    /mnt/toshiba
```

## Funcionalidades implementadas

- Instalación y configuración de Ubuntu Server.
- Acceso remoto mediante SSH.
- Administración de usuarios Linux.
- Configuración de carpetas y permisos.
- Compartición de archivos mediante Samba.
- Acceso remoto privado mediante Tailscale.
- Montaje automático de almacenamiento compartido desde otro equipo Linux.
- Sincronización de repositorios mediante Git y GitHub.

## Estructura del repositorio

```text
toshiba-homelab-linux/
├── README.md
├── docs/
├── screenshots/
├── scripts/
└── LICENSE
```

## Seguridad

- Acceso remoto mediante SSH.
- Conectividad privada mediante Tailscale.
- Autenticación de usuarios en Samba.
- Credenciales de montaje almacenadas fuera del repositorio.
- No se publican contraseñas, claves privadas ni tokens.

## Próximas implementaciones

- Automatización de tareas con Bash.
- Copias de seguridad programadas.
- Administración de contenedores con Docker.
- Despliegue de aplicaciones web con Nginx.
- Administración de bases de datos.
- Monitorización de servicios y recursos.
- Integración y despliegue continuo con GitHub Actions.

## Propósito profesional

Este laboratorio forma parte de mi proceso de aprendizaje práctico en desarrollo de software, administración Linux, infraestructura y tecnologías DevOps.

Cada implementación se documenta con el propósito de demostrar conocimientos técnicos, capacidad de resolución de problemas y aprendizaje autónomo.

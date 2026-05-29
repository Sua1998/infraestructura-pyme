# 02. Diseño de la infraestructura

## 1. Resumen del diseño

La infraestructura propuesta se basa en un servidor LAMP documentado sobre Ubuntu Server 22.04. El diseño contempla servicios web, base de datos, acceso remoto seguro, firewall, monitorización y copias de seguridad.

En la sesión 4 se añade un balanceador HAProxy delante del servidor Apache para simular un cambio de alcance solicitado por el cliente.

## 2. Arquitectura lógica

```mermaid
flowchart LR
    U[Usuarios externos] --> H[HAProxy - Balanceador]
    H --> W[Servidor Web Apache + PHP]
    W --> D[(MySQL/MariaDB)]
    W --> M[Netdata / Script monitorización]
    D --> B[Backups mysqldump + rsync]
    A[Administrador] -->|SSH seguro| W
```

## 3. Componentes principales

| Componente | Función | Puerto |
|---|---|---|
| HAProxy | Balanceo de carga HTTP/HTTPS | 80, 443 |
| Apache | Servidor web | 80, 443 |
| PHP | Procesamiento de aplicación web | No aplica |
| MySQL/MariaDB | Base de datos | 3306 |
| SSH | Administración remota | 22 |
| UFW | Firewall local | No aplica |
| Netdata | Monitorización | 19999 |
| rsync | Sincronización de backups | 22 |

## Tabla de versiones de software

| Componente | Versión | Función |
|---|---|---|
| Apache | 2.4.59 | Servidor web HTTP |
| PHP | 8.1 | Lenguaje de servidor |
| MySQL | 8.0 | Base de datos |
| UFW | 0.36 | Firewall básico |
| Netdata | Última estable | Monitorización |
| Certbot | 2.9 | SSL/TLS automático |

## 5. Redes y seguridad

| Elemento | Decisión |
|---|---|
| Acceso SSH | Permitido solo a administradores |
| Tráfico web | Permitido en 80/tcp y 443/tcp |
| Base de datos | No expuesta públicamente |
| Backups | Copias locales y sincronización externa |
| Monitorización | Acceso restringido o protegido por firewall |

## 6. Diseño del balanceador HAProxy

El balanceador se coloca delante de Apache para recibir peticiones HTTP/HTTPS y reenviarlas al servidor web.

Ejemplo documental de configuración:

```haproxy
frontend http_front
    bind *:80
    default_backend apache_back

backend apache_back
    server web01 192.168.1.10:80 check
```

## 7. Justificación

El diseño es adecuado para una PYME porque:

- Usa tecnologías conocidas y documentadas.
- Permite administración remota segura.
- Facilita recuperación mediante backups.
- Incluye monitorización básica.
- Puede evolucionar con balanceo de carga.


## Revisión de diseño inicial

Este diseño inicial define los componentes principales de la infraestructura LAMP.

Elementos incluidos:

- Servidor web Apache.
- PHP.
- Base de datos MySQL/MariaDB.
- SSH para administración.
- UFW como firewall.
- Monitorización.
- Backups.

Esta sección forma parte del trabajo inicial de la rama `feature/plataforma-base`.
# Documentación colaborativa de infraestructura LAMP para PYME

Proyecto de documentación técnica para planificar el despliegue de una infraestructura LAMP con monitorización, copias de seguridad y plan de recuperación.

> Este repositorio **no instala máquinas virtuales ni despliega servicios reales**.  
> El objetivo es documentar el proceso completo en Markdown y practicar Git/GitHub: ramas, commits, Pull Requests, revisiones, conflictos, merge, rebase, issues y releases.

## Integrantes

| Nombre | Rol inicial | Rol tras sesión 3 |
|---|---|---|
| Alumno A | Documentalista de plataforma | Documentalista de operaciones |
| Alumno B | Documentalista de operaciones | Documentalista de plataforma |

## Índice de documentación

| Documento | Descripción |
|---|---|
| [01-analisis.md](docs/01-analisis.md) | Requisitos del cliente y alcance |
| [02-diseno.md](docs/02-diseno.md) | Diseño de la infraestructura, componentes y diagrama |
| [03-planificacion.md](docs/03-planificacion.md) | Planificación por sesiones y tareas |
| [servidor-web.md](docs/04-instalacion/servidor-web.md) | Documentación del servidor Apache/PHP |
| [base-de-datos.md](docs/04-instalacion/base-de-datos.md) | Documentación de MySQL/MariaDB |
| [ssh-firewall.md](docs/04-instalacion/ssh-firewall.md) | Acceso SSH y reglas UFW |
| [monitorizacion.md](docs/04-instalacion/monitorizacion.md) | Monitorización con Netdata o script de comprobación |
| [backups.md](docs/04-instalacion/backups.md) | Copias de seguridad con mysqldump y rsync |
| [05-operacion.md](docs/05-operacion.md) | Guía de operación y mantenimiento |
| [06-recuperacion.md](docs/06-recuperacion.md) | Plan de recuperación ante desastres |
| [REVISION.md](REVISION.md) | Reflexión final del proyecto |
| [CHANGELOG.md](CHANGELOG.md) | Registro de cambios por sesión |
| [tareas.md](tareas.md) | Lista de tareas y estado |
| [GIT-PASO-A-PASO.md](GIT-PASO-A-PASO.md) | Guía práctica para hacer las 4 sesiones en Git |
| [FALLOS-Y-SOLUCIONES-GIT.md](FALLOS-Y-SOLUCIONES-GIT.md) | Fallos simulados y soluciones |

## Estado del proyecto

- [x] Sesión 1: estructura inicial, README y primeras ramas.
- [x] Sesión 2: revisión cruzada, merge y conflicto resuelto.
- [x] Sesión 3: intercambio de roles y conflicto resuelto con rebase.
- [x] Sesión 4: cambio de alcance, issue, integración final y release.

## Convenciones Git

### Ramas

- `main`: rama protegida.
- `feature/plataforma-base`: análisis y diseño inicial.
- `feature/operaciones`: monitorización y backups.
- `feature/actualizar-versiones`: actualización de versiones.
- `feature/nuevas-tecnologias`: conflicto forzado en `02-diseno.md`.
- `feature/servidor-web-y-bd`: servidor web, base de datos y seguridad.
- `feature/guia-operacion`: operación, recuperación y changelog.
- `feature/balanceador-diseno`: cambios de diseño por HAProxy.
- `feature/balanceador-config`: configuración y mantenimiento de HAProxy.

### Mensajes de commit

Ejemplo:

```bash
git commit -m "Documenta política de backups y rotación"
```

Los mensajes deben ser claros, breves y explicar qué se ha cambiado.

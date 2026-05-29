# 01. Análisis de requisitos

## 1. Contexto

La empresa cliente es una PYME que necesita documentar la planificación de una infraestructura web básica. La solución debe permitir publicar una web corporativa, almacenar datos internos y mantener la infraestructura de forma segura.

Este documento recoge requisitos, alcance, restricciones y criterios de aceptación.

## 2. Objetivos del cliente

- Disponer de una presencia web corporativa basada en Apache y PHP.
- Contar con una base de datos MySQL/MariaDB para la web.
- Contar con una segunda base de datos para gestión interna.
- Permitir administración remota segura mediante SSH.
- Proteger el servidor mediante firewall UFW.
- Definir monitorización básica.
- Diseñar copias de seguridad automáticas.
- Documentar operación diaria y recuperación ante fallos.

## 3. Alcance

### Incluido

- Diseño lógico de una infraestructura LAMP.
- Documentación de instalación y configuración.
- Documentación de monitorización.
- Estrategia de backups.
- Guía de operación.
- Plan de recuperación.
- Gestión colaborativa mediante GitHub.

### No incluido

- Creación real de máquinas virtuales.
- Instalación real de paquetes.
- Configuración real de dominios o DNS.
- Puesta en producción real.
- Gestión avanzada de alta disponibilidad, salvo la documentación del balanceador HAProxy añadida en sesión 4.

## 4. Requisitos funcionales

| Código | Requisito | Prioridad |
|---|---|---|
| RF-01 | Documentar servidor Apache con PHP | Alta |
| RF-02 | Documentar base de datos para web | Alta |
| RF-03 | Documentar base de datos para gestión interna | Alta |
| RF-04 | Documentar acceso SSH seguro | Alta |
| RF-05 | Documentar firewall UFW | Alta |
| RF-06 | Documentar monitorización básica | Media |
| RF-07 | Documentar copias de seguridad | Alta |
| RF-08 | Documentar recuperación ante desastres | Alta |
| RF-09 | Añadir balanceador HAProxy al diseño | Media |

## 5. Requisitos no funcionales

| Código | Requisito | Descripción |
|---|---|---|
| RNF-01 | Seguridad | Reducir superficie de ataque y limitar SSH |
| RNF-02 | Mantenibilidad | Documentación clara y ordenada |
| RNF-03 | Trazabilidad | Cambios registrados mediante commits y PR |
| RNF-04 | Disponibilidad | Recuperación mediante backups |
| RNF-05 | Escalabilidad | Posibilidad futura de añadir balanceador |

## 6. Roles

| Rol | Responsabilidades |
|---|---|
| Documentalista de plataforma | Infraestructura base, diseño, servidor web, base de datos y seguridad |
| Documentalista de operaciones | Monitorización, backups, operación, recuperación y changelog |

En la sesión 3 los roles se intercambian para que ambos integrantes trabajen sobre documentos del compañero.

## 7. Riesgos iniciales

| Riesgo | Impacto | Mitigación |
|---|---|---|
| Comandos mal documentados | Alto | Revisión cruzada en PR |
| Conflictos Git | Medio | Pull frecuente y resolución guiada |
| Documentos incompletos | Alto | Checklist en `tareas.md` |
| Falta de coherencia entre archivos | Medio | Revisión final de enlaces y términos |

## 8. Criterios de aceptación

El proyecto se considera válido si:

- Todos los documentos están escritos en Markdown.
- Existe estructura organizada en carpetas.
- Hay historial Git con ramas y commits significativos.
- Hay Pull Requests revisados.
- Hay al menos dos conflictos resueltos.
- Se usa merge y rebase.
- Existe un release `v1.0`.
- Existe reflexión final en `REVISION.md`.



## Revisión de sesión 1

Este documento se trabaja desde la rama `feature/plataforma-base`.

Cambios realizados:

- Se revisan los requisitos principales del cliente.
- Se define el alcance del proyecto.
- Se aclara que no se crearán máquinas virtuales reales.
- Se identifican requisitos funcionales y no funcionales.
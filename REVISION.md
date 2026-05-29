# Revisión final del proyecto

## Objetivo

Este documento recoge la reflexión final sobre el trabajo realizado durante el proyecto de documentación colaborativa de una infraestructura LAMP con monitorización.

El proyecto se ha realizado usando Markdown, Git y GitHub, simulando el trabajo colaborativo de un equipo técnico.

## Conflictos encontrados

Durante el proyecto se han provocado conflictos para practicar su resolución con Git.

### Conflicto en `02-diseno.md`

El primer conflicto se produjo en la tabla de versiones de software del archivo `docs/02-diseno.md`.

Una rama modificó la versión de Apache a `2.4.60` y otra rama modificó la misma zona añadiendo Certbot y usando otra versión de Apache.

La solución final fue mantener la versión `2.4.60` de Apache y añadir Certbot como nueva tecnología:

```markdown
| Apache | 2.4.60 | Servidor web HTTP actualizado |
| Certbot | 2.9 | SSL/TLS automático |
```

### Conflicto o actualización con rebase en `ssh-firewall.md`

En la sesión 3 se practicó el uso de:

```bash
git pull --rebase origin main
```

En este caso, Git pudo integrar los cambios automáticamente, por lo que no fue necesario resolver marcadores manuales de conflicto.

Aun así, el proceso sirvió para practicar la actualización de una rama de trabajo con los últimos cambios de `main`.

## Comandos Git utilizados

Durante el proyecto se utilizaron los siguientes comandos:

```bash
git status
git branch
git checkout
git checkout -b
git add
git commit
git push
git pull
git pull --rebase
git push --force-with-lease
```

## Ramas utilizadas

Las ramas principales de trabajo fueron:

| Rama | Uso |
|---|---|
| `feature/plataforma-base` | Documentación inicial de análisis y diseño |
| `feature/operaciones` | Monitorización y backups |
| `feature/actualizar-versiones` | Actualización de versión de Apache |
| `feature/nuevas-tecnologias` | Añadir Certbot y resolver conflicto |
| `feature/guia-operacion` | Operación, recuperación y CHANGELOG |
| `feature/servidor-web-y-bd` | Cambios de SSH y firewall desde plataforma |
| `feature/balanceador-config` | Documentación de HAProxy desde operaciones |

## Pull Requests

Se han usado Pull Requests para integrar los cambios en la rama `main`.

Como el proyecto se ha realizado desde una sola cuenta de GitHub, las revisiones se han simulado mediante comentarios en los Pull Requests.

## Trabajo con Markdown

Los documentos se han redactado usando Markdown con:

- Títulos y subtítulos.
- Listas ordenadas y desordenadas.
- Tablas.
- Bloques de código.
- Rutas de archivos.
- Explicaciones técnicas.

## Intercambio de roles

El intercambio de roles permitió que ambos alumnos trabajaran en distintas partes del proyecto.

El Alumno A trabajó principalmente la parte de plataforma, análisis, diseño y seguridad.

El Alumno B trabajó principalmente la parte de operaciones, monitorización, backups, mantenimiento, recuperación y HAProxy.

## Qué se haría diferente

En un proyecto real se podría mejorar:

- Usando dos cuentas reales de GitHub.
- Activando la protección de `main` con revisión obligatoria.
- Creando issues para cada tarea.
- Haciendo commits más pequeños.
- Revisando los documentos antes de cada merge.
- Probando todos los enlaces internos del README.
- Documentando cada incidencia en un apartado específico.

## Conclusión

El proyecto ha permitido practicar documentación técnica con Markdown, control de versiones con Git, uso de ramas, Pull Requests, resolución de conflictos y actualización de ramas mediante rebase.

También ha servido para entender cómo se organiza la documentación profesional de una infraestructura LAMP con monitorización, copias de seguridad, operación y recuperación ante desastres.
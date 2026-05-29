# Guía paso a paso para hacer las 4 sesiones

Esta guía te dice qué hacer en GitHub y en Git para cumplir el documento.  
No hay que crear máquinas virtuales. Solo hay que crear documentos Markdown y practicar el flujo Git.

---

# Antes de empezar

## 1. Crear repositorio

En GitHub:

1. New repository.
2. Nombre: `infraestructura-pyme`.
3. Público o privado, según indique el profesor.
4. Añade al compañero como colaborador:
   - Settings.
   - Collaborators.
   - Add people.

## 2. Proteger `main`

En GitHub:

1. Settings.
2. Branches.
3. Add branch protection rule.
4. Branch name pattern: `main`.
5. Activar:
   - Require a pull request before merging.
   - Require approvals: 1.
6. Guardar.

## 3. Clonar repositorio

```bash
git clone URL_DEL_REPOSITORIO
cd infraestructura-pyme
```

---

# Sesión 1: repositorio, estructura y primeras ramas

## Paso 1. Crear estructura

Crea estos archivos y carpetas:

```bash
mkdir -p docs/04-instalacion
touch README.md CHANGELOG.md tareas.md
touch docs/01-analisis.md docs/02-diseno.md docs/03-planificacion.md
touch docs/04-instalacion/servidor-web.md
touch docs/04-instalacion/base-de-datos.md
touch docs/04-instalacion/ssh-firewall.md
touch docs/04-instalacion/monitorizacion.md
touch docs/04-instalacion/backups.md
touch docs/05-operacion.md docs/06-recuperacion.md
```

## Paso 2. Primer commit en `main`

```bash
git add .
git commit -m "Estructura inicial del proyecto de documentación LAMP"
git push origin main
```

## Posible fallo

### Fallo

```text
fatal: not a git repository
```

### Solución

Estás fuera de la carpeta del repo.

```bash
cd infraestructura-pyme
git status
```

## Paso 3. README inicial

Edita `README.md` con título, integrantes e índice.

```bash
git add README.md
git commit -m "Añade README inicial del proyecto"
git push origin main
```

## Paso 4. El compañero sincroniza

```bash
git pull origin main
```

## Paso 5. Rama del documentalista de plataforma

```bash
git checkout -b feature/plataforma-base main
```

Edita:

- `docs/01-analisis.md`
- `docs/02-diseno.md`

Luego:

```bash
git add docs/01-analisis.md docs/02-diseno.md
git commit -m "Borrador inicial de análisis y diseño de infraestructura"
git push -u origin feature/plataforma-base
```

## Paso 6. Rama del documentalista de operaciones

```bash
git checkout main
git pull origin main
git checkout -b feature/operaciones main
```

Edita:

- `docs/04-instalacion/monitorizacion.md`
- `docs/04-instalacion/backups.md`

Luego:

```bash
git add docs/04-instalacion/monitorizacion.md docs/04-instalacion/backups.md
git commit -m "Primera versión de monitorización y política de backups"
git push -u origin feature/operaciones
```

## Paso 7. Abrir Pull Requests

En GitHub:

1. Ir a Pull Requests.
2. New Pull Request.
3. Base: `main`.
4. Compare: tu rama.
5. Crear PR.
6. Asignar al compañero como reviewer.

No hagas merge todavía.

---

# Sesión 2: revisión, merge y conflicto

## Paso 1. Revisar PR del compañero

En GitHub:

1. Abre el PR del compañero.
2. Revisa archivos cambiados.
3. Añade comentarios.
4. Aprueba o solicita cambios.

Cosas a revisar:

- Markdown correcto.
- Tablas bien formateadas.
- Comandos dentro de bloques de código.
- Coherencia técnica.

## Paso 2. Aplicar feedback

Cada uno vuelve a su rama:

```bash
git checkout feature/plataforma-base
```

O:

```bash
git checkout feature/operaciones
```

Edita según los comentarios.

```bash
git add .
git commit -m "Correcciones tras revisión del compañero"
git push origin NOMBRE_DE_LA_RAMA
```

## Paso 3. Aprobar y mergear PR

En GitHub:

1. El compañero aprueba.
2. El autor del PR pulsa Merge Pull Request.
3. Confirm merge.

Después, en local:

```bash
git checkout main
git pull origin main
```

## Paso 4. Crear conflicto en `02-diseno.md`

### Miembro A

```bash
git checkout main
git pull origin main
git checkout -b feature/actualizar-versiones main
```

En `docs/02-diseno.md`, cambia Apache a:

```markdown
| Apache | 2.4.60 | Servidor web |
```

Commit y push:

```bash
git add docs/02-diseno.md
git commit -m "Actualiza versión de Apache a 2.4.60"
git push -u origin feature/actualizar-versiones
```

Abre PR, el compañero aprueba y se mergea.

### Miembro B

```bash
git checkout main
git pull origin main
git checkout -b feature/nuevas-tecnologias main
```

En la misma tabla cambia Apache a:

```markdown
| Apache | 2.4.59 | Servidor web |
| Certbot | 2.9 | SSL/TLS automático |
```

Commit y push:

```bash
git add docs/02-diseno.md
git commit -m "Añade Certbot y actualiza tabla de tecnologías"
git push -u origin feature/nuevas-tecnologias
```

Abre PR. GitHub mostrará conflicto.

## Paso 5. Resolver conflicto con merge

Miembro B:

```bash
git checkout feature/nuevas-tecnologias
git pull origin main
```

Git marcará conflicto en `docs/02-diseno.md`.

Abre el archivo. Verás algo parecido a:

```text
<<<<<<< HEAD
| Apache | 2.4.59 | Servidor web |
| Certbot | 2.9 | SSL/TLS automático |
=======
| Apache | 2.4.60 | Servidor web |
>>>>>>> main
```

Déjalo así:

```markdown
| Apache | 2.4.60 | Servidor web, versión actualizada |
| Certbot | 2.9 | SSL/TLS automático añadido |
```

Termina:

```bash
git add docs/02-diseno.md
git commit -m "Resuelto conflicto en tabla de versiones. Se integra Apache 2.4.60 y Certbot"
git push origin feature/nuevas-tecnologias
```

Ahora el PR se puede mergear.

---

# Sesión 3: intercambio de roles y rebase

## Paso 1. Actualizar `main`

Ambos:

```bash
git checkout main
git pull origin main
```

## Paso 2. Intercambiar roles

El que hizo plataforma pasa a operaciones.  
El que hizo operaciones pasa a plataforma.

## Paso 3. Rama de servidor web y base de datos

```bash
git checkout -b feature/servidor-web-y-bd main
```

Edita:

- `docs/04-instalacion/servidor-web.md`
- `docs/04-instalacion/base-de-datos.md`
- `docs/04-instalacion/ssh-firewall.md`

Commit:

```bash
git add docs/04-instalacion/servidor-web.md docs/04-instalacion/base-de-datos.md docs/04-instalacion/ssh-firewall.md
git commit -m "Documenta servidor web, base de datos y seguridad inicial"
git push -u origin feature/servidor-web-y-bd
```

## Paso 4. Rama de operación

```bash
git checkout main
git pull origin main
git checkout -b feature/guia-operacion main
```

Edita:

- `docs/05-operacion.md`
- `docs/06-recuperacion.md`
- `CHANGELOG.md`

Commit:

```bash
git add docs/05-operacion.md docs/06-recuperacion.md CHANGELOG.md
git commit -m "Añade guía de operación, recuperación y changelog"
git push -u origin feature/guia-operacion
```

## Paso 5. Provocar conflicto en `ssh-firewall.md`

### Miembro A

En `feature/servidor-web-y-bd`, añade:

```markdown
## Reglas UFW

- Permitir SSH solo desde IP de la oficina: `ufw allow from 192.168.1.0/24 to any port 22`
- Permitir tráfico web: `ufw allow 80/tcp` y `ufw allow 443/tcp`
```

Commit y push:

```bash
git add docs/04-instalacion/ssh-firewall.md
git commit -m "Añade reglas UFW restringidas por red de oficina"
git push origin feature/servidor-web-y-bd
```

Abre PR, se aprueba y se mergea.

### Miembro B

En `feature/guia-operacion`, añade en la misma zona:

```markdown
## Configuración de firewall con UFW

- `ufw default deny incoming`
- `ufw allow 22/tcp`
- `ufw allow 80,443/tcp`
- `ufw enable`
```

Commit y push:

```bash
git add docs/04-instalacion/ssh-firewall.md
git commit -m "Añade configuración general de firewall UFW"
git push origin feature/guia-operacion
```

Abre PR. GitHub mostrará conflicto.

## Paso 6. Resolver conflicto con rebase

Miembro B:

```bash
git checkout feature/guia-operacion
git pull --rebase origin main
```

Git mostrará conflicto en:

```text
docs/04-instalacion/ssh-firewall.md
```

Edita el archivo y combina las dos versiones:

```markdown
## Reglas UFW

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow from 192.168.1.0/24 to any port 22 proto tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```
```

Termina el rebase:

```bash
git add docs/04-instalacion/ssh-firewall.md
git rebase --continue
git push --force-with-lease origin feature/guia-operacion
```

Luego se mergea el PR.

---

# Sesión 4: cambio de alcance, issue y release

## Paso 1. Crear issue

En GitHub:

1. Ir a Issues.
2. New issue.
3. Título:

```text
Añadir balanceador HAProxy a la infraestructura
```

4. Descripción:

```markdown
El cliente solicita añadir un balanceador HAProxy delante del servidor Apache.
Hay que actualizar diseño, planificación, servidor web y operación.
```

## Paso 2. Rama de diseño

```bash
git checkout main
git pull origin main
git checkout -b feature/balanceador-diseno main
```

Edita:

- `docs/02-diseno.md`
- `docs/03-planificacion.md`

Commit:

```bash
git add docs/02-diseno.md docs/03-planificacion.md
git commit -m "Añade HAProxy al diseño y planificación"
git push -u origin feature/balanceador-diseno
```

Abre PR, revisión y merge.

## Paso 3. Rama de configuración

```bash
git checkout main
git pull origin main
git checkout -b feature/balanceador-config main
```

Edita:

- `docs/04-instalacion/servidor-web.md`
- `docs/05-operacion.md`

Antes de push, actualiza con rebase por si cambió `main`:

```bash
git pull --rebase origin main
```

Commit:

```bash
git add docs/04-instalacion/servidor-web.md docs/05-operacion.md
git commit -m "Documenta configuración y operación de HAProxy"
git push -u origin feature/balanceador-config
```

Abre PR, revisión y merge.

## Paso 4. Pulido final

En `main`:

```bash
git checkout main
git pull origin main
```

Revisa:

- README.
- Enlaces internos.
- CHANGELOG.
- Tareas.
- REVISION.

Commit final si hace falta:

```bash
git add .
git commit -m "Pulido final de documentación y enlaces"
git push origin main
```

## Paso 5. Crear release

En GitHub:

1. Releases.
2. Create a new release.
3. Tag: `v1.0`.
4. Title: `Entrega final documentación LAMP`.
5. Description:

```markdown
Entrega final del proyecto de documentación colaborativa.
Incluye sesiones 1 a 4, conflictos resueltos, HAProxy y reflexión final.
```

6. Publish release.

---

# Checklist final de entrega

- [ ] Repositorio creado.
- [ ] `main` protegida.
- [ ] Estructura Markdown completa.
- [ ] README completo.
- [ ] PR de sesión 1 abiertos.
- [ ] PR revisados en sesión 2.
- [ ] Conflicto en `02-diseno.md` resuelto.
- [ ] Intercambio de roles hecho.
- [ ] Conflicto en `ssh-firewall.md` resuelto con rebase.
- [ ] Issue de HAProxy creado.
- [ ] Release `v1.0` creado.
- [ ] `REVISION.md` redactado.

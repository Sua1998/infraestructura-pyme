# Fallos y soluciones Git

Este documento simula fallos que pueden aparecer durante las 4 sesiones y cómo resolverlos.

## 1. Error: no estás en la rama correcta

### Situación

Editas un archivo y luego ves que estás en `main` en vez de en tu rama.

```bash
git branch
```

### Solución

Si no has hecho commit:

```bash
git checkout -b feature/nombre-correcto
git add .
git commit -m "Añade cambios en la rama correcta"
git push -u origin feature/nombre-correcto
```

## 2. Error: rechazo al hacer push

### Mensaje típico

```text
rejected because the remote contains work that you do not have locally
```

### Causa

Tu rama local está desactualizada.

### Solución

```bash
git pull origin main
git push origin nombre-rama
```

Si estás trabajando con rebase:

```bash
git pull --rebase origin main
git push origin nombre-rama
```

## 3. Error: conflicto en `02-diseno.md`

### Mensaje típico

```text
CONFLICT (content): Merge conflict in docs/02-diseno.md
Automatic merge failed; fix conflicts and then commit the result.
```

### Solución

1. Abrir el archivo.
2. Buscar marcadores:

```text
<<<<<<< HEAD
=======
>>>>>>> rama
```

3. Dejar la versión final correcta.
4. Guardar.
5. Ejecutar:

```bash
git add docs/02-diseno.md
git commit -m "Resuelto conflicto en tabla de versiones"
git push origin feature/nuevas-tecnologias
```

## 4. Error: conflicto durante rebase

### Mensaje típico

```text
CONFLICT (content): Merge conflict in docs/04-instalacion/ssh-firewall.md
error: could not apply ...
```

### Solución

```bash
# Editar el archivo y resolver conflicto
git add docs/04-instalacion/ssh-firewall.md
git rebase --continue
git push --force-with-lease origin feature/guia-operacion
```

## 5. Error: me equivoqué en el mensaje del último commit

### Solución

```bash
git commit --amend -m "Nuevo mensaje correcto"
git push --force-with-lease
```

Solo usar si el commit ya estaba subido y sabes que no perjudicas al compañero.

## 6. Error: hice cambios pero Git no los detecta

### Comprobación

```bash
git status
```

Puede que hayas editado otro archivo, no hayas guardado o estés en otra carpeta.

### Solución

```bash
pwd
ls
git status
```

## 7. Error: quiero cancelar cambios sin commit

### Para un archivo

```bash
git restore docs/02-diseno.md
```

### Para todo

```bash
git restore .
```

Cuidado: esto borra cambios locales no guardados en commit.

## 8. Error: hice commit en `main` por accidente

### Opción segura

Crear una rama desde ese commit:

```bash
git checkout -b feature/arreglo-desde-main
git push -u origin feature/arreglo-desde-main
```

Luego volver `main` al remoto:

```bash
git checkout main
git reset --hard origin/main
```

## 9. Error: el PR dice que tiene conflictos

### Solución general

```bash
git checkout mi-rama
git pull origin main
# resolver conflictos
git add .
git commit -m "Resuelve conflictos con main"
git push origin mi-rama
```

Si el profesor pide rebase:

```bash
git checkout mi-rama
git pull --rebase origin main
# resolver conflictos
git add .
git rebase --continue
git push --force-with-lease origin mi-rama
```

## 10. Error: olvidé hacer pull antes de trabajar

### Solución

```bash
git checkout main
git pull origin main
git checkout mi-rama
git merge main
```

O con rebase:

```bash
git checkout mi-rama
git pull --rebase origin main
```

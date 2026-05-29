# Revisión final del proyecto

## 1. Conflictos encontrados

### Conflicto 1: `docs/02-diseno.md`

Durante la sesión 2, dos ramas modificaron la misma tabla de versiones:

- Rama `feature/actualizar-versiones`: cambió Apache a `2.4.60`.
- Rama `feature/nuevas-tecnologias`: cambió Apache a `2.4.59` y añadió Certbot.

Git no pudo decidir automáticamente qué cambio conservar.

### Solución

Se resolvió manualmente manteniendo:

- Apache `2.4.60`.
- Certbot `2.9`.

Commit recomendado:

```bash
git commit -m "Resuelto conflicto en tabla de versiones. Se integra Apache 2.4.60 y Certbot"
```

## 2. Conflicto 2: `docs/04-instalacion/ssh-firewall.md`

Durante la sesión 3, ambos integrantes modificaron la misma sección de reglas UFW.

- Una versión limitaba SSH a la red de oficina.
- Otra versión añadía políticas por defecto y reglas web.

### Solución

Se resolvió con rebase combinando ambas propuestas:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow from 192.168.1.0/24 to any port 22 proto tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

## 3. Comandos Git más usados

```bash
git clone
git checkout -b
git status
git add .
git commit -m
git push
git pull
git pull --rebase
git merge
git rebase --continue
git push --force-with-lease
```

## 4. Qué haríamos diferente

- Hacer `git pull origin main` antes de crear cada rama.
- Crear issues más detallados.
- Escribir commits más pequeños.
- Revisar enlaces Markdown en cada Pull Request.
- Acordar previamente qué secciones editará cada persona para evitar conflictos innecesarios.

## 5. Funcionamiento del intercambio de roles

El intercambio de roles fue útil porque obligó a cada integrante a revisar y mejorar documentos del compañero. Esto ayudó a detectar errores, mejorar la coherencia y entender todo el proyecto, no solo una parte.

## 6. Conclusión

El proyecto cumple el objetivo: se ha generado documentación técnica completa en Markdown y se ha practicado trabajo colaborativo real con GitHub, incluyendo Pull Requests, revisiones, merge, rebase, conflictos, issues y release.

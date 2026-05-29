# Copias de seguridad

## 1. Objetivo

Documentar una estrategia de copias de seguridad automáticas para bases de datos y archivos relevantes.

## 2. Elementos a respaldar

| Elemento | Ruta o comando |
|---|---|
| Web | `/var/www/empresa` |
| Configuración Apache | `/etc/apache2` |
| Base de datos web | `web_empresa` |
| Base de datos interna | `gestion_interna` |
| Configuración UFW/SSH | `/etc/ssh`, reglas documentadas |

## 3. Backup de bases de datos

```bash
mysqldump -u root -p web_empresa > /backup/mysql/web_empresa_$(date +%F).sql
mysqldump -u root -p gestion_interna > /backup/mysql/gestion_interna_$(date +%F).sql
```

## 4. Backup de archivos con rsync

```bash
rsync -avz /var/www/empresa/ /backup/www/empresa/
rsync -avz /etc/apache2/ /backup/config/apache2/
```

## 5. Script documental de backup

```bash
#!/bin/bash

FECHA=$(date +%F)
DESTINO="/backup/$FECHA"

mkdir -p "$DESTINO/mysql"
mkdir -p "$DESTINO/www"
mkdir -p "$DESTINO/config"

mysqldump -u root -p web_empresa > "$DESTINO/mysql/web_empresa.sql"
mysqldump -u root -p gestion_interna > "$DESTINO/mysql/gestion_interna.sql"

rsync -avz /var/www/empresa/ "$DESTINO/www/empresa/"
rsync -avz /etc/apache2/ "$DESTINO/config/apache2/"
```

## 6. Programación con cron

```bash
0 2 * * * /usr/local/bin/backup_pyme.sh >> /var/log/backup_pyme.log 2>&1
```

## 7. Rotación

Mantener:

| Tipo | Retención |
|---|---|
| Diarios | 7 días |
| Semanales | 4 semanas |
| Mensuales | 6 meses |

Ejemplo:

```bash
find /backup -type d -mtime +30 -exec rm -rf {} \;
```

## 8. Comprobación de backups

Una copia no se considera válida hasta que se ha probado su restauración.

Checklist:

- [ ] Existe archivo SQL.
- [ ] El archivo no está vacío.
- [ ] Se puede restaurar en entorno de prueba.
- [ ] Los archivos web están completos.
- [ ] El log de backup no muestra errores.

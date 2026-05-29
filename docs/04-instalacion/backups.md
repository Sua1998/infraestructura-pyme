# Copias de seguridad

## Objetivo

Las copias de seguridad permiten recuperar la información en caso de pérdida de datos, error humano, fallo del servidor o ataque.

En esta infraestructura se propone una estrategia basada en:

- Copias de seguridad de bases de datos con `mysqldump`.
- Copias de archivos web con `rsync`.
- Rotación de copias antiguas.
- Almacenamiento en una ubicación externa o servidor secundario.

## Elementos que se deben copiar

| Elemento | Ruta o recurso | Motivo |
|---|---|---|
| Archivos web | `/var/www/html` | Contiene la aplicación web |
| Configuración de Apache | `/etc/apache2` | Permite recuperar la configuración del servidor web |
| Bases de datos | MySQL/MariaDB | Contienen datos de la web y gestión interna |
| Scripts de mantenimiento | `/opt/scripts` | Automatizan tareas importantes |
| Documentación | Repositorio GitHub | Contiene la documentación técnica del proyecto |

## Estructura propuesta de backups

Se propone guardar las copias en:

```text
/backups/
├── diario/
├── semanal/
└── mensual/
```

## Copia de seguridad de la base de datos

Ejemplo de copia de una base de datos:

```bash
mysqldump -u usuario_backup -p nombre_base_datos > /backups/diario/bd_web.sql
```

Ejemplo con fecha automática:

```bash
mysqldump -u usuario_backup -p bd_web > /backups/diario/bd_web_$(date +%F).sql
```

## Copia de seguridad de archivos web

Ejemplo usando `rsync`:

```bash
rsync -av /var/www/html/ /backups/diario/web/
```

Copia hacia un servidor externo:

```bash
rsync -av /var/www/html/ usuario@servidor-backup:/backups/web/
```

## Script básico de backup

Ejemplo de script documentado:

```bash
#!/bin/bash

FECHA=$(date +%F)
DESTINO="/backups/diario"

mkdir -p "$DESTINO"

mysqldump -u usuario_backup -p bd_web > "$DESTINO/bd_web_$FECHA.sql"
rsync -av /var/www/html/ "$DESTINO/web_$FECHA/"

echo "Backup completado: $FECHA"
```

El script podría guardarse como:

```text
/opt/scripts/backup_diario.sh
```

## Programación con cron

Para ejecutar el backup todos los días a las 02:00:

```bash
0 2 * * * /opt/scripts/backup_diario.sh
```

## Política de rotación

| Tipo de copia | Frecuencia | Conservación |
|---|---|---|
| Diaria | Todos los días | 7 días |
| Semanal | Cada domingo | 4 semanas |
| Mensual | Primer día del mes | 6 meses |

## Comprobación de backups

No basta con crear backups. También hay que comprobar que se pueden restaurar.

Acciones recomendadas:

- Verificar que los archivos se crean correctamente.
- Comprobar el tamaño de los backups.
- Revisar logs de ejecución.
- Hacer pruebas de restauración periódicas.
- Guardar una copia fuera del servidor principal.

## Restauración básica de base de datos

Ejemplo de restauración:

```bash
mysql -u usuario_backup -p bd_web < /backups/diario/bd_web_2026-05-29.sql
```

## Buenas prácticas

- Automatizar las copias de seguridad.
- No guardar todas las copias en el mismo servidor.
- Proteger los backups con permisos adecuados.
- Cifrar copias sensibles si se almacenan fuera.
- Revisar periódicamente que las copias funcionan.
- Documentar fecha, responsable y resultado de cada prueba de restauración.
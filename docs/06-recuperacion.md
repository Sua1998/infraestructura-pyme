# Plan de recuperación ante desastres

## Objetivo

Este documento describe el procedimiento para recuperar la infraestructura en caso de fallo grave, pérdida de datos o caída de servicios.

El objetivo es reducir el tiempo de inactividad y garantizar que la empresa pueda volver a operar lo antes posible.

## Escenarios de desastre

| Escenario | Impacto | Prioridad |
|---|---|---|
| Caída de Apache | La web no está disponible | Alta |
| Fallo de MySQL/MariaDB | La aplicación no puede acceder a datos | Alta |
| Pérdida de archivos web | La aplicación queda incompleta | Alta |
| Eliminación accidental de datos | Pérdida de información | Alta |
| Fallo de disco | Pérdida parcial o total del sistema | Crítica |
| Bloqueo por firewall | Pérdida de acceso remoto | Media/Alta |

## Servicios críticos

Los servicios que deben recuperarse primero son:

1. SSH.
2. Apache.
3. MySQL/MariaDB.
4. UFW.
5. Netdata.
6. Sistema de backups.

## Orden recomendado de recuperación

1. Comprobar si el servidor responde.
2. Verificar acceso por SSH.
3. Revisar estado de servicios críticos.
4. Consultar logs.
5. Restaurar archivos web si es necesario.
6. Restaurar base de datos si es necesario.
7. Verificar funcionamiento de la aplicación.
8. Documentar la incidencia.

## Comprobación inicial

Comprobar conectividad:

```bash
ping IP_DEL_SERVIDOR
```

Conectarse por SSH:

```bash
ssh usuario@IP_DEL_SERVIDOR
```

Comprobar servicios:

```bash
sudo systemctl status apache2
sudo systemctl status mysql
sudo systemctl status ssh
```

## Recuperación de Apache

Iniciar Apache:

```bash
sudo systemctl start apache2
```

Reiniciar Apache:

```bash
sudo systemctl restart apache2
```

Comprobar configuración:

```bash
sudo apache2ctl configtest
```

Revisar logs:

```bash
sudo tail -f /var/log/apache2/error.log
```

## Recuperación de MySQL/MariaDB

Comprobar servicio:

```bash
sudo systemctl status mysql
```

Iniciar servicio:

```bash
sudo systemctl start mysql
```

Restaurar una base de datos desde backup:

```bash
mysql -u usuario_backup -p bd_web < /backups/diario/bd_web_2026-05-29.sql
```

Comprobar bases de datos disponibles:

```bash
mysql -u usuario_backup -p -e "SHOW DATABASES;"
```

## Recuperación de archivos web

Restaurar archivos desde copia local:

```bash
rsync -av /backups/diario/web/ /var/www/html/
```

Restaurar archivos desde servidor externo:

```bash
rsync -av usuario@servidor-backup:/backups/web/ /var/www/html/
```

Aplicar permisos recomendados:

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

## Recuperación de acceso SSH

Comprobar servicio SSH:

```bash
sudo systemctl status ssh
```

Comprobar reglas UFW:

```bash
sudo ufw status verbose
```

Permitir acceso SSH desde la red de oficina:

```bash
sudo ufw allow from 192.168.1.0/24 to any port 22
```

## Recuperación del firewall

Listar reglas numeradas:

```bash
sudo ufw status numbered
```

Eliminar una regla incorrecta:

```bash
sudo ufw delete NUMERO_REGLA
```

Permitir tráfico web:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

## Verificación final

Después de recuperar el servicio, se debe comprobar:

- La web carga correctamente.
- Apache está activo.
- MySQL/MariaDB está activo.
- SSH funciona.
- El firewall mantiene las reglas necesarias.
- Netdata muestra métricas.
- Los backups siguen programados.
- No hay errores críticos en logs.

## Registro de recuperación

| Campo | Información |
|---|---|
| Fecha | Día y hora de la incidencia |
| Servicio afectado | Apache, MySQL, SSH, etc. |
| Descripción | Qué ha ocurrido |
| Causa | Motivo detectado |
| Solución | Acciones realizadas |
| Responsable | Persona que interviene |
| Estado final | Resuelto o pendiente |

## Buenas prácticas

- Probar restauraciones periódicamente.
- Mantener copias fuera del servidor principal.
- Documentar todos los cambios.
- No borrar backups antiguos sin verificar los nuevos.
- Mantener una copia de la documentación en GitHub.
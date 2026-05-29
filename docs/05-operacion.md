# Guía de operación y mantenimiento

## Objetivo

Este documento describe las tareas necesarias para mantener en funcionamiento la infraestructura LAMP de la PYME.

La guía está pensada para el personal técnico encargado de revisar el estado del servidor, comprobar servicios, validar copias de seguridad y actuar ante incidencias básicas.

## Servicios principales

| Servicio | Función | Comando de revisión |
|---|---|---|
| Apache | Servidor web | `sudo systemctl status apache2` |
| MySQL/MariaDB | Base de datos | `sudo systemctl status mysql` |
| SSH | Acceso remoto seguro | `sudo systemctl status ssh` |
| UFW | Firewall básico | `sudo ufw status verbose` |
| Netdata | Monitorización | `sudo systemctl status netdata` |

## Tareas diarias

| Tarea | Descripción | Responsable |
|---|---|---|
| Revisar Apache | Comprobar que el servidor web está activo | Sistemas |
| Revisar MySQL | Comprobar que la base de datos responde | Sistemas |
| Revisar espacio en disco | Detectar falta de almacenamiento | Sistemas |
| Revisar memoria | Comprobar consumo de RAM | Sistemas |
| Revisar backups | Confirmar que la copia diaria se ha generado | Sistemas |
| Revisar monitorización | Comprobar alertas o métricas anómalas | Sistemas |

## Comandos diarios recomendados

Comprobar Apache:

```bash
sudo systemctl status apache2
```

Comprobar MySQL:

```bash
sudo systemctl status mysql
```

Comprobar espacio en disco:

```bash
df -h
```

Comprobar memoria:

```bash
free -h
```

Comprobar procesos activos:

```bash
top
```

Comprobar firewall:

```bash
sudo ufw status verbose
```

## Tareas semanales

- Revisar logs de Apache.
- Revisar logs de MySQL.
- Comprobar intentos de acceso SSH.
- Revisar el tamaño de los backups.
- Comprobar actualizaciones pendientes.
- Verificar que Netdata sigue activo.
- Revisar que no existan archivos temporales innecesarios.

## Tareas mensuales

- Realizar una prueba de restauración de backup.
- Revisar usuarios con acceso SSH.
- Revisar reglas del firewall.
- Comprobar versiones de Apache, PHP y MySQL.
- Actualizar la documentación si cambia la infraestructura.
- Revisar el plan de recuperación ante desastres.

## Revisión de logs

Logs de Apache:

```bash
sudo tail -f /var/log/apache2/access.log
sudo tail -f /var/log/apache2/error.log
```

Logs del sistema:

```bash
journalctl -xe
```

Logs de autenticación:

```bash
sudo tail -f /var/log/auth.log
```

## Gestión de incidencias

Cuando se detecte una incidencia, se recomienda seguir estos pasos:

1. Identificar el servicio afectado.
2. Comprobar el estado con `systemctl`.
3. Revisar los logs relacionados.
4. Comprobar recursos del sistema.
5. Aplicar una solución temporal si es necesario.
6. Documentar la incidencia.
7. Revisar si hay que actualizar la documentación.

## Incidencias comunes

| Problema | Posible causa | Acción recomendada |
|---|---|---|
| La web no carga | Apache detenido | Reiniciar Apache |
| Error de base de datos | MySQL detenido | Revisar servicio MySQL |
| No hay espacio en disco | Logs o backups acumulados | Limpiar archivos antiguos |
| No se puede acceder por SSH | Firewall o SSH mal configurado | Revisar UFW y servicio SSH |
| Netdata no carga | Servicio detenido o puerto bloqueado | Revisar Netdata y firewall |

## Buenas prácticas

- No realizar cambios sin documentarlos.
- Mantener copias de seguridad actualizadas.
- Revisar servicios críticos después de cada cambio.
- Usar cuentas con permisos mínimos.
- No compartir claves privadas SSH.
- Registrar las incidencias importantes.



## Operación del balanceador HAProxy

### Objetivo

Esta sección describe las tareas básicas de mantenimiento y revisión del balanceador HAProxy.

### Comprobar estado del servicio

```bash
sudo systemctl status haproxy
```

### Validar configuración

Antes de reiniciar HAProxy, validar el archivo de configuración:

```bash
sudo haproxy -c -f /etc/haproxy/haproxy.cfg
```

### Reiniciar HAProxy

```bash
sudo systemctl restart haproxy
```

### Revisar logs de HAProxy

```bash
journalctl -u haproxy
```

### Incidencias comunes

| Problema | Posible causa | Solución |
|---|---|---|
| HAProxy no inicia | Error en `haproxy.cfg` | Validar configuración con `haproxy -c` |
| La web no responde | Backend Apache caído | Revisar Apache |
| Error 503 | No hay servidores backend disponibles | Comprobar servidores configurados |
| Puerto ocupado | Otro servicio usa el puerto 80 | Revisar servicios activos |
| Cambios no aplicados | HAProxy no fue reiniciado | Reiniciar servicio |

### Buenas prácticas

- Validar configuración antes de reiniciar.
- Mantener copia de seguridad de `haproxy.cfg`.
- Revisar logs después de cada cambio.
- Documentar cualquier modificación.
- Comprobar periódicamente los servidores backend.
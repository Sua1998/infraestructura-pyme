# 05. Guía de operación y mantenimiento

## 1. Objetivo

Definir tareas de operación diaria, semanal y mensual para mantener la infraestructura documentada.

## 2. Tareas diarias

| Tarea | Comando documental |
|---|---|
| Revisar estado de Apache | `systemctl status apache2` |
| Revisar estado de MySQL | `systemctl status mysql` |
| Revisar uso de disco | `df -h` |
| Revisar memoria | `free -h` |
| Revisar logs de errores web | `tail -n 50 /var/log/apache2/error.log` |
| Revisar backup nocturno | `tail -n 50 /var/log/backup_pyme.log` |

## 3. Tareas semanales

- Revisar actualizaciones disponibles:

```bash
sudo apt update
apt list --upgradable
```

- Comprobar rotación de backups.
- Revisar accesos SSH sospechosos:

```bash
sudo grep "Failed password" /var/log/auth.log
```

- Revisar métricas de Netdata.

## 4. Tareas mensuales

- Probar restauración de una copia de seguridad.
- Revisar usuarios de base de datos.
- Revisar reglas UFW.
- Revisar certificados SSL.
- Actualizar documentación si cambia la infraestructura.

## 5. Operación de HAProxy

Añadido en sesión 4 por cambio de alcance.

Comprobar estado:

```bash
sudo systemctl status haproxy
```

Reiniciar tras cambios:

```bash
sudo systemctl reload haproxy
```

Consultar logs:

```bash
sudo journalctl -u haproxy
```

## 6. Procedimiento ante caída web

1. Comprobar HAProxy.
2. Comprobar Apache.
3. Revisar logs.
4. Comprobar espacio en disco.
5. Reiniciar servicio si procede.
6. Registrar incidencia en `REVISION.md` o issue.

```bash
sudo systemctl status haproxy
sudo systemctl status apache2
sudo systemctl restart apache2
```

## 7. Buenas prácticas

- No hacer cambios directamente en producción sin documentar.
- Crear issue antes de cambios relevantes.
- Mantener `CHANGELOG.md` actualizado.
- Usar Pull Request para cambios en documentación.

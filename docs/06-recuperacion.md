# 06. Plan de recuperación ante desastres

## 1. Objetivo

Definir cómo recuperar la infraestructura ante fallos graves.

## 2. Escenarios contemplados

| Escenario | Impacto | Acción |
|---|---|---|
| Caída de Apache | Web no disponible | Reinicio y revisión de logs |
| Corrupción de base de datos | Pérdida parcial de datos | Restaurar backup SQL |
| Borrado accidental de web | Web incompleta | Restaurar `/var/www/empresa` |
| Fallo de configuración | Servicio no inicia | Restaurar configuración anterior |
| Servidor comprometido | Riesgo alto | Aislar, restaurar y cambiar credenciales |

## 3. Recuperación de Apache

```bash
sudo systemctl restart apache2
sudo apache2ctl configtest
sudo tail -n 100 /var/log/apache2/error.log
```

## 4. Recuperación de base de datos

Restaurar backup:

```bash
mysql -u root -p web_empresa < /backup/2026-05-04/mysql/web_empresa.sql
mysql -u root -p gestion_interna < /backup/2026-05-04/mysql/gestion_interna.sql
```

## 5. Recuperación de archivos web

```bash
rsync -avz /backup/2026-05-04/www/empresa/ /var/www/empresa/
```

Corregir permisos:

```bash
sudo chown -R www-data:www-data /var/www/empresa
```

## 6. Recuperación de configuración Apache

```bash
rsync -avz /backup/2026-05-04/config/apache2/ /etc/apache2/
sudo apache2ctl configtest
sudo systemctl reload apache2
```

## 7. Recuperación ante incidente de seguridad

1. Desconectar servidor de red si hay compromiso grave.
2. Revisar logs.
3. Cambiar contraseñas y claves.
4. Restaurar desde backup limpio.
5. Aplicar actualizaciones.
6. Revisar reglas UFW.
7. Documentar la incidencia.

## 8. RTO y RPO

| Concepto | Valor objetivo |
|---|---|
| RTO | 4 horas |
| RPO | 24 horas |

## 9. Checklist final

- [ ] Servicio restaurado.
- [ ] Logs revisados.
- [ ] Backup validado.
- [ ] Contraseñas revisadas.
- [ ] Documentación actualizada.

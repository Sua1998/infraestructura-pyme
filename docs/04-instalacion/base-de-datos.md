# Base de datos MySQL/MariaDB

## 1. Objetivo

Documentar la preparación de un servidor de base de datos para la web corporativa y para la gestión interna.

## 2. Instalación documental

```bash
sudo apt update
sudo apt install mysql-server -y
```

Comprobación:

```bash
sudo systemctl status mysql
```

## 3. Endurecimiento inicial

```bash
sudo mysql_secure_installation
```

Decisiones recomendadas:

| Pregunta | Respuesta recomendada |
|---|---|
| Validar contraseñas | Sí |
| Eliminar usuarios anónimos | Sí |
| Deshabilitar login remoto de root | Sí |
| Eliminar base de datos test | Sí |
| Recargar privilegios | Sí |

## 4. Bases de datos

```sql
CREATE DATABASE web_empresa CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE DATABASE gestion_interna CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

## 5. Usuarios

```sql
CREATE USER 'web_user'@'localhost' IDENTIFIED BY 'Cambiar_esta_clave_segura';
CREATE USER 'gestion_user'@'localhost' IDENTIFIED BY 'Cambiar_esta_clave_segura';

GRANT SELECT, INSERT, UPDATE, DELETE ON web_empresa.* TO 'web_user'@'localhost';
GRANT SELECT, INSERT, UPDATE, DELETE ON gestion_interna.* TO 'gestion_user'@'localhost';

FLUSH PRIVILEGES;
```

## 6. Seguridad

| Medida | Descripción |
|---|---|
| Usuarios separados | Un usuario por aplicación |
| Mínimos privilegios | Evitar `GRANT ALL` salvo necesidad |
| Sin acceso remoto | Bind local salvo que sea imprescindible |
| Backups | Copias automáticas con `mysqldump` |
| Contraseñas robustas | Claves largas y no reutilizadas |

## 7. Verificación

```bash
mysql -u web_user -p web_empresa
mysql -u gestion_user -p gestion_interna
```

## 8. Restauración documental

```bash
mysql -u root -p web_empresa < web_empresa_backup.sql
```

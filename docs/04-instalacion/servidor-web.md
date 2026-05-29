# Servidor web Apache y PHP

## 1. Objetivo

Documentar los pasos necesarios para preparar un servidor web Apache con PHP dentro de una infraestructura LAMP.

> No se ejecuta realmente. Estos comandos son documentación técnica.

## 2. Paquetes necesarios

```bash
sudo apt update
sudo apt install apache2 php libapache2-mod-php php-mysql -y
```

## 3. Verificación del servicio Apache

```bash
sudo systemctl status apache2
```

Resultado esperado:

```text
Active: active (running)
```

## 4. Estructura web recomendada

| Ruta | Uso |
|---|---|
| `/var/www/html` | Directorio web por defecto |
| `/etc/apache2/sites-available` | VirtualHosts disponibles |
| `/etc/apache2/sites-enabled` | VirtualHosts activos |
| `/var/log/apache2/access.log` | Log de accesos |
| `/var/log/apache2/error.log` | Log de errores |

## 5. VirtualHost de ejemplo

```apache
<VirtualHost *:80>
    ServerName empresa.local
    DocumentRoot /var/www/empresa

    <Directory /var/www/empresa>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/empresa_error.log
    CustomLog ${APACHE_LOG_DIR}/empresa_access.log combined
</VirtualHost>
```

Activación documental:

```bash
sudo a2ensite empresa.conf
sudo systemctl reload apache2
```

## 6. Prueba PHP

Archivo de prueba:

```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/empresa/info.php
```

Recomendación de seguridad:

```bash
sudo rm /var/www/empresa/info.php
```

## 7. Añadido de sesión 4: HAProxy delante de Apache

El cambio de alcance del cliente pide añadir HAProxy como balanceador delante de Apache.

Instalación documental:

```bash
sudo apt install haproxy -y
```

Configuración mínima:

```haproxy
frontend http_front
    bind *:80
    default_backend apache_back

backend apache_back
    server web01 192.168.1.10:80 check
```

Reinicio documental:

```bash
sudo systemctl restart haproxy
```

## 8. Buenas prácticas

- No exponer archivos de prueba como `info.php`.
- Mantener Apache actualizado.
- Usar HTTPS mediante Certbot.
- Revisar logs periódicamente.
- No exponer directorios sensibles.

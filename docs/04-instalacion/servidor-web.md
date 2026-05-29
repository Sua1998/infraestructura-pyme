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


## Balanceador de carga HAProxy

### Objetivo

El cliente solicita añadir un balanceador de carga HAProxy delante del servidor Apache.

HAProxy recibirá las peticiones HTTP/HTTPS de los usuarios y las reenviará al servidor web Apache correspondiente. Esto permite mejorar la disponibilidad y preparar la infraestructura para crecer en el futuro.

### Instalación propuesta

> Nota: este proyecto solo documenta el proceso. Los comandos no se ejecutan durante la práctica.

```bash
sudo apt update
sudo apt install haproxy -y
```

### Comprobación del servicio

```bash
sudo systemctl status haproxy
```

Activar HAProxy en el arranque:

```bash
sudo systemctl enable haproxy
```

Iniciar HAProxy:

```bash
sudo systemctl start haproxy
```

### Archivo de configuración

El archivo principal de configuración de HAProxy es:

```text
/etc/haproxy/haproxy.cfg
```

### Ejemplo básico de configuración

```text
frontend http_front
    bind *:80
    default_backend apache_back

backend apache_back
    balance roundrobin
    server web1 192.168.1.10:80 check
```

### Explicación de la configuración

| Directiva | Función |
|---|---|
| `frontend` | Define el punto de entrada del tráfico |
| `bind *:80` | Escucha peticiones HTTP en el puerto 80 |
| `default_backend` | Envía el tráfico al grupo de servidores definido |
| `backend` | Define los servidores Apache de destino |
| `balance roundrobin` | Reparte las peticiones de forma equilibrada |
| `check` | Comprueba si el servidor backend responde |

### Validar configuración

Antes de reiniciar HAProxy se debe validar el archivo de configuración:

```bash
sudo haproxy -c -f /etc/haproxy/haproxy.cfg
```

### Reiniciar HAProxy

```bash
sudo systemctl restart haproxy
```

### Reglas de firewall para HAProxy

Permitir tráfico HTTP:

```bash
sudo ufw allow 80/tcp
```

Permitir tráfico HTTPS:

```bash
sudo ufw allow 443/tcp
```

### Buenas prácticas

- Validar la configuración antes de reiniciar HAProxy.
- Documentar todos los servidores backend.
- Revisar logs si aparecen errores de conexión.
- Usar HTTPS en producción.
- Monitorizar el estado del servicio.
- Monitorizar el estado del servicio.
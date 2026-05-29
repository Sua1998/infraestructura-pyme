# SSH y Firewall UFW

## 1. Objetivo

Documentar una configuración segura de acceso remoto SSH y firewall básico con UFW.

Este archivo se utiliza en la sesión 3 para provocar y resolver un conflicto con rebase.

## 2. Configuración SSH recomendada

Archivo:

```bash
/etc/ssh/sshd_config
```

Parámetros recomendados:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
MaxAuthTries 3
```

Reinicio documental:

```bash
sudo systemctl restart ssh
```

## Configuración de firewall con UFW

Se propone usar UFW para definir una política básica de seguridad en el servidor.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80,443/tcp
sudo ufw enable
```

Estas reglas permiten la administración por SSH y el tráfico web HTTP/HTTPS.
```

## 4. Explicación de reglas

| Regla | Motivo |
|---|---|
| `deny incoming` | Bloquea conexiones entrantes no autorizadas |
| `allow outgoing` | Permite tráfico saliente normal |
| `allow from 192.168.1.0/24 to any port 22` | Limita SSH a la red de oficina |
| `allow 80/tcp` | Permite HTTP |
| `allow 443/tcp` | Permite HTTPS |

## 5. Comprobación

```bash
sudo ufw status verbose
```

Resultado esperado:

```text
Status: active
22/tcp ALLOW 192.168.1.0/24
80/tcp ALLOW Anywhere
443/tcp ALLOW Anywhere
```

## 6. Buenas prácticas

- No permitir SSH abierto a todo Internet salvo necesidad.
- Usar claves SSH.
- Deshabilitar acceso root.
- Revisar logs de autenticación:

```bash
sudo tail -f /var/log/auth.log
```



## Reglas UFW

- Permitir SSH solo desde IP de la oficina: `ufw allow from 192.168.1.0/24 to any port 22`
- Permitir tráfico web: `ufw allow 80/tcp` y `ufw allow 443/tcp`
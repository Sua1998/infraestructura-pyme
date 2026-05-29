# Monitorización

## 1. Objetivo

Documentar una estrategia sencilla de monitorización para la infraestructura.

Se propone Netdata como opción principal y un script de comprobación como alternativa mínima.

## 2. Opción A: Netdata

Instalación documental:

```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```

Comprobación:

```bash
sudo systemctl status netdata
```

Acceso:

```text
http://IP_SERVIDOR:19999
```

## 3. Métricas a revisar

| Métrica | Motivo |
|---|---|
| CPU | Detectar sobrecarga |
| RAM | Detectar consumo anómalo |
| Disco | Evitar falta de espacio |
| Red | Ver tráfico entrante y saliente |
| Apache | Ver actividad web |
| MySQL | Detectar consultas o carga excesiva |

## 4. Opción B: script básico de check

```bash
#!/bin/bash

echo "=== Uso de disco ==="
df -h

echo "=== Memoria ==="
free -h

echo "=== Servicios ==="
systemctl is-active apache2
systemctl is-active mysql
systemctl is-active ssh
```

Guardar como:

```bash
/usr/local/bin/check_sistema.sh
```

Permisos:

```bash
sudo chmod +x /usr/local/bin/check_sistema.sh
```

## 5. Ejecución periódica con cron

```bash
*/30 * * * * /usr/local/bin/check_sistema.sh >> /var/log/check_sistema.log 2>&1
```

## 6. Alertas recomendadas

| Situación | Acción |
|---|---|
| Disco > 85% | Revisar logs y backups antiguos |
| RAM > 90% | Revisar procesos |
| Apache caído | Reiniciar servicio y revisar logs |
| MySQL caído | Revisar logs y espacio en disco |




# Monitorización del sistema

## Objetivo

La monitorización permite comprobar el estado del servidor y detectar problemas antes de que afecten al servicio web o a la base de datos.

En esta infraestructura se propone usar **Netdata** como herramienta principal de monitorización, ya que permite ver métricas del sistema en tiempo real mediante una interfaz web sencilla.

## Herramienta seleccionada

| Herramienta | Uso principal | Motivo de elección |
|---|---|---|
| Netdata | Monitorización en tiempo real | Fácil instalación, interfaz web y métricas completas |
| journalctl | Revisión de logs del sistema | Integrado en Linux |
| systemctl | Estado de servicios | Permite comprobar Apache, MySQL y otros servicios |

## Instalación de Netdata

```bash
sudo apt update
sudo apt install netdata -y
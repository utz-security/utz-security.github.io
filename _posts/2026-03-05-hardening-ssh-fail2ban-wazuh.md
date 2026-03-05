---
layout: post
title: "Defensa Activa: Hardening SSH, Fail2ban y Monitoreo con Wazuh SIEM"
date: 2026-03-05
categories: [Laboratorios, Blue Team]
tags: [Linux, SSH, Fail2ban, Wazuh, SOC]
---

Bienvenidos a un nuevo write-up en mi bitácora. En este laboratorio práctico, documentaré el proceso completo de asegurar un servidor Ubuntu 24.04, desde la configuración inicial hasta la detección de ataques en tiempo real utilizando herramientas de nivel corporativo.

## Objetivo del Laboratorio
El objetivo fue construir una defensa en profundidad (Defense in Depth) para un servidor expuesto, dividiendo el trabajo en tres fases:
1. **Prevención:** Hardening del servicio SSH.
2. **Contención automatizada:** Implementación de Fail2ban.
3. **Visibilidad:** Monitoreo y correlación de eventos con Wazuh SIEM.

---

## Fase 1: Hardening de SSH (Modo Dios)

El primer paso para asegurar el perímetro fue modificar la configuración por defecto de `/etc/ssh/sshd_config`. Las acciones clave fueron:

* **Cambio de puerto:** Se migró del puerto 22 al puerto 2222 para evitar escaneos automatizados (ruido de fondo en internet).
* **Bloqueo de Root:** Se estableció `PermitRootLogin no` para mitigar ataques de fuerza bruta directos al superusuario.
* **Autenticación por llaves criptográficas:** Se generó un par de llaves **ED25519** (el estándar más seguro actualmente) y se deshabilitó por completo la autenticación por contraseñas (`PasswordAuthentication no`).

> **Troubleshooting en Ubuntu 24.04:** Al cambiar el puerto, el servicio parecía seguir escuchando en el puerto 22. Esto se debe al `ssh.socket` introducido en versiones recientes de Ubuntu. La solución fue deshabilitar el socket con `sudo systemctl disable --now ssh.socket` y reiniciar el servicio clásico.

---

## Fase 2: Defensa Activa con Fail2ban

Para castigar a los atacantes insistentes, instalé Fail2ban configurando una celda (`jail.local`) específica para SSH.

```ini
[sshd]
enabled = true
port = 2222
backend = systemd
maxretry = 3
findtime = 10m
bantime = 1h
Superando al "Jefe Final" (Error de Systemd)
Al intentar iniciar Fail2ban en Ubuntu 24.04, el servicio fallaba repetidamente. Utilizando el comando de diagnóstico sudo fail2ban-server -t, identifiqué que Fail2ban (basado en Python) no podía leer los logs modernos de systemd.

La solución fue instalar el traductor necesario:

Bash
sudo apt install python3-systemd -y
Tras corregir duplicados en la configuración, el servicio levantó exitosamente (active (running)).

Fase 3: Simulacro y Detección en Wazuh (SOC)
Con la fortaleza levantada, me puse el sombrero de Red Team y lancé un ataque de fuerza bruta intencional contra el puerto 2222 usando usuarios inexistentes.

La respuesta del sistema fue perfecta:

Wazuh (Detección): El agente reportó los intentos fallidos. El Manager correlacionó los eventos y disparó una alerta de Nivel 8 (Regla 5758: Maximum authentication attempts exceeded) en el dashboard.

Fail2ban (Contención): Aisló inmediatamente la IP origen a nivel de red, bloqueando la conexión.

![Alerta de fuerza bruta detectada por Wazuh](/assets/severities.png)

![IP del atacante bloqueada por Fail2ban](/assets/jail.png)

Conclusión
Este laboratorio demuestra la importancia de no depender de una sola capa de seguridad. El hardening evita el acceso no autorizado, Fail2ban automatiza la respuesta ante atacantes insistentes, y Wazuh nos da la visibilidad necesaria (como operadores de SOC) para saber exactamente qué está sucediendo en nuestra infraestructura.

¡Gracias por leer!

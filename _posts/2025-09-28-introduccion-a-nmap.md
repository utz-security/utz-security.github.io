---
layout: post
title: "Introducción a Nmap: escaneo básico en Linux"
date: 2025-09-28
categories: [herramientas, nmap, linux]
---

# Introducción a Nmap 😃

Nmap (**Network Mapper**) es una de las herramientas más utilizadas en ciberseguridad y administración de sistemas.  
Sirve para **descubrir hosts y servicios** en una red mediante el envío de paquetes y el análisis de las respuestas.

---

## 🔹 Instalación en Linux

**Debian/Ubuntu:**
```bash
sudo apt update
sudo apt install nmap -y

Fedora/RedHat:

sudo dnf install nmap -y

🔹 Comandos básicos de Nmap
1️⃣ Escaneo rápido de un host
nmap 192.168.1.10


Detecta puertos abiertos en el host.

Útil para obtener una visión rápida de los servicios activos.

2️⃣ Escaneo de múltiples hosts
nmap 192.168.1.0/24


Escanea un rango de IPs en la red.

Permite mapear rápidamente la red local.

3️⃣ Escaneo de puertos específicos
nmap -p 22,80,443 192.168.1.10


Solo escanea los puertos indicados.

Útil para centrarse en servicios críticos.

4️⃣ Detección de sistema operativo y servicios
nmap -A 192.168.1.10


Identifica el sistema operativo del host.

Detecta versiones de servicios y posibles vulnerabilidades.

5️⃣ Escaneo con scripts
nmap -sC 192.168.1.10


Ejecuta scripts de Nmap para obtener información adicional de seguridad.

Permite descubrir vulnerabilidades comunes y servicios específicos.

🔹 Consejos y buenas prácticas

Siempre tener permiso para escanear una red.

Comenzar con escaneos rápidos y luego profundizar según la necesidad.

Combinar Nmap con otras herramientas como Wireshark para análisis de tráfico.

Documentar los resultados de cada escaneo para futuras auditorías.

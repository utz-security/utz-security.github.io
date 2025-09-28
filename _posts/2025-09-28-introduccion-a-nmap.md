---
layout: post
title: "Introducción a Nmap: escaneo básico en Linux"
date: 2025-09-28
categories: [herramientas, nmap, linux]
---

Nmap (**Network Mapper**) es una de las herramientas más utilizadas en ciberseguridad y administración de sistemas.  
Sirve para **descubrir hosts y servicios** en una red mediante el envío de paquetes y el análisis de las respuestas.

---

## 🔹 Instalación en Linux

En la mayoría de distribuciones basadas en Debian/Ubuntu:

```bash
sudo apt update
sudo apt install nmap -y

En Fedora o RedHat:

sudo dnf install nmap -y

🔹 Escaneo básico

Escaneo de un host:

nmap 192.168.1.10


Escaneo de un rango de IPs:

nmap 192.168.1.0/24


Detección de sistema operativo y servicios:

nmap -A 192.168.1.10

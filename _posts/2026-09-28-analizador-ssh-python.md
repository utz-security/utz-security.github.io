#!/usr/bin/env python3
import re
from collections import Counter

def analizar_auth_log(ruta_log):
    # Patrón para detectar intentos fallidos de SSH en Linux
    patron_ip = re.compile(r"Failed password for .* from (\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})")
    
    ips_atacantes = []

    try:
        with open(ruta_log, "r") as archivo:
            for linea in archivo:
                match = patron_ip.search(linea)
                if match:
                    ips_atacantes.append(match.group(1))
                    
        # Contar frecuencia de cada IP
        conteo_ips = Counter(ips_atacantes)
        
        print("--- REPORTE DE SEGURIDAD SOC (SSH) ---")
        print(f"Total de intentos fallidos detectados: {len(ips_atacantes)}\n")
        print("IPs con mayor cantidad de intentos:")
        for ip, frecuencia in conteo_ips.most_common(5):
            print(f" [!] IP: {ip} -> {frecuencia} intentos fallidos")
            
    except FileNotFoundError:
        print(f"[-] No se encontró el archivo en la ruta: {ruta_log}")

if __name__ == "__main__":
    analizar_auth_log("auth.log")

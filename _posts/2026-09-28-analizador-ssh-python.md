```python
#!/usr/bin/env python3
import re
from collections import Counter

def analizar_auth_log(ruta_log):
    patron_ip = re.compile(r"Failed password for .* from (\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})")
    ips_atacantes = []
    
    try:
        with open(ruta_log, "r") as archivo:
            for linea in archivo:
                match = patron_ip.search(linea)
                if match:
                    ips_atacantes.append(match.group(1))
                    
        conteo_ips = Counter(ips_atacantes)
        print("--- REPORTE DE SEGURIDAD SOC (SSH) ---")
        for ip, frecuencia in conteo_ips.most_common(5):
            print(f" [!] IP: {ip} -> {frecuencia} intentos fallidos")
    except FileNotFoundError:
        print("[-] No se encontró el archivo")

if __name__ == "__main__":
    analizar_auth_log("auth.log")
```

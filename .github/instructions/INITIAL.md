1. Realizar un escaneo de cabeceras HTTP con curl.
# Objetivo: Buscar información sensible expuesta en las cabeceras (versiones de software, etc.).
# Comando:
`curl -I http://dominio.com`

2. Realizar un escaneo de cifrado TLS con sslscan.
# Objetivo: Verificar si el servidor usa protocolos o cifrados débiles y obsoletos.
# Comando:
`sslscan --no-colour https://dominio.com`

3. Realizar un escaneo de cabeceras de seguridad con shcheck.
# Objetivo: Auditar la implementación de cabeceras de seguridad (CSP, HSTS, etc.).
# Comando:
`python3 /home/phara/Tools/shcheck/shcheck.py https://dominio.com/`
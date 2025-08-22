1. Realizar un escaneo de cabeceras HTTP con curl.
# Objetivo: Buscar información sensible expuesta en las cabeceras (versiones de software, etc.).
# Comando:
`curl -I http://dominio.com`

// Acá pueden ir las herramientas que queremos que se ejecute al principio, Ej. shcheck (para ver las cabeceras), sslscan (para ver el cifrado), etc.

# Hacking Day 2025 — Laboratorio de Pentesting con MCP, VSCode y Copilot

> [!IMPORTANT]
> Documentación en desarrollo.

**Evento:** [Hacking Day 2025](https://www.hackingday.com.ar/) — 8/8, Paraná, Entre Ríos, Argentina

Jornada intensiva dedicada a la ciberseguridad, la cultura hacker y la innovación tecnológica.

**Laboratorio:** MCP + VSCode + GitHub Copilot para Pentesting Controlado

**Laboratorio presentado por Hugo Avila**

Contacto:
- Linkedin: [devhugoavila](https://www.linkedin.com/in/devhugoavila/)
- GitHub: [hugok2k](https://github.com/hugok2k)
- X: [@hugok2k](https://x.com/hugok2k)

Este laboratorio muestra cómo orquestar agentes MCP (filesystem, playwright y burp) desde VSCode con GitHub Copilot para apoyar un proceso de pentesting educativo, limitado y documentado. El enfoque es enseñar metodología, no explotación agresiva.


## Objetivos del Laboratorio

- Comprender cómo personalizar el comportamiento de Copilot mediante `copilot-instructions.md`.
- Usar el protocolo MCP para dotar a Copilot de contexto y herramientas específicas (navegación, filesystem, proxy de seguridad).
- Seguir un flujo de pentesting reproducible: Alcance → Reconocimiento → Pruebas → Evidencias → Reporte.
- Generar hallazgos documentados usando la plantilla estándar.

## Componentes Clave

| Componente             | Ruta                                 | Propósito                                                                    |
| ---------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| Instrucciones de rol   | `./.github/copilot-instructions.md`  | Define el perfil del asistente (Red Team) y el flujo de trabajo obligatorio. |
| Alcance                | `./.github/instructions/SCOPE.md`    | Lista única de objetivos autorizados.                                        |
| Guía inicial           | `./.github/instructions/INITIAL.md`  | Escaneos básicos de enumeración pasiva/semi-pasiva.                          |
| Plantilla de hallazgos | `./.github/instructions/TEMPLATE.md` | Estructura uniforme para cada vulnerabilidad confirmada.                     |
| Configuración MCP      | `./.vscode/mcp.json`                 | Declara servidores (agentes) MCP disponibles en VS Code.                     |
| Requerimientos previos | `./REQUERIMIENTOS.md`                | Software a instalar antes del taller.                                        |

## Estructura mínima del repositorio

```
├── .github/
│   ├── copilot-instructions.md
│   └── instructions/     <-- Esta carpeta puede estar en otro lugar
│       ├── SCOPE.md
│       ├── INITIAL.md
│       └── TEMPLATE.md
└── .vscode/
    └── mcp.json
```

## Requerimientos

1. Node.js (incluye npm y npx)
2. Java (JDK) para ejecutar el servidor Burp MCP
3. Visual Studio Code + sesión iniciada con tu cuenta GitHub
4. GitHub Copilot habilitado

Verificaciones rápidas:

```bash
node -v
npm -v
java -version
```

## Configuración Inicial Paso a Paso

1. Clonar el repositorio (o copiar la estructura mínima).
2. Abrir la carpeta en VSCode.
3. Confirmar que GitHub Copilot esté activo (icono en la barra inferior) y que la cuenta esté autenticada.
4. Revisar y, si hace falta, editar rutas en `.vscode/mcp.json` para que coincidan con tu entorno local:
   - Ruta del directorio del proyecto para el servidor `filesystem`.
   - Ruta al ejecutable de Java y al JAR / proxy de Burp (`mcp-proxy.jar` o equivalente).


## Instalación de Burp MCP y mcp-proxy.jar

Para que el servidor MCP de Burp funcione correctamente, necesitas instalar la extensión oficial "Burp MCP Server" en Burp Suite y descargar el archivo `mcp-proxy.jar`:

1. Abre Burp Suite y ve a la pestaña **Extensiones** (BApp Store).
2. Busca e instala la extensión **Burp MCP Server**.
3. Una vez instalada, ingresa a la extensión MCP en las tools y descarga `mcp-proxy.jar` haciendo click en el botón **Extract server proxy jar**
4. Copia la ruta completa de `mcp-proxy.jar` y actualízala en el campo correspondiente de `.vscode/mcp.json`:

```jsonc
"burp": {
    "command": "/usr/bin/java",
    "args": [
        "-jar",
        "/ruta/a/mcp-proxy.jar", // <--- Actualiza aquí la ruta real
        "--sse-url",
        "http://127.0.0.1:9876"
    ]
}
```

Sin este archivo, el servidor MCP de Burp no podrá iniciar ni comunicarse con VSCode.


## Descripción de los Servidores MCP

| Servidor   | Función                                                | Casos de uso                                                                       |
| ---------- | ------------------------------------------------------ | ---------------------------------------------------------------------------------- |
| filesystem | Lectura/creación de archivos dentro del lab            | Leer `SCOPE.md`, añadir hallazgos a `INFORME.md`.                                  |
| playwright | Navegación/automatización del navegador                | Reconocimiento, captura de endpoints, enumeración de formularios.                  |
| burp       | Envío/alteración de solicitudes HTTP y análisis activo | Pruebas de parámetros, manipulación de peticiones, validación de vulnerabilidades. |

## Resolución de Problemas (Troubleshooting)

| Síntoma                      | Posible causa                               | Acción sugerida                                             |
| ---------------------------- | ------------------------------------------- | ----------------------------------------------------------- |
| Copilot no ve servidores MCP | Rutas incorrectas o VS Code no re-cargó     | Reabrir VS Code, verificar `mcp.json` y rutas absolutas.    |
| Burp MCP no responde         | Java / JAR incorrecto o puerto SSE inválido | Verifica versión Java y ruta al JAR, revisa puertos libres. |

## Extensiones / Evoluciones Futuras

- Añadir servidor MCP para escaneo SCA (dependencias) o análisis de contenedores.
- Integrar un pipeline CI que valide formato de hallazgos.
- Añadir scripts para normalizar severidad (CVSS) automáticamente.
- Incorporar ejemplos de payloads seguros (plantilla de cheat‑sheet interna).

## Aviso Legal / Ética

> [!WARNING]
> Este laboratorio es solo para fines educativos sobre objetivos autorizados. No reutilices técnicas ni herramientas contra sistemas sin permiso explícito. El incumplimiento puede ser ilegal.

---
¿Mejoras o dudas? Abre un issue.

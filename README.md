# Go IIS Shortname Scanner

Este escáner está diseñado para descubrir archivos y directorios ocultos en servidores IIS a través de la vulnerabilidad IIS Tilde Enumeration.

El IIS Tilde Enumeration es una vulnerabilidad que permite descubrir archivos y directorios ocultos en servidores web Microsoft IIS.

### Cómo Funciona

Windows mantiene compatibilidad con MS-DOS creando shortnames (por ejemplo, `PROGRA~1` para "Program Files"). Los servidores IIS pueden exponer inadvertidamente estos shortnames a través de solicitudes HTTP específicas usando caracteres como `*` y `~`.

**Ejemplo:**
- Archivo real: `transfer.aspx`
- Shortname: `TRANSF~1.ASP`
- El escáner descubre: `TRANSF~1.ASP` → revela la existencia de un archivo oculto

Esta vulnerabilidad puede afectar a:

- **IIS 7.0** y anteriores (más vulnerables)
- **IIS 7.5** (Windows Server 2008 R2)
- **IIS 8.0** (Windows Server 2012)
- **IIS 8.5** (Windows Server 2012 R2)
- **IIS 10.0** (parcialmente, dependiendo de la configuración)

**Aviso Legal:** Solo usa esta herramienta contra sistemas para los que tengas permiso explícito de prueba. El escaneo no autorizado es ilegal.

### Binarios Pre-compilados

Descarga la última versión desde la página de [Releases](../../releases):

- **Windows (AMD64)**: `iis-scanner.exe`
- **Linux (AMD64)**: `iis-scanner-linux`
- **macOS (Intel)**: `iis-scanner-darwin-amd64`
- **macOS (Apple Silicon)**: `iis-scanner-darwin-arm64`

No se requieren dependencias - ejecución de binario único.

## Uso

### Escaneo Básico

```bash
# Windows
.\iis-scanner.exe -u http://target.com/

# Linux/macOS
./iis-scanner -u http://target.com/
```

### Opciones Avanzadas

```bash
# Escanear con 50 hilos
.\iis-scanner.exe -u http://target.com/ -t 50

# Usar proxy
.\iis-scanner.exe -u http://target.com/ -p http://127.0.0.1:8080

# Modo verbose
.\iis-scanner.exe -u http://target.com/ -v

# Método HTTP personalizado
.\iis-scanner.exe -u http://target.com/ -method OPTIONS

# Timeout personalizado (milisegundos)
.\iis-scanner.exe -u http://target.com/ -timeout 30000
```

### Opciones Completas

```
Uso: iis-scanner [opciones]

Opciones:
  -u string
        Target URL (requerido)
        Ejemplo: http://target.com/
  
  -t int
        Número de hilos (predeterminado: 20)
        Rango: 1-50
  
  -p string
        URL del proxy (opcional)
        Ejemplo: http://127.0.0.1:8080
  
  -v    Modo verbose - mostrar progreso detallado
  
  -method string
        Método HTTP a usar (opcional)
        Opciones: OPTIONS, GET, POST, HEAD, TRACE, TRACK, DEBUG
        Predeterminado: Auto-detectar
  
  -timeout int
        Timeout de solicitud en milisegundos (predeterminado: 20000)
  
  -user-agent string
        Encabezado User-Agent personalizado
  
  -magic-parts string
        Magic parts separadas por comas
        Predeterminado: /~1/.rem,/~1/,/a.aspx,/a.asp,...
  
  -scan-chars string
        Caracteres a usar para el escaneo
        Predeterminado: ETAONRISHDLFCMUGYPWBVKJXQZ0123456789...
  
  -no-color
        Deshabilitar salida con colores
```

## Ejemplo de Salida

```
    ____  ____  _____   _____
   /  _/ /  _/ / ___/  / ___/_________ _____  ____  ___  _____
   / /   / /   \__ \   \__ \/ ___/ __ '/ __ \/ __ \/ _ \/ ___/
 _/ /  _/ /   ___/ /  ___/ / /__/ /_/ / / / / / / /  __/ /
/___/ /___/  /____/  /____/\___/\__,_/_/ /_/_/ /_/\___/_/
                                            v1.0 (Go)

[+] Objetivo: http://10.129.66.40/
[+] Estado: Vulnerable
[+] Método: OPTIONS
[+] Parte Mágica: /~1/.rem
[+] Identificando caracteres válidos...
[+] Caracteres Válidos: TCAU
[+] Caracteres de Extensión Válidos: ASP
[+] Escaneando archivos y directorios...
[DIR]  ASPNET~1
[DIR]  UPLOAD~1
[FILE] TRANSF~1.ASP
[FILE] CSASPX~1.CS

[+] Encontrados 2 directorios y 4 archivos
[+] Escaneo completado en 1m6.6s
```

**Ejemplo:**
```
TRANSF~1.ASP → transfer.aspx
```

## Créditos y Referencias

Este escáner está basado en la implementación de:

- **Implementación Java**: [IIS-ShortName-Scanner](https://github.com/irsdl/IIS-ShortName-Scanner)

## Descargo de Responsabilidad Legal

Esta herramienta se proporciona solo con fines educativos y de pruebas de seguridad autorizadas.

**Eres responsable de:**
- Obtener la autorización adecuada antes de escanear cualquier sistema
- Cumplir con todas las leyes y regulaciones aplicables
- Usar esta herramienta de manera ética y responsable

**El autor NO es responsable de:**
- Cualquier mal uso o daño causado por esta herramienta
- Cualquier consecuencia legal resultante del uso no autorizado
- Cualquier daño a sistemas o redes

**Al usar esta herramienta, aceptas que tienes permiso para probar los sistemas objetivo.**

## Licencia


Este proyecto se publica bajo la Licencia MIT. Consulta el archivo LICENSE para más detalles.

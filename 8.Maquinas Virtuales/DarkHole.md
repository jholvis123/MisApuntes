# DarkHole - Pentesting Lab

**Tags:** `#sql-injection` `#web-security` `#reconnaissance` `#privilege-escalation` `#beginner-box`

## 🎯 Resumen Ejecutivo

DarkHole es una máquina virtual clasificada como "caja para principiantes, pero no fácil". Esta box está diseñada para practicar técnicas de pentesting web, específicamente vulnerabilidades de SQL Injection y escalación de privilegios en sistemas Linux Ubuntu.

**Consejo del creador:** No pierdas el tiempo con Brute-Force.

**Vulnerabilidades principales identificadas:**
- SQL Injection en parámetros web
- Configuración insegura de cookies de sesión
- Posible escalación de privilegios en sistema Ubuntu

## 📋 Descripción del Escenario

**Sistema objetivo:** Ubuntu Linux con servicios SSH y HTTP  
**Dificultad:** Principiante-Intermedio  
**Enfoque:** Explotación web y escalación de privilegios  

## 🔍 Fase 1: Reconocimiento
### 1.1 Descubrimiento de Red

Realizamos un escaneo ARP para identificar hosts activos en la red local:

```bash
arp-scan -I ens33 --localnet
```

**Resultado:**
```
Interface: ens33, type: EN10MB, MAC: 00:0c:29:1d:7d:8f, IPv4: 192.168.0.188
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.0.1     be:d1:65:65:26:d1   (Unknown: locally administered)
192.168.0.171   34:6f:24:57:38:cb   AzureWave Technology Inc.
192.168.0.235   00:0c:29:51:99:f6   VMware, Inc. ← OBJETIVO
192.168.0.254   00:05:04:03:02:01   Naray Information & Communication Enterprise
192.168.0.131   ba:3e:d8:a8:08:a9   (Unknown: locally administered)
```

**🎯 Target identificado:** `192.168.0.235`

## 🔍 Fase 2: Enumeración

### 2.1 Escaneo de Puertos

Realizamos un escaneo completo de puertos TCP:

```bash
nmap -p- -sS -n -Pn 192.168.0.235
```

**Resultado:**
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-10-10 12:54 -04
Nmap scan report for 192.168.0.235
Host is up (0.00073s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
MAC Address: 00:0C:29:51:99:F6 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 10.32 seconds
```

**🔍 Puertos abiertos identificados:** `22 (SSH), 80 (HTTP)`

### 2.2 Enumeración de Servicios

Realizamos un escaneo detallado de los servicios:

```bash
nmap -sCV -p22,80 192.168.0.235
```

**Resultado:**
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-10-10 13:08 -04
Nmap scan report for 192.168.0.235
Host is up (0.00022s latency).
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 e4:50:d9:50:5d:91:30:50:e9:b5:7d:ca:b0:51:db:74 (RSA)
|   256 73:0c:76:86:60:63:06:00:21:c2:36:20:3b:99:c1:f7 (ECDSA)
|_  256 54:53:4c:3f:4f:3a:26:f6:02:aa:9a:24:ea:1b:92:8c (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: DarkHole
MAC Address: 00:0C:29:51:99:F6 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.44 seconds
```

**🔍 Hallazgos importantes:**
- **SSH:** OpenSSH 8.2p1 Ubuntu (sin vulnerabilidades conocidas críticas)
- **HTTP:** Apache 2.4.41 con PHP habilitado
- **Cookie insegura:** PHPSESSID sin flag HttpOnly (potencial vector de ataque)
- **Sistema:** Ubuntu Linux

### 2.3 Exploración Web

Accedemos a la aplicación web en `http://192.168.0.235/`:

![Página principal DarkHole](Pasted%20image%2020251010131339.png)

**Observaciones:**
- Interfaz de login disponible
- Opción de registro de nuevos usuarios
- Título de la aplicación: "DarkHole"

Creamos una cuenta y accedemos al dashboard:

![Dashboard de usuario](Pasted%20image%2020251010131748.png)

**🔍 Análisis de funcionalidad:**
- Panel de usuario con opción de cambio de contraseña
- URL con parámetro: `http://192.168.0.235/dashboard.php?id=5`
- Posible vector de SQL Injection en parámetro `id`

## ⚔️ Fase 3: Explotación

### 3.1 Análisis de SQL Injection

**URL objetivo:** `http://192.168.0.235/dashboard.php?id=5`

**Observación:** La manipulación directa desde el navegador no es efectiva, por lo que utilizaremos Burp Suite para interceptar y modificar las peticiones HTTP.

### 3.2 Configuración de Burp Suite

**Pasos de configuración:**
1. Iniciar Burp Suite Professional/Community
2. Configurar proxy del navegador (127.0.0.1:8080)
3. Interceptar peticiones al dashboard
4. Modificar parámetro `id` para testing de SQLi

**Payloads de prueba recomendados:**
```sql
-- Prueba básica de error
id=5'

-- Union-based injection
id=5 UNION SELECT 1,2,3,4--

-- Boolean-based blind
id=5 AND 1=1--
id=5 AND 1=2--

-- Time-based blind
id=5 AND (SELECT SLEEP(5))--
```

### 3.3 Metodología de Explotación Recomendada

**Pasos sugeridos:**
1. **Identificar tipo de inyección:** Error-based, Union-based, o Blind
2. **Enumerar base de datos:** Nombre, tablas, columnas
3. **Extraer datos sensibles:** Credenciales, hashes de contraseñas
4. **Buscar escalación:** Privilegios de DB, lectura de archivos del sistema
5. **Post-explotación:** Obtener shell, escalación de privilegios

## 📊 Estado Actual del Análisis

**🔍 Vulnerabilidades Confirmadas:**
- Potencial SQL Injection en parámetro `id`
- Cookie de sesión sin flag HttpOnly
- Aplicación web con autenticación personalizada

**🎯 Próximos Pasos:**
- Completar explotación de SQL Injection
- Obtener credenciales del sistema
- Intentar acceso SSH con credenciales encontradas
- Realizar escalación de privilegios en Ubuntu

## 🛡️ Recomendaciones de Mitigación

### Inmediatas (Críticas)
1. **Consultas preparadas:** Implementar prepared statements para todas las consultas SQL
2. **Validación de entrada:** Sanitizar todos los parámetros de entrada
3. **Configuración de cookies:** Añadir flags `HttpOnly` y `Secure`

### A corto plazo
1. **WAF (Web Application Firewall):** Implementar filtrado de ataques de inyección
2. **Logging y monitoreo:** Implementar detección de patrones de ataque
3. **Principio de menor privilegio:** Limitar permisos de usuario de base de datos

### A largo plazo
1. **Code review:** Auditoría completa del código de la aplicación
2. **Pentest regular:** Evaluaciones de seguridad periódicas
3. **Capacitación:** Formación del equipo en desarrollo seguro

## 🎓 Lecciones Aprendidas

1. **Reconocimiento meticuloso:** La enumeración completa es crucial para identificar vectores de ataque
2. **Herramientas especializadas:** Burp Suite es esencial para testing de aplicaciones web
3. **Metodología sistemática:** Seguir un proceso estructurado mejora la efectividad del pentest

## 🔗 Referencias

- [OWASP SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [Burp Suite Documentation](https://portswigger.net/burp/documentation)
- [SQLMap Usage Guide](https://sqlmap.org/)
- [Ubuntu Security Guide](https://ubuntu.com/security)

---
**Estado:** En desarrollo - Pendiente completar explotación  
**Elaborado por:** Elvis  
**Fecha:** 13 de octubre de 2025  
**Laboratorio:** DarkHole Pentesting Challenge
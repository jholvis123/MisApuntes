# Mis Apuntes de Bash Intermedio

*Tags: bash-intermedio, scripting, regex, arrays, funciones, debugging*

## Notas iniciales 

Ya me manejo bien con lo básico de bash, pero quiero subir de nivel. Estos son mis apuntes de cosas más avanzadas que voy aprendiendo.

**Lo que ya sé hacer:**
- Los comandos típicos (ls, cd, cp, etc.)
- Variables simples: `nombre="algo"` 
- If básicos y bucles for/while
- Redirección básica con > y |

**Test rápido para ver si estoy listo:**
```bash
for archivo in *.txt; do
    if [ -f "$archivo" ]; then
        echo "Procesando: $archivo"
    fi
done
```
Si entiendo esto (busca archivos .txt y muestra nombres), entonces sí puedo seguir.

**Lo que quiero aprender:**
- Regex (expresiones regulares) - para buscar patrones como un pro
- Arrays - listas de datos 
- Funciones más complejas
- Como debuggear cuando algo no funciona
- Optimizar scripts para que vayan rápido
- Automatizar tareas complicadas

---

## Expresiones Regulares (Regex)

Al principio pensé que eran súper complicadas, pero son como un "lenguaje especial" para buscar patrones en texto.

**Mi forma de entenderlo:** Es como si le dijeras a alguien:
- "Busca cosas que parezcan teléfonos" → `\d{3}-\d{3}-\d{4}` 
- "Busca cosas que parezcan emails" → algo@algo.algo

**¿Para qué me sirven?**
- Validar si un email está bien escrito
- Sacar números de teléfono de un documento
- Limpiar logs que están hechos un desastre
- Procesar archivos CSV o JSON
- Verificar que las contraseñas sean seguras

### Aprendiendo regex de a poco

**Lo más básico - buscar texto literal:**
```bash
grep "error" archivo.log
```
Esto busca exactamente la palabra "error". Nada más.

**Empezando con los caracteres especiales:**
```bash
# El punto . significa "cualquier carácter"
grep "er.or" archivo.log    # encuentra: error, ergot, erizor...

# El asterisco * significa "cero o más del anterior"  
grep "erro*r" archivo.log   # encuentra: error, errrror, eror...

# El + significa "uno o más del anterior"
grep "erro+r" archivo.log   # encuentra: error, errrror (pero NO eror)
```

**Nota:** El + no funciona en grep básico, necesitas `grep -E` o `egrep`

**Clases de caracteres (lo que más uso):**
```bash
# [aeiou] = cualquier vocal
grep "[aeiou]" archivo.txt  

# [0-9] = cualquier número
grep "[0-9]" archivo.txt    

# [A-Z] = cualquier mayúscula
grep "[A-Z]" archivo.txt    

# [^0-9] = todo EXCEPTO números (el ^ niega)
grep "[^0-9]" archivo.txt   
```

### Ejemplo real que me pasó

Tenía un archivo de log todo desordenado:
```
2023-10-13 14:30:25 INFO Usuario juan@email.com logueado
2023-10-13 14:31:10 ERROR Fallo en conexión IP: 192.168.1.100
2023-10-13 14:32:05 WARN Memoria baja: 85% utilizada
usuario@servidor:~$ comando inválido
2023-10-13 14:33:15 INFO Backup completado
```

Quería sacar solo las líneas que tenían fecha/hora correcta.

```bash
# Mi patrón: YYYY-MM-DD HH:MM:SS
grep "^[0-9]{4}-[0-9]{2}-[0-9]{2} [0-9]{2}:[0-9]{2}:[0-9]{2}" archivo.log
```

**Qué significa cada parte:**
- `^` = tiene que empezar la línea así
- `[0-9]{4}` = exactamente 4 números (el año)
- `-` = un guión
- `[0-9]{2}` = exactamente 2 números (mes)
- ` ` = un espacio
- `[0-9]{2}:[0-9]{2}:[0-9]{2}` = hora formato HH:MM:SS

**Nota:** Los {} no funcionan en grep normal, uso `grep -E` o `egrep`

### Usando sed para buscar y reemplazar

**Caso real:** Tenía números de teléfono en diferentes formatos:
```
(555) 123-4567
555-123-4567  
555.123.4567
```

Quería que todos quedaran como: 555-123-4567

```bash
# Primero quito los paréntesis
sed 's/[()]//g' telefonos.txt

# Después cambio puntos por guiones
sed 's/\./\-/g' telefonos.txt

# Los dos juntos:
sed 's/[()]//g; s/\./\-/g' telefonos.txt
```

**Sacar emails de un documento:**
```bash
# Patrón que aprendí para emails
grep -o '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' documento.txt
```

Lo que significa:
- `[a-zA-Z0-9._%+-]+` = letras, números y caracteres válidos antes del @
- `@` = la arroba
- `[a-zA-Z0-9.-]+` = el dominio
- `\.` = punto (hay que escaparlo con \)
- `[a-zA-Z]{2,}` = extensión (.com, .org, etc)

### Scripts útiles que hice

**Validador de contraseñas que hice para un proyecto:**
```bash
#!/bin/bash
# Me pidieron hacer un validador de contraseñas

validar_password() {
    local password="$1"
    
    # Mínimo 8 caracteres
    if ! [[ $password =~ .{8,} ]]; then
        echo "❌ Muy corta, mínimo 8 caracteres"
        return 1
    fi
    
    # Debe tener mayúscula
    if ! [[ $password =~ [A-Z] ]]; then
        echo "❌ Falta una mayúscula"
        return 1
    fi
    
    # Debe tener minúscula  
    if ! [[ $password =~ [a-z] ]]; then
        echo "❌ Falta una minúscula"
        return 1
    fi
    
    # Debe tener número
    if ! [[ $password =~ [0-9] ]]; then
        echo "❌ Falta un número"
        return 1
    fi
    
    # Debe tener carácter especial
    if ! [[ $password =~ [!@#$%^&*] ]]; then
        echo "❌ Falta un carácter especial (!@#$%^&*)"
        return 1
    fi
    
    echo "✅ Contraseña buena"
    return 0
}

# Como usarlo:
read -p "Dame tu contraseña: " -s password
echo
validar_password "$password"
```

#### Ejercicio 2: Procesador de Logs de Apache
```bash
#!/bin/bash
# analizar_logs.sh

LOG_FILE="/var/log/apache2/access.log"

echo "=== ANÁLISIS DE LOGS DE APACHE ==="

# Contar requests por IP
echo "Top 10 IPs más activas:"
grep -o '^[0-9.]*' "$LOG_FILE" | sort | uniq -c | sort -nr | head -10

# Buscar ataques 404
echo -e "\nPosibles ataques (404s frecuentes):"
grep ' 404 ' "$LOG_FILE" | grep -o '^[0-9.]*' | sort | uniq -c | sort -nr | head -5

# Buscar User Agents sospechosos
echo -e "\nUser Agents sospechosos:"
grep -i 'bot\|crawler\|spider' "$LOG_FILE" | grep -o '"[^"]*"$' | sort | uniq
```

---

## Arrays (listas)

### ¿Qué son?

Son como listas numeradas. Imagínate la lista del súper:
```
Lista[0] = Leche      (bash empieza en 0, no en 1!)
Lista[1] = Pan  
Lista[2] = Huevos
```

En bash:
```bash
lista=("Leche" "Pan" "Huevos")
echo ${lista[0]}  # dice: Leche
```

### Como crear arrays

```bash
# Forma 1: todo junto
frutas=("manzana" "banana" "naranja")

# Forma 2: uno por uno
frutas[0]="manzana"
frutas[1]="banana" 
frutas[2]="naranja"

# Forma 3: desde un comando (esto es útil)
archivos=($(ls *.txt))

# Forma 4: vacío para llenarlo después
declare -a nombres
```

### Como usar arrays

```bash
frutas=("manzana" "banana" "naranja" "uva")

# Un elemento específico
echo ${frutas[0]}        # manzana
echo ${frutas[2]}        # naranja

# Todos los elementos
echo ${frutas[@]}        # manzana banana naranja uva

# Cuántos elementos hay
echo ${#frutas[@]}       # 4

# Los números de índice
echo ${!frutas[@]}       # 0 1 2 3
```

### Modificar arrays

```bash
frutas=("manzana" "banana" "naranja")

# Agregar al final
frutas+=("uva")
echo ${frutas[@]}        

# Cambiar uno
frutas[1]="pera"
echo ${frutas[@]}        

# Borrar uno
unset frutas[2]
echo ${frutas[@]}        # quedan huecos en los índices!
```

**Nota importante:** Cuando borras elementos quedan "huecos" en los números.

### 🎯 Ejemplos Prácticos con Arrays

#### Ejemplo 1: Gestor de Tareas
```bash
#!/bin/bash
# gestor_tareas.sh

# Array para almacenar tareas
declare -a tareas

mostrar_menu() {
    echo "=== GESTOR DE TAREAS ==="
    echo "1. Añadir tarea"
    echo "2. Listar tareas"
    echo "3. Completar tarea"
    echo "4. Eliminar tarea"
    echo "5. Salir"
}

añadir_tarea() {
    read -p "Describe la tarea: " nueva_tarea
    tareas+=("$nueva_tarea")
    echo "✅ Tarea añadida: $nueva_tarea"
}

listar_tareas() {
    if [ ${#tareas[@]} -eq 0 ]; then
        echo "📝 No tienes tareas pendientes"
        return
    fi
    
    echo "📋 Tus tareas:"
    for i in "${!tareas[@]}"; do
        echo "$((i+1)). ${tareas[i]}"
    done
}

completar_tarea() {
    listar_tareas
    read -p "¿Qué tarea completaste? (número): " num
    
    if [[ $num =~ ^[0-9]+$ ]] && [ $num -ge 1 ] && [ $num -le ${#tareas[@]} ]; then
        indice=$((num-1))
        echo "🎉 ¡Completaste: ${tareas[indice]}!"
        unset tareas[indice]
        # Reindexar array
        tareas=("${tareas[@]}")
    else
        echo "❌ Número de tarea inválido"
    fi
}

# Loop principal
while true; do
    mostrar_menu
    read -p "Elige una opción: " opcion
    
    case $opcion in
        1) añadir_tarea ;;
        2) listar_tareas ;;
        3) completar_tarea ;;
        4) eliminar_tarea ;;
        5) echo "¡Hasta luego!"; exit 0 ;;
        *) echo "❌ Opción inválida" ;;
    esac
    echo
done
```

#### Ejemplo 2: Analizador de Archivos del Sistema
```bash
#!/bin/bash
# analizar_sistema.sh

# Arrays para diferentes tipos de datos
declare -a archivos_grandes
declare -a directorios_vacios
declare -a archivos_antiguos

echo "🔍 Analizando sistema..."

# Buscar archivos grandes (>100MB)
echo "Buscando archivos grandes..."
while IFS= read -r -d '' archivo; do
    archivos_grandes+=("$archivo")
done < <(find /home -size +100M -type f -print0 2>/dev/null)

# Buscar directorios vacíos
echo "Buscando directorios vacíos..."
while IFS= read -r directorio; do
    directorios_vacios+=("$directorio")
done < <(find /home -type d -empty 2>/dev/null)

# Buscar archivos no modificados en 1 año
echo "Buscando archivos antiguos..."
while IFS= read -r -d '' archivo; do
    archivos_antiguos+=("$archivo")
done < <(find /home -type f -mtime +365 -print0 2>/dev/null)

# Generar reporte
echo "=== REPORTE DEL SISTEMA ==="

echo "📁 Archivos grandes encontrados: ${#archivos_grandes[@]}"
if [ ${#archivos_grandes[@]} -gt 0 ]; then
    for archivo in "${archivos_grandes[@]:0:5}"; do  # Solo primeros 5
        tamaño=$(du -h "$archivo" | cut -f1)
        echo "  - $archivo ($tamaño)"
    done
fi

echo "📂 Directorios vacíos: ${#directorios_vacios[@]}"
if [ ${#directorios_vacios[@]} -gt 0 ]; then
    for directorio in "${directorios_vacios[@]:0:5}"; do
        echo "  - $directorio"
    done
fi

echo "🗓️ Archivos antiguos (>1 año): ${#archivos_antiguos[@]}"
```

### 🧪 Arrays Asociativos (Bash 4+)

**¿Qué son?** Arrays que usan texto como índice en lugar de números.

```bash
# Declarar array asociativo
declare -A usuarios

# Asignar valores
usuarios["admin"]="password123"
usuarios["guest"]="guest123"
usuarios["root"]="secreto"

# Acceder valores
echo ${usuarios["admin"]}    # password123

# Listar todas las claves
echo ${!usuarios[@]}         # admin guest root

# Listar todos los valores
echo ${usuarios[@]}          # password123 guest123 secreto
```

#### Ejemplo Práctico: Base de Datos Simple
```bash
#!/bin/bash
# base_datos_usuarios.sh

declare -A usuarios
declare -A emails

# Función para añadir usuario
añadir_usuario() {
    read -p "Nombre de usuario: " nombre
    read -p "Email: " email
    read -p "Contraseña: " -s password
    echo
    
    usuarios["$nombre"]="$password"
    emails["$nombre"]="$email"
    
    echo "✅ Usuario $nombre añadido"
}

# Función para listar usuarios
listar_usuarios() {
    echo "=== USUARIOS REGISTRADOS ==="
    for usuario in "${!usuarios[@]}"; do
        echo "👤 $usuario (${emails[$usuario]})"
    done
}

# Función para verificar login
verificar_login() {
    read -p "Usuario: " nombre
    read -p "Contraseña: " -s password
    echo
    
    if [[ ${usuarios[$nombre]} == "$password" ]]; then
        echo "✅ Login exitoso. Bienvenido $nombre"
    else
        echo "❌ Credenciales incorrectas"
    fi
}
```

## 🔧 Capítulo 3: Funciones Avanzadas - Código Modular y Reutilizable

### 🤔 ¿Por Qué Funciones Avanzadas?

**Piénsalo así:** Las funciones son como "recetas de cocina" para tu código:
- 📝 **Receta básica:** "Hacer café" (función simple)
- 👨‍🍳 **Receta gourmet:** "Hacer café con opciones personalizadas" (función avanzada)

### Cómo hacer funciones bien hechas

```bash
# Así hago mis funciones cuando quiero que funcionen bien
mi_funcion() {
    # 1. Variables locales (no afectan variables globales)
    local parametro1="$1"
    local parametro2="$2"
    local resultado=""
    
    # 2. Validación de parámetros
    if [ $# -lt 2 ]; then
        echo "Error: Se requieren 2 parámetros" >&2
        return 1
    fi
    
    # 3. Lógica principal
    resultado="Procesando $parametro1 y $parametro2"
    
    # 4. Salida (return o echo)
    echo "$resultado"
    return 0
}
```

### Sistema de logs que hice

```bash
#!/bin/bash
# logger.sh - para escribir logs decentes

# Variables globales para configuración
LOG_FILE="${LOG_FILE:-/var/log/mi_app.log}"
LOG_LEVEL="${LOG_LEVEL:-INFO}"

# Función para obtener timestamp
get_timestamp() {
    date '+%Y-%m-%d %H:%M:%S'
}

# Función principal de logging
log_message() {
    local level="$1"
    local message="$2"
    local timestamp=$(get_timestamp)
    local script_name=$(basename "$0")
    
    # Validar parámetros
    if [ $# -lt 2 ]; then
        echo "Error: log_message requiere nivel y mensaje" >&2
        return 1
    fi
    
    # Formato del mensaje
    local formatted_message="[$timestamp] [$level] [$script_name] $message"
    
    # Escribir al archivo y mostrar en pantalla si es error
    echo "$formatted_message" >> "$LOG_FILE"
    
    if [ "$level" = "ERROR" ] || [ "$level" = "CRITICAL" ]; then
        echo "$formatted_message" >&2
    fi
    
    return 0
}

# Funciones wrapper para diferentes niveles
log_info() {
    log_message "INFO" "$1"
}

log_warning() {
    log_message "WARNING" "$1"
}

log_error() {
    log_message "ERROR" "$1"
}

log_debug() {
    if [ "$LOG_LEVEL" = "DEBUG" ]; then
        log_message "DEBUG" "$1"
    fi
}

# Función para rotar logs
rotar_logs() {
    local max_size="$1"
    local backup_count="$2"
    
    # Valores por defecto
    max_size="${max_size:-10485760}"  # 10MB
    backup_count="${backup_count:-5}"
    
    if [ -f "$LOG_FILE" ]; then
        local file_size=$(stat -f%z "$LOG_FILE" 2>/dev/null || stat -c%s "$LOG_FILE" 2>/dev/null)
        
        if [ "$file_size" -gt "$max_size" ]; then
            # Mover logs antiguos
            for ((i=$backup_count; i>=1; i--)); do
                if [ -f "${LOG_FILE}.$i" ]; then
                    mv "${LOG_FILE}.$i" "${LOG_FILE}.$((i+1))"
                fi
            done
            
            # Mover log actual
            mv "$LOG_FILE" "${LOG_FILE}.1"
            touch "$LOG_FILE"
            
            log_info "Log rotado. Nuevo archivo de log creado."
        fi
    fi
}

# Ejemplo de uso
main() {
    log_info "Aplicación iniciada"
    log_debug "Modo debug activado"
    
    # Simular algún procesamiento
    if ! procesar_datos; then
        log_error "Fallo al procesar datos"
        return 1
    fi
    
    log_info "Procesamiento completado exitosamente"
    rotar_logs
}

procesar_datos() {
    # Simulación de procesamiento
    log_info "Iniciando procesamiento de datos"
    sleep 2
    log_info "Datos procesados correctamente"
    return 0
}

# Ejecutar si es llamado directamente
if [ "${BASH_SOURCE[0]}" = "${0}" ]; then
    main "$@"
fi
```

### 🔄 Funciones con Manejo de Errores Avanzado

```bash
# Función con múltiples tipos de salida
procesar_archivo() {
    local archivo="$1"
    local operacion="$2"
    local resultado=""
    
    # Validaciones detalladas
    if [ $# -ne 2 ]; then
        echo "Error: Uso incorrecto. procesar_archivo <archivo> <operacion>" >&2
        return 64  # Código de error específico
    fi
    
    if [ ! -f "$archivo" ]; then
        echo "Error: El archivo '$archivo' no existe" >&2
        return 65
    fi
    
    if [ ! -r "$archivo" ]; then
        echo "Error: Sin permisos de lectura para '$archivo'" >&2
        return 66
    fi
    
    # Procesamiento según operación
    case "$operacion" in
        "contar")
            resultado=$(wc -l < "$archivo")
            echo "Líneas en $archivo: $resultado"
            return 0
            ;;
        "validar")
            if grep -q "ERROR" "$archivo"; then
                echo "Archivo contiene errores"
                return 1
            else
                echo "Archivo válido"
                return 0
            fi
            ;;
        *)
            echo "Error: Operación '$operacion' no soportada" >&2
            return 67
            ;;
    esac
}

# Función que maneja múltiples archivos
procesar_lote() {
    local operacion="$1"
    shift  # Elimina el primer parámetro, quedando solo archivos
    local archivos=("$@")
    
    local exitosos=0
    local fallidos=0
    local total=${#archivos[@]}
    
    echo "Procesando $total archivo(s) con operación: $operacion"
    
    for archivo in "${archivos[@]}"; do
        echo -n "Procesando $archivo... "
        
        if procesar_archivo "$archivo" "$operacion" >/dev/null 2>&1; then
            echo "✅ OK"
            ((exitosos++))
        else
            echo "❌ FALLO"
            ((fallidos++))
        fi
    done
    
    echo "=== RESUMEN ==="
    echo "Total: $total"
    echo "Exitosos: $exitosos"
    echo "Fallidos: $fallidos"
    
    # Retornar código basado en resultados
    if [ $fallidos -eq 0 ]; then
        return 0  # Todo OK
    elif [ $exitosos -gt 0 ]; then
        return 1  # Algunos fallaron
    else
        return 2  # Todos fallaron
    fi
}

---

## Debugging (encontrar errores)

### Por qué es importante

La verdad es que la mayoría del tiempo lo paso arreglando errores, no escribiendo código nuevo. Al principio me frustraba mucho cuando algo no funcionaba, pero después aprendí estas técnicas.

### Trucos para debugging

**El comando `set` es tu mejor amigo:**

```bash
#!/bin/bash
# Estas líneas al principio de tus scripts te salvan la vida

set -e    # Para si cualquier comando falla
set -u    # Para si usas una variable que no existe
set -o pipefail  # Para si algo en un pipe falla
set -x    # Muestra cada comando antes de ejecutarlo

# Todo junto: set -euxo pipefail

echo "Empezando..."
archivo="datos.txt"

# Si el archivo no existe, se para aquí (por el set -e)
lineas=$(wc -l < "$archivo")
echo "El archivo tiene $lineas líneas"
```

**Debugging que puedes prender y apagar:**

```bash
#!/bin/bash
# Script con debugging inteligente

DEBUG=${DEBUG:-0}  # Variable de entorno, default 0

debug_log() {
    if [ "$DEBUG" -eq 1 ]; then
        echo "[DEBUG $(date '+%H:%M:%S')] $*" >&2
    fi
}

procesar_datos() {
    local archivo="$1"
    debug_log "Procesando archivo: $archivo"
    
    if [ ! -f "$archivo" ]; then
        debug_log "ERROR: Archivo no encontrado"
        return 1
    fi
    
    local lineas=$(wc -l < "$archivo")
    debug_log "Archivo tiene $lineas líneas"
    
    # Procesamiento...
    debug_log "Procesamiento completado"
    return 0
}

# Usar así:
# DEBUG=1 ./mi_script.sh    # Con debug
# ./mi_script.sh            # Sin debug
```

**Midiendo qué tan rápido va tu script:**

```bash
#!/bin/bash
# Midiendo tiempo de ejecución de funciones

time_function() {
    local func_name="$1"
    shift
    
    echo "⏱️ Ejecutando: $func_name"
    local start_time=$(date +%s.%N)
    
    # Ejecutar la función con sus parámetros
    "$func_name" "$@"
    local exit_code=$?
    
    local end_time=$(date +%s.%N)
    local duration=$(echo "$end_time - $start_time" | bc)
    
    printf "⏰ %s completada en %.3f segundos\n" "$func_name" "$duration"
    return $exit_code
}

# Función de ejemplo que toma tiempo
procesar_archivo_lento() {
    local archivo="$1"
    # Simulación de procesamiento lento
    grep -r "patron" "$archivo" | sort | uniq > resultado.tmp
    return 0
}

# Uso
time_function procesar_archivo_lento "archivo_grande.txt"
```

**Atrapar errores antes de que rompan todo:**

```bash
#!/bin/bash
# Limpieza automática cuando algo sale mal

# Variables globales para limpieza
TEMP_FILES=()
CHILD_PIDS=()

# Función de limpieza
cleanup() {
    local exit_code=$?
    
    echo "🧹 Realizando limpieza..."
    
    # Eliminar archivos temporales
    for temp_file in "${TEMP_FILES[@]}"; do
        if [ -f "$temp_file" ]; then
            rm -f "$temp_file"
            echo "  Eliminado: $temp_file"
        fi
    done
    
    # Terminar procesos hijos
    for pid in "${CHILD_PIDS[@]}"; do
        if kill -0 "$pid" 2>/dev/null; then
            kill "$pid"
            echo "  Terminado proceso: $pid"
        fi
    done
    
    echo "Limpieza completada. Saliendo con código: $exit_code"
    exit $exit_code
}

# Configurar traps para diferentes señales
trap cleanup EXIT      # Cuando el script termina normalmente
trap cleanup INT       # Ctrl+C
trap cleanup TERM      # kill
trap cleanup ERR       # Cuando hay error (requiere set -e)

set -e  # Necesario para que funcione trap ERR

# Función que registra archivos temporales
create_temp_file() {
    local temp_file=$(mktemp)
    TEMP_FILES+=("$temp_file")
    echo "$temp_file"
}

# Función que registra procesos
start_background_process() {
    local command="$1"
    $command &
    local pid=$!
    CHILD_PIDS+=($pid)
    echo "Proceso iniciado: $pid"
    return 0
}

# Uso del script
main() {
    echo "Iniciando procesamiento..."
    
    # Crear archivo temporal
    local temp_data=$(create_temp_file)
    echo "Datos temporales" > "$temp_data"
    
    # Iniciar proceso en background
    start_background_process "sleep 30"
    
    # Simular trabajo que puede fallar
    echo "Procesando..."
    sleep 2
    
    # Simular error (quita el comentario para probar)
    # false
    
    echo "✅ Procesamiento completado"
}

main "$@"
```

### Hacer que tus scripts vayan más rápido

**No uses comandos lentos:**

```bash
# 🐌 Lento - hace subshells
for archivo in $(ls *.txt); do
    echo "Procesando: $archivo"
done

# 🚀 Rápido - usa patrones directos  
for archivo in *.txt; do
    echo "Procesando: $archivo"
done

# 🐌 Lento - demasiados comandos
resultado=$(echo "$variable" | tr '[:lower:]' '[:upper:]')

# 🚀 Rápido - usa expansión de bash
resultado="${variable^^}"  # convierte a mayúsculas
```

**Procesar varios archivos a la vez:**

```bash
#!/bin/bash
# Procesar archivos en paralelo

MAX_JOBS=${MAX_JOBS:-4}  # cuántos procesos a la vez

procesar_archivo() {
    local archivo="$1"
    echo "Procesando: $archivo"
    
    # hacer el trabajo pesado
    grep -i "error" "$archivo" > "${archivo}.errors" 2>/dev/null
    wc -l "$archivo" > "${archivo}.count"
    
    echo "Terminé: $archivo"
}

# Función para manejar varios trabajos
procesar_en_paralelo() {
    local archivos=("$@")
    local pids=()
    
    for archivo in "${archivos[@]}"; do
        # Si ya hay muchos trabajos, esperar
        while [ ${#pids[@]} -ge $MAX_JOBS ]; do
            for i in "${!pids[@]}"; do
                if ! kill -0 "${pids[i]}" 2>/dev/null; then
                    unset pids[i]
                fi
            done
            pids=("${pids[@]}")  
            sleep 0.1
        done
        
        # Empezar nuevo trabajo en segundo plano
        procesar_archivo "$archivo" &
        pids+=($!)
        echo "Empezando: $archivo (PID: $!)"
    done
    
    # Esperar que terminen todos
    for pid in "${pids[@]}"; do
        wait "$pid"
    done
    
    echo "Todos terminaron"
}

# Para usarlo
archivos_a_procesar=(*.log)
procesar_en_paralelo "${archivos_a_procesar[@]}"
```

**No llenes la memoria:**

```bash
#!/bin/bash
# Para archivos grandes

# 🐌 Malo - carga todo el archivo
contenido=$(cat archivo_gigante.txt)
echo "$contenido" | grep "patron"

# 🚀 Mejor - lee línea por línea
while IFS= read -r linea; do
    if [[ $linea == *"patron"* ]]; then
        echo "$linea"
    fi
done < archivo_gigante.txt

# ✅ MEJOR - Usa herramientas optimizadas
grep "patron" archivo_gigante.txt
```

---

## Proyectos para practicar todo

### Proyecto: Monitor de servidor

Este es un script que hice para monitorear un servidor. Combina todo lo que hemos visto.

```bash
#!/bin/bash
# monitor_servidor.sh - Sistema completo de monitoreo

# Configuración
CONFIG_FILE="${HOME}/.monitor_config"
LOG_FILE="${HOME}/monitor.log"
ALERT_EMAIL="${ALERT_EMAIL:-admin@empresa.com}"

# Umbrales por defecto
CPU_THRESHOLD=80
MEMORY_THRESHOLD=90
DISK_THRESHOLD=85
LOAD_THRESHOLD=2.0

# Cargar configuración si existe
if [ -f "$CONFIG_FILE" ]; then
    source "$CONFIG_FILE"
fi

# Para escribir logs
log() {
    local level="$1"
    local message="$2"
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    echo "[$timestamp] [$level] $message" | tee -a "$LOG_FILE"
}

# Para mandar alertas cuando algo anda mal
send_alert() {
    local subject="$1"
    local message="$2"
    
    # Mandar email si está configurado
    if command -v mail >/dev/null; then
        echo "$message" | mail -s "$subject" "$ALERT_EMAIL"
    fi
    
    # Log local
    log "ALERT" "$subject: $message"
    
    # Notificación desktop si está disponible
    if command -v notify-send >/dev/null && [ -n "$DISPLAY" ]; then
        notify-send "Monitor Servidor" "$subject"
    fi
}

# Monitoreo de CPU
check_cpu() {
    local cpu_usage=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | sed 's/%us,//')
    cpu_usage=${cpu_usage%.*}  # Eliminar decimales
    
    log "INFO" "CPU Usage: ${cpu_usage}%"
    
    if [ "$cpu_usage" -gt "$CPU_THRESHOLD" ]; then
        send_alert "CPU Alta" "Uso de CPU: ${cpu_usage}% (umbral: ${CPU_THRESHOLD}%)"
        return 1
    fi
    return 0
}

# Monitoreo de memoria
check_memory() {
    local memory_info=$(free | grep MemAvailable | awk '{print ($1/$2) * 100.0}')
    local memory_usage=${memory_info%.*}
    
    log "INFO" "Memory Usage: ${memory_usage}%"
    
    if [ "$memory_usage" -gt "$MEMORY_THRESHOLD" ]; then
        send_alert "Memoria Alta" "Uso de memoria: ${memory_usage}% (umbral: ${MEMORY_THRESHOLD}%)"
        return 1
    fi
    return 0
}

# Monitoreo de disco
check_disk() {
    local alert_sent=0
    
    while read -r filesystem size used avail use_percent mount; do
        # Extraer solo el número del porcentaje
        use_num=${use_percent%?}
        
        log "INFO" "Disk $mount: ${use_percent} used"
        
        if [ "$use_num" -gt "$DISK_THRESHOLD" ]; then
            send_alert "Disco Lleno" "Disco $mount: ${use_percent} usado (umbral: ${DISK_THRESHOLD}%)"
            alert_sent=1
        fi
    done < <(df -h | grep -E '^/dev/')
    
    return $alert_sent
}

# Monitoreo de servicios críticos
check_services() {
    local services=("ssh" "nginx" "mysql" "postgresql")
    local failed_services=()
    
    for service in "${services[@]}"; do
        if systemctl is-active --quiet "$service" 2>/dev/null; then
            log "INFO" "Service $service: OK"
        else
            log "WARNING" "Service $service: FAILED"
            failed_services+=("$service")
        fi
    done
    
    if [ ${#failed_services[@]} -gt 0 ]; then
        local services_list=$(IFS=', '; echo "${failed_services[*]}")
        send_alert "Servicios Caídos" "Servicios no disponibles: $services_list"
        return 1
    fi
    return 0
}

# Generar reporte
generate_report() {
    local report_file="${HOME}/monitor_report_$(date +%Y%m%d_%H%M%S).html"
    
    cat > "$report_file" << EOF
<!DOCTYPE html>
<html>
<head>
    <title>Reporte de Monitoreo - $(date)</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .ok { color: green; }
        .warning { color: orange; }
        .critical { color: red; }
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #f2f2f2; }
    </style>
</head>
<body>
    <h1>Reporte de Monitoreo del Sistema</h1>
    <p><strong>Fecha:</strong> $(date)</p>
    <p><strong>Servidor:</strong> $(hostname)</p>
    
    <h2>Estado General</h2>
    <table>
        <tr><th>Métrica</th><th>Valor</th><th>Estado</th></tr>
        <tr><td>CPU</td><td>$(top -bn1 | grep "Cpu(s)" | awk '{print $2}')</td><td class="ok">OK</td></tr>
        <tr><td>Memoria</td><td>$(free -h | grep Mem | awk '{print $3 "/" $2}')</td><td class="ok">OK</td></tr>
        <tr><td>Disco (/)</td><td>$(df -h / | tail -1 | awk '{print $5}')</td><td class="ok">OK</td></tr>
        <tr><td>Uptime</td><td>$(uptime -p)</td><td class="ok">OK</td></tr>
    </table>
    
    <h2>Procesos Top CPU</h2>
    <pre>$(ps aux --sort=-%cpu | head -10)</pre>
    
    <h2>Log Reciente</h2>
    <pre>$(tail -20 "$LOG_FILE")</pre>
    
</body>
</html>
EOF
    
    log "INFO" "Reporte generado: $report_file"
    echo "$report_file"
}

# Función principal
main() {
    local mode="${1:-check}"
    
    case "$mode" in
        "check")
            log "INFO" "Iniciando chequeo de monitoreo"
            
            check_cpu
            check_memory  
            check_disk
            check_services
            
            log "INFO" "Chequeo completado"
            ;;
        "report")
            generate_report
            ;;
        "daemon")
            log "INFO" "Iniciando modo daemon"
            while true; do
                main check
                sleep 300  # Chequear cada 5 minutos
            done
            ;;
        *)
            echo "Uso: $0 {check|report|daemon}"
            echo "  check  - Ejecutar chequeo único"
            echo "  report - Generar reporte HTML"
            echo "  daemon - Ejecutar continuamente"
            exit 1
            ;;
    esac
}

# Configurar trap para limpieza
trap 'log "INFO" "Monitor detenido"; exit 0' INT TERM

# Ejecutar
main "$@"
```

### Proyecto: Script de backups automáticos

```bash
#!/bin/bash
# backup_inteligente.sh - Sistema de backup con rotación y compresión

# Configuración avanzada
declare -A CONFIG=(
    ["source_dirs"]="/home /etc /var/www"
    ["backup_dest"]="/backups"
    ["retention_days"]="30"
    ["compression"]="gzip"  # gzip, bzip2, xz
    ["encryption"]="false"
    ["max_size_gb"]="100"
    ["exclude_patterns"]="*.tmp *.log *.cache"
    ["notification_email"]=""
    ["parallel_jobs"]="2"
)

# Arrays para tracking
declare -a BACKUP_JOBS=()
declare -a TEMP_FILES=()
declare -a SUCCESS_BACKUPS=()
declare -a FAILED_BACKUPS=()

# Cargar configuración personalizada
load_config() {
    local config_file="${HOME}/.backup_config"
    
    if [ -f "$config_file" ]; then
        source "$config_file"
        log "INFO" "Configuración cargada desde $config_file"
    fi
}

# Sistema de logging avanzado
log() {
    local level="$1"
    local message="$2"
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    local log_file="${CONFIG[backup_dest]}/backup.log"
    
    # Crear directorio de log si no existe
    mkdir -p "$(dirname "$log_file")"
    
    # Formatear mensaje
    local formatted="[$timestamp] [$level] [$$] $message"
    
    # Escribir a archivo y stdout
    echo "$formatted" | tee -a "$log_file"
    
    # Enviar a syslog si está disponible
    if command -v logger >/dev/null; then
        logger -t "backup_inteligente" "$level: $message"
    fi
}

# Validación de prerequisitos
validate_environment() {
    local errors=0
    
    # Verificar comandos necesarios
    local required_commands=("tar" "find" "du" "df")
    for cmd in "${required_commands[@]}"; do
        if ! command -v "$cmd" >/dev/null; then
            log "ERROR" "Comando requerido no encontrado: $cmd"
            ((errors++))
        fi
    done
    
    # Verificar directorio destino
    if [ ! -d "${CONFIG[backup_dest]}" ]; then
        log "INFO" "Creando directorio de backup: ${CONFIG[backup_dest]}"
        if ! mkdir -p "${CONFIG[backup_dest]}"; then
            log "ERROR" "No se puede crear directorio: ${CONFIG[backup_dest]}"
            ((errors++))
        fi
    fi
    
    # Verificar espacio disponible
    local available_gb=$(df -BG "${CONFIG[backup_dest]}" | tail -1 | awk '{print $4}' | sed 's/G//')
    if [ "$available_gb" -lt "${CONFIG[max_size_gb]}" ]; then
        log "WARNING" "Espacio disponible ($available_gb GB) menor que límite (${CONFIG[max_size_gb]} GB)"
    fi
    
    return $errors
}

# Crear archivo de exclusiones
create_exclude_file() {
    local exclude_file=$(mktemp)
    TEMP_FILES+=("$exclude_file")
    
    # Patrones comunes de exclusión
    cat > "$exclude_file" << EOF
*.tmp
*.log
*.cache
*/.cache/*
*/tmp/*
*/temp/*
*.pid
*.lock
*~
.DS_Store
Thumbs.db
EOF
    
    # Añadir patrones personalizados
    IFS=' ' read -ra PATTERNS <<< "${CONFIG[exclude_patterns]}"
    for pattern in "${PATTERNS[@]}"; do
        echo "$pattern" >> "$exclude_file"
    done
    
    echo "$exclude_file"
}

# Función de backup para un directorio
backup_directory() {
    local source_dir="$1"
    local backup_name="$2"
    local exclude_file="$3"
    
    log "INFO" "Iniciando backup de: $source_dir"
    
    # Verificar que el directorio existe
    if [ ! -d "$source_dir" ]; then
        log "ERROR" "Directorio fuente no existe: $source_dir"
        FAILED_BACKUPS+=("$source_dir")
        return 1
    fi
    
    # Calcular tamaño del directorio
    local size_mb=$(du -sm "$source_dir" 2>/dev/null | cut -f1)
    log "INFO" "Tamaño a respaldar: ${size_mb}MB"
    
    # Nombre del archivo de backup
    local timestamp=$(date +%Y%m%d_%H%M%S)
    local backup_file="${CONFIG[backup_dest]}/${backup_name}_${timestamp}.tar"
    
    # Añadir extensión según compresión
    case "${CONFIG[compression]}" in
        "gzip") backup_file="${backup_file}.gz"; local tar_opts="-czf" ;;
        "bzip2") backup_file="${backup_file}.bz2"; local tar_opts="-cjf" ;;
        "xz") backup_file="${backup_file}.xz"; local tar_opts="-cJf" ;;
        *) local tar_opts="-cf" ;;
    esac
    
    # Crear backup con barra de progreso
    log "INFO" "Creando archivo: $(basename "$backup_file")"
    
    if tar $tar_opts "$backup_file" \
        --exclude-from="$exclude_file" \
        --ignore-failed-read \
        -C "$(dirname "$source_dir")" \
        "$(basename "$source_dir")" 2>&1 | while IFS= read -r line; do
        log "DEBUG" "TAR: $line"
    done; then
        
        # Verificar integridad
        log "INFO" "Verificando integridad del backup..."
        if tar -tf "$backup_file" >/dev/null 2>&1; then
            local backup_size=$(du -sh "$backup_file" | cut -f1)
            log "SUCCESS" "Backup completado: $backup_file ($backup_size)"
            SUCCESS_BACKUPS+=("$backup_file")
            
            # Encriptar si está habilitado
            if [ "${CONFIG[encryption]}" = "true" ]; then
                encrypt_backup "$backup_file"
            fi
            
            return 0
        else
            log "ERROR" "Backup corrupto: $backup_file"
            rm -f "$backup_file"
            FAILED_BACKUPS+=("$source_dir")
            return 1
        fi
    else
        log "ERROR" "Fallo al crear backup de: $source_dir"
        FAILED_BACKUPS+=("$source_dir")
        return 1
    fi
}

# Encriptación de backup
encrypt_backup() {
    local backup_file="$1"
    local encrypted_file="${backup_file}.gpg"
    
    log "INFO" "Encriptando backup: $(basename "$backup_file")"
    
    if command -v gpg >/dev/null; then
        # Usar clave simétrica (requiere contraseña)
        if gpg --symmetric --cipher-algo AES256 --output "$encrypted_file" "$backup_file"; then
            log "SUCCESS" "Backup encriptado: $(basename "$encrypted_file")"
            # Eliminar archivo sin encriptar
            rm -f "$backup_file"
            # Actualizar array de éxitos
            SUCCESS_BACKUPS=("${SUCCESS_BACKUPS[@]/$backup_file/$encrypted_file}")
        else
            log "ERROR" "Fallo al encriptar: $backup_file"
        fi
    else
        log "WARNING" "GPG no disponible, saltando encriptación"
    fi
}

# Rotación de backups antiguos
rotate_backups() {
    local retention_days="${CONFIG[retention_days]}"
    
    log "INFO" "Iniciando rotación de backups (retención: $retention_days días)"
    
    local deleted_count=0
    local freed_space=0
    
    # Buscar backups antiguos
    while IFS= read -r -d '' old_backup; do
        local file_size=$(du -sb "$old_backup" | cut -f1)
        freed_space=$((freed_space + file_size))
        
        log "INFO" "Eliminando backup antiguo: $(basename "$old_backup")"
        rm -f "$old_backup"
        ((deleted_count++))
        
    done < <(find "${CONFIG[backup_dest]}" -name "*.tar*" -mtime +$retention_days -print0)
    
    if [ $deleted_count -gt 0 ]; then
        local freed_mb=$((freed_space / 1024 / 1024))
        log "INFO" "Rotación completada: $deleted_count archivos eliminados, ${freed_mb}MB liberados"
    else
        log "INFO" "No hay backups antiguos para eliminar"
    fi
}

# Función principal de backup
run_backup() {
    log "INFO" "=== INICIANDO PROCESO DE BACKUP ==="
    
    # Validar ambiente
    if ! validate_environment; then
        log "ERROR" "Validación de ambiente falló"
        return 1
    fi
    
    # Crear archivo de exclusiones
    local exclude_file=$(create_exclude_file)
    
    # Obtener lista de directorios a respaldar
    IFS=' ' read -ra SOURCE_DIRS <<< "${CONFIG[source_dirs]}"
    
    # Procesar cada directorio
    local pids=()
    for source_dir in "${SOURCE_DIRS[@]}"; do
        # Control de trabajos paralelos
        while [ ${#pids[@]} -ge "${CONFIG[parallel_jobs]}" ]; do
            for i in "${!pids[@]}"; do
                if ! kill -0 "${pids[i]}" 2>/dev/null; then
                    wait "${pids[i]}"
                    unset pids[i]
                fi
            done
            pids=("${pids[@]}")  # Reindexar
            sleep 1
        done
        
        # Crear nombre de backup basado en el directorio
        local backup_name=$(echo "$source_dir" | tr '/' '_' | sed 's/^_//')
        
        # Iniciar backup en background
        backup_directory "$source_dir" "$backup_name" "$exclude_file" &
        pids+=($!)
        log "INFO" "Backup iniciado para $source_dir (PID: $!)"
    done
    
    # Esperar a que terminen todos los trabajos
    for pid in "${pids[@]}"; do
        wait "$pid"
    done
    
    # Limpiar archivos temporales
    for temp_file in "${TEMP_FILES[@]}"; do
        rm -f "$temp_file"
    done
    
    # Rotar backups antiguos
    rotate_backups
    
    # Generar reporte final
    generate_backup_report
    
    log "INFO" "=== PROCESO DE BACKUP COMPLETADO ==="
}

# Generar reporte de backup
generate_backup_report() {
    local report_file="${CONFIG[backup_dest]}/backup_report_$(date +%Y%m%d_%H%M%S).html"
    
    log "INFO" "Generando reporte: $report_file"
    
    cat > "$report_file" << EOF
<!DOCTYPE html>
<html>
<head>
    <title>Reporte de Backup - $(date)</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .success { color: green; }
        .error { color: red; }
        .warning { color: orange; }
        table { border-collapse: collapse; width: 100%; margin: 20px 0; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #f2f2f2; }
        .summary { background-color: #f9f9f9; padding: 15px; border-radius: 5px; }
    </style>
</head>
<body>
    <h1>Reporte de Backup</h1>
    
    <div class="summary">
        <h2>Resumen</h2>
        <p><strong>Fecha:</strong> $(date)</p>
        <p><strong>Servidor:</strong> $(hostname)</p>
        <p><strong>Backups Exitosos:</strong> <span class="success">${#SUCCESS_BACKUPS[@]}</span></p>
        <p><strong>Backups Fallidos:</strong> <span class="error">${#FAILED_BACKUPS[@]}</span></p>
    </div>
    
    <h2>Backups Exitosos</h2>
    <table>
        <tr><th>Archivo</th><th>Tamaño</th><th>Fecha</th></tr>
EOF
    
    for backup in "${SUCCESS_BACKUPS[@]}"; do
        if [ -f "$backup" ]; then
            local size=$(du -sh "$backup" | cut -f1)
            local date=$(stat -c %y "$backup" 2>/dev/null || stat -f %Sm "$backup" 2>/dev/null)
            echo "        <tr><td>$(basename "$backup")</td><td>$size</td><td>$date</td></tr>" >> "$report_file"
        fi
    done
    
    cat >> "$report_file" << EOF
    </table>
    
    <h2>Backups Fallidos</h2>
    <ul>
EOF
    
    for failed in "${FAILED_BACKUPS[@]}"; do
        echo "        <li class=\"error\">$failed</li>" >> "$report_file"
    done
    
    cat >> "$report_file" << EOF
    </ul>
    
    <h2>Espacio en Disco</h2>
    <pre>$(df -h "${CONFIG[backup_dest]}")</pre>
    
    <h2>Log del Proceso</h2>
    <pre>$(tail -50 "${CONFIG[backup_dest]}/backup.log")</pre>
    
</body>
</html>
EOF
    
    log "SUCCESS" "Reporte generado: $report_file"
    
    # Enviar por email si está configurado
    if [ -n "${CONFIG[notification_email]}" ] && command -v mail >/dev/null; then
        echo "Reporte de backup adjunto" | mail -s "Backup Report - $(date)" -A "$report_file" "${CONFIG[notification_email]}"
        log "INFO" "Reporte enviado por email a: ${CONFIG[notification_email]}"
    fi
}

# Función principal
main() {
    local action="${1:-backup}"
    
    # Cargar configuración
    load_config
    
    case "$action" in
        "backup")
            run_backup
            ;;
        "restore")
            echo "Función de restauración en desarrollo..."
            ;;
        "config")
            echo "Configuración actual:"
            for key in "${!CONFIG[@]}"; do
                echo "$key = ${CONFIG[$key]}"
            done
            ;;
        *)
            echo "Uso: $0 {backup|restore|config}"
            exit 1
            ;;
    esac
}

# Configurar limpieza al salir
cleanup() {
    log "INFO" "Limpiando archivos temporales..."
    for temp_file in "${TEMP_FILES[@]}"; do
        rm -f "$temp_file"
    done
}

trap cleanup EXIT INT TERM

# Ejecutar
main "$@"
```

## 🎓 Capítulo 6: Recursos y Próximos Pasos

### 📚 Libros y Documentación Recomendada

1. **"Advanced Bash-Scripting Guide"** - Mendel Cooper (gratuito online)
2. **"Learning the bash Shell"** - Cameron Newham
3. **"Bash Pocket Reference"** - Arnold Robbins

### 🌐 Recursos Online Esenciales

- **ShellCheck.net**: Validador de scripts online
- **Explainshell.com**: Explica comandos complejos
- **Bash Hackers Wiki**: bashblackers.org
- **GNU Bash Manual**: gnu.org/manual/bash

### 🏆 Certificaciones y Competencias

- **LPIC-1 (Linux Professional Institute)**
- **CompTIA Linux+**
- **Red Hat Certified System Administrator (RHCSA)**

### Proyecto final: Mi propio framework

Combina todo lo aprendido creando tu propio framework de scripts:

```bash
#!/bin/bash
# mi_framework.sh - Template para todos tus futuros scripts

# Metadata del script
readonly SCRIPT_NAME=$(basename "$0")
readonly SCRIPT_DIR=$(cd "$(dirname "$0")" && pwd)
readonly SCRIPT_VERSION="1.0.0"
readonly SCRIPT_AUTHOR="Tu Nombre"

# Configuración global
DEBUG=${DEBUG:-0}
VERBOSE=${VERBOSE:-0}
DRY_RUN=${DRY_RUN:-0}

# Incluir librerías comunes
source "${SCRIPT_DIR}/lib/logging.sh" 2>/dev/null || {
    echo "Error: No se pueden cargar librerías"
    exit 1
}

# Tu código aquí...
main() {
    log_info "Iniciando $SCRIPT_NAME v$SCRIPT_VERSION"
    
    # Tu lógica principal
    
    log_info "Script completado exitosamente"
}

# Ejecutar solo si es llamado directamente
if [[ "${BASH_SOURCE[0]}" == "${0}" ]]; then
    main "$@"
fi
```

---

## Lo que aprendí

Después de esto ya puedo:

- ✅ Hacer scripts que no se rompen con cualquier cosa
- ✅ Encontrar errores rápido cuando algo no funciona  
- ✅ Automatizar las cosas aburridas del trabajo
- ✅ Hacer que mis scripts vayan más rápido
- ✅ Crear herramientas que otros puedan usar

**Nota personal:** Esto es como aprender un idioma. Tienes que practicar seguido para no olvidarlo.

**Lo que sigue:** Bash avanzado, donde voy a aprender sobre redes, APIs y cosas más complicadas.

---
**Elaborado por:** Elvis  
**Última actualización:** 14 de octubre de 2025  
**Nivel:** Intermedio → Avanzado  
**Tiempo estimado de dominio:** 2-3 meses con práctica regular  ```

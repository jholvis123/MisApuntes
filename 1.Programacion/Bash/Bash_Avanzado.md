# Bash Avanzado - Mis Apuntes

Estos son mis apuntes de las cosas más complicadas de bash. Ya doy por hecho que sabes lo básico e intermedio.

---

## Red y conexiones

### Enviar peticiones HTTP

Lo más básico que necesitas saber para interactuar con APIs:

```bash
# GET simple
curl https://api.github.com/users/octocat

# POST con datos
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"name":"nuevo repo"}' \
  https://api.github.com/user/repos

# Con autenticación
curl -H "Authorization: Bearer tu_token" \
  https://api.github.com/user
```

**Mi función para APIs que uso siempre:**

```bash
api_call() {
    local method="$1"
    local url="$2" 
    local data="$3"
    local token="$4"
    
    local headers=("-H" "Content-Type: application/json")
    
    if [[ -n "$token" ]]; then
        headers+=("-H" "Authorization: Bearer $token")
    fi
    
    if [[ -n "$data" ]]; then
        curl -X "$method" "${headers[@]}" -d "$data" "$url"
    else
        curl -X "$method" "${headers[@]}" "$url"
    fi
}

# Uso:
api_call "GET" "https://api.github.com/user" "" "mi_token"
api_call "POST" "https://api.ejemplo.com/data" '{"key":"value"}' "mi_token"
```

### Conexiones TCP/UDP

A veces necesitas conectarte directamente a puertos:

```bash
# Probar si un puerto está abierto
timeout 3 bash -c "</dev/tcp/google.com/80" && echo "Puerto abierto" || echo "Puerto cerrado"

# Enviar datos por TCP
echo "GET / HTTP/1.1" > /dev/tcp/google.com/80

# Crear un servidor simple
while true; do
    echo "Servidor funcionando en puerto 8080"
    nc -l 8080
done
```

**Script que hice para monitorear puertos:**

```bash
#!/bin/bash
# monitor_puertos.sh

check_port() {
    local host="$1"
    local port="$2"
    
    if timeout 2 bash -c "</dev/tcp/$host/$port" 2>/dev/null; then
        echo "✅ $host:$port está abierto"
        return 0
    else
        echo "❌ $host:$port está cerrado"
        return 1
    fi
}

# Lista de servicios a monitorear
declare -A servicios=(
    ["web"]="google.com:80"
    ["ssh"]="mi-servidor.com:22"
    ["database"]="db.empresa.com:5432"
)

echo "Revisando servicios..."
for nombre in "${!servicios[@]}"; do
    IFS=':' read -r host port <<< "${servicios[$nombre]}"
    check_port "$host" "$port"
done
```

---

## Procesamiento de archivos masivo

### Procesar archivos gigantes

Cuando tienes archivos de varios GB que no puedes cargar en memoria:

```bash
# Leer archivo línea por línea sin cargarlo todo
process_big_file() {
    local file="$1"
    local count=0
    
    while IFS= read -r line; do
        # Procesar cada línea
        if [[ $line == *"ERROR"* ]]; then
            echo "Error encontrado: $line" >> errores.log
        fi
        
        ((count++))
        if ((count % 10000 == 0)); then
            echo "Procesadas $count líneas..."
        fi
    done < "$file"
    
    echo "Total procesadas: $count líneas"
}

# Procesar solo las últimas líneas de un archivo grande
tail -f /var/log/huge.log | while read line; do
    echo "Nueva línea: $line"
done
```

### Procesamiento en paralelo

Para archivos múltiples:

```bash
#!/bin/bash
# procesar_paralelo.sh

MAX_JOBS=4

process_file() {
    local file="$1"
    echo "Procesando: $file"
    
    # Tu lógica aquí
    wc -l "$file" > "${file}.count"
    grep -c "ERROR" "$file" > "${file}.errors" 2>/dev/null
    
    echo "Terminado: $file"
}

# Función para manejar trabajos en paralelo
parallel_process() {
    local files=("$@")
    local pids=()
    
    for file in "${files[@]}"; do
        # Esperar si hay demasiados trabajos
        while [ ${#pids[@]} -ge $MAX_JOBS ]; do
            for i in "${!pids[@]}"; do
                if ! kill -0 "${pids[i]}" 2>/dev/null; then
                    unset pids[i]
                fi
            done
            pids=("${pids[@]}")
            sleep 0.1
        done
        
        # Iniciar nuevo trabajo
        process_file "$file" &
        pids+=($!)
    done
    
    # Esperar todos los trabajos
    for pid in "${pids[@]}"; do
        wait "$pid"
    done
}

# Usar con todos los archivos .log
files=(*.log)
parallel_process "${files[@]}"
```

---

## Bases de datos desde bash

### SQLite

El más fácil para scripts:

```bash
# Crear y usar base de datos
create_db() {
    sqlite3 mi_db.sqlite3 <<EOF
CREATE TABLE IF NOT EXISTS usuarios (
    id INTEGER PRIMARY KEY,
    nombre TEXT NOT NULL,
    email TEXT UNIQUE,
    fecha_creacion DATETIME DEFAULT CURRENT_TIMESTAMP
);
EOF
}

# Insertar datos
add_user() {
    local nombre="$1"
    local email="$2"
    
    sqlite3 mi_db.sqlite3 <<EOF
INSERT INTO usuarios (nombre, email) VALUES ('$nombre', '$email');
EOF
}

# Consultar datos
get_users() {
    sqlite3 -header -column mi_db.sqlite3 "SELECT * FROM usuarios;"
}

# Script completo de ejemplo
#!/bin/bash
create_db
add_user "Juan" "juan@email.com"
add_user "María" "maria@email.com"
get_users
```

### MySQL/PostgreSQL

Para bases remotas:

```bash
# MySQL
mysql_query() {
    local query="$1"
    mysql -h servidor -u usuario -p'password' -D base_datos -e "$query"
}

# PostgreSQL  
pg_query() {
    local query="$1"
    PGPASSWORD='password' psql -h servidor -U usuario -d base_datos -c "$query"
}

# Ejemplo: backup automático
backup_database() {
    local fecha=$(date +%Y%m%d_%H%M%S)
    local backup_file="backup_$fecha.sql"
    
    mysqldump -h servidor -u usuario -p'password' base_datos > "$backup_file"
    
    if [[ $? -eq 0 ]]; then
        echo "✅ Backup creado: $backup_file"
        
        # Comprimir
        gzip "$backup_file"
        
        # Subir a s3 o donde sea
        # aws s3 cp "${backup_file}.gz" s3://mi-bucket/backups/
    else
        echo "❌ Error en backup"
    fi
}
```

---

## Criptografía y seguridad

### Cifrado básico

```bash
# Cifrar archivo
encrypt_file() {
    local file="$1"
    local password="$2"
    
    openssl enc -aes-256-cbc -salt -in "$file" -out "${file}.enc" -pass pass:"$password"
    
    if [[ $? -eq 0 ]]; then
        echo "✅ Archivo cifrado: ${file}.enc"
        # Opcional: borrar original
        # rm "$file"
    fi
}

# Descifrar archivo
decrypt_file() {
    local file="$1"
    local password="$2"
    local output="${file%.enc}"  # quitar .enc del final
    
    openssl enc -aes-256-cbc -d -in "$file" -out "$output" -pass pass:"$password"
    
    if [[ $? -eq 0 ]]; then
        echo "✅ Archivo descifrado: $output"
    fi
}

# Generar hash
get_hash() {
    local file="$1"
    echo "MD5:    $(md5sum "$file" | cut -d' ' -f1)"
    echo "SHA256: $(sha256sum "$file" | cut -d' ' -f1)"
}
```

### Generar passwords seguros

```bash
# Generar password aleatorio
generate_password() {
    local length="${1:-16}"
    
    # Método 1: usando /dev/urandom
    tr -dc 'A-Za-z0-9!@#$%^&*' < /dev/urandom | head -c "$length"
    echo
    
    # Método 2: usando openssl
    openssl rand -base64 32 | tr -d '+/=' | head -c "$length"
    echo
}

# Verificar fortaleza de password
check_password_strength() {
    local password="$1"
    local score=0
    
    # Longitud
    if [[ ${#password} -ge 12 ]]; then
        ((score++))
        echo "✅ Longitud adecuada"
    else
        echo "❌ Muy corto (mínimo 12 caracteres)"
    fi
    
    # Mayúsculas
    if [[ $password =~ [A-Z] ]]; then
        ((score++))
        echo "✅ Contiene mayúsculas"
    else
        echo "❌ Sin mayúsculas"
    fi
    
    # Minúsculas
    if [[ $password =~ [a-z] ]]; then
        ((score++))
        echo "✅ Contiene minúsculas"
    else
        echo "❌ Sin minúsculas"
    fi
    
    # Números
    if [[ $password =~ [0-9] ]]; then
        ((score++))
        echo "✅ Contiene números"
    else
        echo "❌ Sin números"
    fi
    
    # Símbolos
    if [[ $password =~ [^A-Za-z0-9] ]]; then
        ((score++))
        echo "✅ Contiene símbolos"
    else
        echo "❌ Sin símbolos"
    fi
    
    echo "Puntuación: $score/5"
    
    if [[ $score -ge 4 ]]; then
        echo "🟢 Password fuerte"
    elif [[ $score -ge 2 ]]; then
        echo "🟡 Password regular"
    else
        echo "🔴 Password débil"
    fi
}
```

---

## Automatización del sistema

### Cron jobs inteligentes

```bash
#!/bin/bash
# cron_job.sh - Para usar en crontab

# Logging para cron
LOG_FILE="/var/log/mi_script.log"

log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*" >> "$LOG_FILE"
}

# Función principal
main() {
    log "Iniciando tarea programada"
    
    # Tu lógica aquí
    if hacer_backup; then
        log "✅ Backup completado"
    else
        log "❌ Error en backup"
        # Enviar notificación
        echo "Error en backup del $(date)" | mail -s "Error Cron Job" admin@empresa.com
    fi
    
    log "Tarea finalizada"
}

hacer_backup() {
    # Lógica de backup
    return 0  # 0 = éxito, 1 = error
}

# Solo ejecutar si se llama directamente
if [[ "${BASH_SOURCE[0]}" == "${0}" ]]; then
    main "$@"
fi
```

**Mi crontab típico:**

```bash
# Editar crontab: crontab -e

# Backup diario a las 2 AM
0 2 * * * /home/user/scripts/backup_diario.sh

# Limpiar logs cada domingo a las 3 AM  
0 3 * * 0 /home/user/scripts/limpiar_logs.sh

# Monitorear servidor cada 5 minutos
*/5 * * * * /home/user/scripts/check_server.sh

# Reiniciar service problemático cada hora
0 * * * * systemctl restart servicio_problematico
```

### Monitoreo del sistema

```bash
#!/bin/bash
# monitor_sistema.sh

# Configuración
CPU_LIMIT=80
MEMORY_LIMIT=85
DISK_LIMIT=90

send_alert() {
    local message="$1"
    echo "$message" | mail -s "ALERTA SERVIDOR" admin@empresa.com
    echo "$(date): $message" >> /var/log/alertas.log
}

check_cpu() {
    local cpu_usage=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d'%' -f1)
    cpu_usage=${cpu_usage%.*}  # quitar decimales
    
    if [[ $cpu_usage -gt $CPU_LIMIT ]]; then
        send_alert "🚨 CPU alto: ${cpu_usage}% (límite: ${CPU_LIMIT}%)"
        return 1
    fi
    
    echo "✅ CPU: ${cpu_usage}%"
    return 0
}

check_memory() {
    local memory_usage=$(free | grep Mem | awk '{printf("%.0f", ($3/$2) * 100.0)}')
    
    if [[ $memory_usage -gt $MEMORY_LIMIT ]]; then
        send_alert "🚨 Memoria alta: ${memory_usage}% (límite: ${MEMORY_LIMIT}%)"
        return 1
    fi
    
    echo "✅ Memoria: ${memory_usage}%"
    return 0
}

check_disk() {
    local disk_usage=$(df -h / | awk 'NR==2{print $5}' | cut -d'%' -f1)
    
    if [[ $disk_usage -gt $DISK_LIMIT ]]; then
        send_alert "🚨 Disco lleno: ${disk_usage}% (límite: ${DISK_LIMIT}%)"
        return 1
    fi
    
    echo "✅ Disco: ${disk_usage}%"
    return 0
}

check_services() {
    local services=("nginx" "mysql" "redis")
    local failed_services=()
    
    for service in "${services[@]}"; do
        if ! systemctl is-active --quiet "$service"; then
            failed_services+=("$service")
        fi
    done
    
    if [[ ${#failed_services[@]} -gt 0 ]]; then
        send_alert "🚨 Servicios caídos: ${failed_services[*]}"
        return 1
    fi
    
    echo "✅ Todos los servicios funcionando"
    return 0
}

# Ejecutar todas las comprobaciones
main() {
    echo "=== Monitoreo del Sistema $(date) ==="
    
    local errors=0
    
    check_cpu || ((errors++))
    check_memory || ((errors++))
    check_disk || ((errors++))
    check_services || ((errors++))
    
    if [[ $errors -eq 0 ]]; then
        echo "🟢 Sistema funcionando correctamente"
    else
        echo "🔴 Se encontraron $errors problemas"
    fi
    
    echo "=== Fin del monitoreo ==="
}

main
```

---

## Scripting avanzado

### Manejo de señales

```bash
#!/bin/bash
# Manejo profesional de señales

cleanup() {
    echo "Limpiando antes de salir..."
    
    # Matar procesos hijos
    jobs -p | xargs -r kill
    
    # Limpiar archivos temporales
    rm -f /tmp/mi_script_*
    
    # Cerrar conexiones, etc.
    
    echo "Limpieza completada"
    exit 0
}

# Configurar traps
trap cleanup SIGINT SIGTERM EXIT

# Script principal
main() {
    echo "Script iniciado (PID: $$)"
    
    # Crear archivo temporal
    temp_file=$(mktemp /tmp/mi_script_XXXXXX)
    
    # Trabajo que puede tomar tiempo
    for i in {1..100}; do
        echo "Procesando item $i" > "$temp_file"
        sleep 1
        
        # Simular trabajo
        echo "Progreso: $i/100"
    done
    
    echo "Script completado"
}

main "$@"
```

### Scripts que se auto-actualizan

```bash
#!/bin/bash
# auto_update.sh

SCRIPT_URL="https://raw.githubusercontent.com/user/repo/main/script.sh"
SCRIPT_PATH="$0"
VERSION="1.0.0"

check_for_updates() {
    echo "Verificando actualizaciones..."
    
    # Descargar versión remota
    local temp_script=$(mktemp)
    if curl -sSL "$SCRIPT_URL" -o "$temp_script"; then
        # Extraer versión del script remoto
        local remote_version=$(grep '^VERSION=' "$temp_script" | cut -d'"' -f2)
        
        if [[ "$remote_version" != "$VERSION" ]]; then
            echo "📥 Nueva versión disponible: $remote_version (actual: $VERSION)"
            read -p "¿Actualizar? (y/N): " response
            
            if [[ $response =~ ^[Yy]$ ]]; then
                # Backup del script actual
                cp "$SCRIPT_PATH" "${SCRIPT_PATH}.backup"
                
                # Actualizar
                cp "$temp_script" "$SCRIPT_PATH"
                chmod +x "$SCRIPT_PATH"
                
                echo "✅ Script actualizado. Reiniciando..."
                exec "$SCRIPT_PATH" "$@"
            fi
        else
            echo "✅ Script actualizado (versión $VERSION)"
        fi
        
        rm "$temp_script"
    else
        echo "❌ No se pudo verificar actualizaciones"
    fi
}

# Tu script principal
main() {
    echo "Mi Script v$VERSION"
    
    # Tu lógica aquí
    echo "Haciendo cosas importantes..."
}

# Verificar updates al inicio
check_for_updates "$@"
main "$@"
```

---

## APIs y webhooks

### Crear webhooks simples

```bash
#!/bin/bash
# webhook_receiver.sh

PORT="${PORT:-8080}"

handle_webhook() {
    local method="$1"
    local path="$2"
    local body="$3"
    
    echo "Webhook recibido:"
    echo "Método: $method"
    echo "Ruta: $path"
    echo "Cuerpo: $body"
    
    # Procesar según el webhook
    case "$path" in
        "/github")
            handle_github_webhook "$body"
            ;;
        "/deploy")
            handle_deploy_webhook "$body"
            ;;
        *)
            echo "Webhook desconocido: $path"
            ;;
    esac
}

handle_github_webhook() {
    local payload="$1"
    
    # Extraer información del push
    local repo=$(echo "$payload" | jq -r '.repository.name // "unknown"')
    local branch=$(echo "$payload" | jq -r '.ref // "unknown"' | sed 's|refs/heads/||')
    
    echo "Push recibido en $repo/$branch"
    
    # Si es push a main, hacer deploy
    if [[ "$branch" == "main" ]]; then
        echo "Iniciando deploy automático..."
        /path/to/deploy.sh "$repo" &
    fi
}

handle_deploy_webhook() {
    local payload="$1"
    
    # Tu lógica de deploy
    echo "Deploy solicitado"
}

# Servidor simple con netcat
start_server() {
    echo "Webhook server iniciado en puerto $PORT"
    
    while true; do
        {
            # Leer request HTTP
            read -r method path protocol
            
            # Leer headers
            local content_length=0
            while IFS=': ' read -r key value; do
                if [[ "$key" == "Content-Length" ]]; then
                    content_length=${value%$'\r'}
                fi
                
                # Línea vacía = fin de headers
                if [[ -z "$key" ]]; then
                    break
                fi
            done
            
            # Leer body si hay content-length
            local body=""
            if [[ $content_length -gt 0 ]]; then
                body=$(head -c "$content_length")
            fi
            
            # Procesar webhook
            handle_webhook "$method" "$path" "$body"
            
            # Respuesta HTTP
            echo "HTTP/1.1 200 OK"
            echo "Content-Type: application/json"
            echo ""
            echo '{"status":"ok"}'
            
        } | nc -l -p "$PORT"
    done
}

start_server
```

### Cliente REST completo

```bash
#!/bin/bash
# rest_client.sh

API_BASE_URL="${API_BASE_URL:-https://api.ejemplo.com}"
API_TOKEN="${API_TOKEN:-}"

# Función base para llamadas API
api_request() {
    local method="$1"
    local endpoint="$2"
    local data="$3"
    local extra_headers=("${@:4}")
    
    local url="${API_BASE_URL}${endpoint}"
    local headers=("-H" "Content-Type: application/json")
    
    # Agregar token si existe
    if [[ -n "$API_TOKEN" ]]; then
        headers+=("-H" "Authorization: Bearer $API_TOKEN")
    fi
    
    # Agregar headers extra
    headers+=("${extra_headers[@]}")
    
    # Hacer request
    local response
    if [[ -n "$data" ]]; then
        response=$(curl -s -w "\n%{http_code}" -X "$method" "${headers[@]}" -d "$data" "$url")
    else
        response=$(curl -s -w "\n%{http_code}" -X "$method" "${headers[@]}" "$url")
    fi
    
    # Separar response body y status code
    local body=$(echo "$response" | head -n -1)
    local status=$(echo "$response" | tail -n 1)
    
    # Manejar errores
    if [[ $status -ge 400 ]]; then
        echo "Error $status: $body" >&2
        return 1
    fi
    
    echo "$body"
    return 0
}

# Funciones específicas para endpoints
get_users() {
    api_request "GET" "/users"
}

get_user() {
    local user_id="$1"
    api_request "GET" "/users/$user_id"
}

create_user() {
    local name="$1"
    local email="$2"
    local data=$(jq -n --arg name "$name" --arg email "$email" '{name: $name, email: $email}')
    
    api_request "POST" "/users" "$data"
}

update_user() {
    local user_id="$1"
    local name="$2"
    local email="$3"
    local data=$(jq -n --arg name "$name" --arg email "$email" '{name: $name, email: $email}')
    
    api_request "PUT" "/users/$user_id" "$data"
}

delete_user() {
    local user_id="$1"
    api_request "DELETE" "/users/$user_id"
}

# CLI interface
case "$1" in
    "list")
        get_users | jq '.[]'
        ;;
    "get")
        get_user "$2" | jq '.'
        ;;
    "create")
        create_user "$2" "$3" | jq '.'
        ;;
    "update")
        update_user "$2" "$3" "$4" | jq '.'
        ;;
    "delete")
        delete_user "$2"
        echo "Usuario $2 eliminado"
        ;;
    *)
        echo "Uso: $0 {list|get|create|update|delete} [args...]"
        echo ""
        echo "Ejemplos:"
        echo "  $0 list"
        echo "  $0 get 123"
        echo "  $0 create 'Juan Pérez' 'juan@email.com'"
        echo "  $0 update 123 'Juan Carlos' 'juan.carlos@email.com'"
        echo "  $0 delete 123"
        exit 1
        ;;
esac
```

---

## Lo que aprendí

Con esto ya puedo hacer scripts realmente avanzados que:

- ✅ Se conectan a APIs y bases de datos
- ✅ Procesan archivos gigantes sin problemas
- ✅ Manejan seguridad y cifrado
- ✅ Se auto-actualizan y se mantienen solos
- ✅ Funcionan como servicios web
- ✅ Monitorizan sistemas completos

**Nota personal:** Este nivel ya es para scripts que van en producción. Siempre hay que probar bien antes de ponerlos a funcionar.

**Lo que sigue:** Ya con esto puedes hacer cualquier cosa. Solo falta practicar y seguir aprendiendo según lo que necesites.

---
**Mis apuntes de:** Elvis  
**Fecha:** 13 de octubre de 2025  
**Nivel:** Avanzado  
**Tiempo para dominarlo:** 6+ meses practicando

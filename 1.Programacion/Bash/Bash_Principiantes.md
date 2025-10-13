# 🐚 Guía Completa de Bash para Principiantes

**Tags:** `#bash` `#linux` `#scripting` `#terminal` `#principiantes` `#shell`

## 📚 ¿Qué es Bash?

**Bash** (Bourne Again Shell) es un **intérprete de comandos** que te permite:
- 💻 Ejecutar comandos en sistemas Linux/macOS/Windows (WSL)
- 📝 Escribir scripts (programas) para automatizar tareas
- 🔧 Administrar archivos y sistemas de manera eficiente

**🎯 Piénsalo así:** Bash es como un "traductor" entre tú y tu computadora. Le dices qué hacer en palabras que entiende, y él lo ejecuta.

## 🤔 ¿Por qué aprender Bash?

### Para **Principiantes**:
- ✅ **Más control:** Haces cosas que las interfaces gráficas no permiten
- ✅ **Más rápido:** Un comando hace lo que tomaría 20 clics
- ✅ **Más poderoso:** Puedes automatizar tareas repetitivas

### Para **Desarrolladores**:
- 🚀 Automatizar compilaciones y despliegues
- 🧪 Ejecutar pruebas automáticamente
- 📊 Procesar archivos y datos

### Para **Administradores de Sistemas**:
- ⚙️ Automatizar tareas del servidor
- 📈 Monitorear sistemas
- 🔐 Gestionar usuarios y permisos

## 🚀 Primeros Pasos: Abriendo la Terminal

### En Linux/macOS:
- **Método 1:** Presiona `Ctrl + Alt + T`
- **Método 2:** Busca "Terminal" en tus aplicaciones

### En Windows:
- **Opción 1:** Instala WSL (Windows Subsystem for Linux)
- **Opción 2:** Usa Git Bash
- **Opción 3:** Usa PowerShell (similar pero diferente)

### 🎯 ¿Cómo sé que estoy en Bash?
Verás algo como esto:
```bash
usuario@computadora:~$ 
```

**Explicación:**
- `usuario`: Tu nombre de usuario
- `computadora`: Nombre de tu computadora
- `~`: Estás en tu carpeta personal (home)
- `$`: Indica que puedes escribir comandos
## 📁 Comandos Básicos: Navegación y Archivos

### 1. `pwd` - ¿Dónde estoy?
**¿Qué hace?** Muestra tu ubicación actual (como GPS para carpetas)

```bash
pwd
```
**Salida esperada:**
```
/home/usuario
```

### 2. `ls` - ¿Qué hay aquí?
**¿Qué hace?** Lista el contenido de la carpeta actual

```bash
ls
```
**Salida esperada:**
```
Documentos  Escritorio  Imágenes  Música
```

**Opciones útiles:**
```bash
ls -l        # Lista detallada (permisos, tamaño, fecha)
ls -a        # Muestra archivos ocultos (empiezan con .)
ls -la       # Combina ambas opciones
ls -h        # Tamaños en formato humano (KB, MB, GB)
```

### 3. `cd` - Moverse entre carpetas
**¿Qué hace?** Cambia de directorio (como hacer doble clic en una carpeta)

```bash
cd Documentos        # Entra a la carpeta Documentos
cd ..               # Sube un nivel (carpeta padre)
cd ~                # Vuelve a tu carpeta personal
cd /                # Va a la raíz del sistema
cd -                # Vuelve a la carpeta anterior
```

**🎯 Ejemplo práctico:**
```bash
pwd                 # Estás en: /home/usuario
cd Documentos       # Entras a Documentos
pwd                 # Ahora estás en: /home/usuario/Documentos
cd ..               # Subes un nivel
pwd                 # Vuelves a: /home/usuario
```

### 4. `mkdir` - Crear carpetas
**¿Qué hace?** Crea nuevas carpetas

```bash
mkdir mi_proyecto              # Crea una carpeta
mkdir carpeta1 carpeta2        # Crea varias carpetas
mkdir -p proyecto/src/main     # Crea carpetas anidadas
```

### 5. `touch` - Crear archivos vacíos
**¿Qué hace?** Crea archivos nuevos vacíos

```bash
touch archivo.txt              # Crea un archivo vacío
touch script.sh readme.md      # Crea varios archivos
```

### 6. `echo` - Mostrar texto
**¿Qué hace?** Muestra texto en pantalla (como `print` en otros lenguajes)

```bash
echo "Hola Mundo"             # Muestra: Hola Mundo
echo $USER                    # Muestra tu nombre de usuario
echo "Mi nombre es $USER"     # Combina texto y variables
```

**🎯 Guardar texto en archivos:**
```bash
echo "Hola Mundo" > archivo.txt     # Crea archivo con texto
echo "Segunda línea" >> archivo.txt  # Añade texto al final
```

### 7. `cat` - Ver contenido de archivos
**¿Qué hace?** Muestra el contenido completo de un archivo

```bash
cat archivo.txt        # Muestra todo el contenido
cat -n archivo.txt     # Muestra con números de línea
```

### 8. `cp` - Copiar archivos
**¿Qué hace?** Copia archivos y carpetas

```bash
cp archivo.txt copia.txt           # Copia archivo
cp archivo.txt ~/Documentos/       # Copia a otra carpeta
cp -r carpeta_origen carpeta_copia # Copia carpeta completa
```

### 9. `mv` - Mover/Renombrar
**¿Qué hace?** Mueve archivos o los renombra

```bash
mv archivo.txt nuevo_nombre.txt    # Renombra archivo
mv archivo.txt ~/Documentos/       # Mueve archivo
mv carpeta_vieja carpeta_nueva     # Renombra carpeta
```

### 10. `rm` - Eliminar archivos ⚠️
**¿Qué hace?** Elimina archivos (¡CUIDADO! No va a papelera)

```bash
rm archivo.txt         # Elimina archivo
rm -r carpeta/         # Elimina carpeta y su contenido
rm -f archivo.txt      # Fuerza eliminación sin preguntar
rm -rf carpeta/        # ¡PELIGROSO! Elimina todo sin preguntar
```

**⚠️ PRECAUCIÓN:** `rm` elimina permanentemente. No hay Ctrl+Z. 
## 🔍 Procesamiento de Texto: Comandos Poderosos

### `grep` - El Detective de Texto
**¿Qué hace?** Busca texto específico dentro de archivos (como Ctrl+F pero súper poderoso)

#### 🎯 Ejemplo Básico
```bash
# Crear archivo de ejemplo
echo -e "Hola mundo\nBash es genial\nLinux es poderoso\nbash scripting" > ejemplo.txt

# Buscar "bash" (sensible a mayúsculas)
grep "bash" ejemplo.txt
# Salida: bash scripting

# Buscar "bash" ignorando mayúsculas
grep -i "bash" ejemplo.txt
# Salida: Bash es genial
#         bash scripting
```

#### 🔧 Opciones Más Útiles
```bash
grep -n "texto" archivo.txt     # Muestra número de línea
grep -c "texto" archivo.txt     # Cuenta coincidencias
grep -v "texto" archivo.txt     # Muestra líneas que NO contienen el texto
grep -r "texto" carpeta/        # Busca en todos los archivos de la carpeta
grep -l "texto" *.txt          # Solo muestra nombres de archivos que contienen el texto
```

#### 🎯 Ejemplo Práctico Real
```bash
# Buscar errores en logs
grep -i "error" /var/log/system.log

# Buscar tu IP en configuraciones
grep -r "192.168" /etc/

# Encontrar funciones en código Python
grep -n "def " *.py
```

### `awk` - El Procesador de Columnas
**¿Qué hace?** Procesa texto por columnas (como Excel pero en terminal)

#### 🎯 Conceptos Básicos
- `$1` = Primera columna
- `$2` = Segunda columna  
- `$0` = Línea completa
- `NR` = Número de línea actual

#### 📊 Ejemplo con Datos
```bash
# Crear archivo CSV de ejemplo
echo -e "Juan,25,Ingeniero\nMería,30,Doctora\nCarlos,28,Programador" > personas.csv

# Mostrar solo nombres (primera columna)
awk -F"," '{print $1}' personas.csv
# Salida: Juan
#         María  
#         Carlos

# Mostrar nombres y edades
awk -F"," '{print $1 " tiene " $2 " años"}' personas.csv
# Salida: Juan tiene 25 años
#         María tiene 30 años
#         Carlos tiene 28 años
```

#### 🔧 Ejemplos Útiles
```bash
# Sumar números en una columna
awk '{sum += $1} END {print "Total:", sum}' numeros.txt

# Mostrar líneas largas (más de 80 caracteres)
awk 'length($0) > 80' archivo.txt

# Procesar logs con fecha y hora
awk '{print $1, $2, $4}' /var/log/access.log
```

### `sed` - El Editor de Texto Ninja
**¿Qué hace?** Edita texto automáticamente (buscar y reemplazar súper poderoso)

#### 🎯 Ejemplos Básicos
```bash
# Reemplazar texto (solo primera ocurrencia por línea)
sed 's/viejo/nuevo/' archivo.txt

# Reemplazar TODAS las ocurrencias
sed 's/viejo/nuevo/g' archivo.txt

# Eliminar líneas que contengan "borrar"
sed '/borrar/d' archivo.txt

# Mostrar solo líneas 2 a 5
sed -n '2,5p' archivo.txt
```

#### 🎯 Ejemplo Práctico
```bash
# Cambiar todas las IPs en un archivo de configuración
sed 's/192.168.1.100/192.168.1.200/g' config.txt

# Eliminar líneas vacías
sed '/^$/d' archivo.txt

# Añadir texto al principio de cada línea
sed 's/^/>> /' archivo.txt
```

### `sort` y `uniq` - Ordenar y Limpiar
```bash
# Ordenar líneas alfabéticamente
sort nombres.txt

# Ordenar números correctamente
sort -n numeros.txt

# Eliminar líneas duplicadas (requiere que esté ordenado primero)
sort archivo.txt | uniq

# Contar ocurrencias de cada línea
sort archivo.txt | uniq -c
```

### `wc` - Contador de Todo
```bash
wc archivo.txt          # Líneas, palabras, caracteres
wc -l archivo.txt       # Solo líneas
wc -w archivo.txt       # Solo palabras
wc -c archivo.txt       # Solo caracteres
```

### 🎯 Ejemplo Práctico Combinado
```bash
# Análisis de un log de servidor web
cat access.log | \
awk '{print $1}' | \     # Extraer IPs
sort | \                 # Ordenar
uniq -c | \             # Contar ocurrencias
sort -nr                # Ordenar por número (mayor a menor)
```
**¿Qué hace esto?** Te muestra qué IPs visitan más tu sitio web.

## 📝 Conceptos de Programación en Bash

### 🔤 Variables: Guardando Información
**¿Qué son?** Como cajas etiquetadas donde guardas datos

#### Crear y Usar Variables
```bash
# Crear variables (SIN espacios alrededor del =)
nombre="Juan"
edad=25
fecha=$(date)

# Usar variables (con $)
echo "Hola $nombre"
echo "Tienes $edad años"
echo "Hoy es $fecha"
```

#### 🎯 Variables Especiales del Sistema
```bash
echo $USER          # Tu nombre de usuario
echo $HOME          # Tu carpeta personal  
echo $PWD           # Directorio actual
echo $PATH          # Rutas donde busca comandos
echo $$             # ID del proceso actual
echo $?             # Código de salida del último comando
```

#### 📝 Leer Input del Usuario
```bash
echo "¿Cómo te llamas?"
read nombre
echo "Hola $nombre, ¡encantado de conocerte!"
```

### 🤔 Condicionales: Tomar Decisiones
**¿Qué son?** Como preguntas: "¿Si esto es verdad, haz aquello"

#### Estructura Básica
```bash
if [ condición ]; then
    # Código si es verdadero
else
    # Código si es falso
fi
```

#### 🎯 Ejemplos Prácticos
```bash
# Verificar si un archivo existe
if [ -f "archivo.txt" ]; then
    echo "El archivo existe"
else
    echo "El archivo no existe"
fi

# Comparar números
edad=18
if [ $edad -ge 18 ]; then
    echo "Eres mayor de edad"
else
    echo "Eres menor de edad"
fi

# Comparar texto
usuario="admin"
if [ "$usuario" = "admin" ]; then
    echo "Bienvenido administrador"
else
    echo "Usuario normal"
fi
```

#### 🔧 Operadores de Comparación
```bash
# Para números
-eq  # igual a
-ne  # no igual a  
-gt  # mayor que
-ge  # mayor o igual que
-lt  # menor que
-le  # menor o igual que

# Para archivos
-f   # existe y es archivo
-d   # existe y es directorio
-r   # es legible
-w   # es escribible
-x   # es ejecutable

# Para texto
=    # igual
!=   # diferente
-z   # está vacío
-n   # no está vacío
```

### 🔄 Bucles: Repetir Tareas
**¿Qué son?** Como decir "haz esto X veces" o "haz esto mientras..."

#### Bucle `for` - Repetir con Lista
```bash
# Iterar sobre archivos
for archivo in *.txt; do
    echo "Procesando: $archivo"
done

# Iterar sobre números
for numero in {1..5}; do
    echo "Número: $numero"
done

# Iterar sobre lista de palabras
for color in rojo verde azul; do
    echo "Color: $color"
done
```

#### Bucle `while` - Repetir Mientras...
```bash
# Contador simple
contador=1
while [ $contador -le 5 ]; do
    echo "Vuelta número: $contador"
    contador=$((contador + 1))
done

# Leer líneas de un archivo
while read linea; do
    echo "Línea: $linea"
done < archivo.txt
```

### 🛠️ Funciones: Código Reutilizable
**¿Qué son?** Como crear tus propios comandos personalizados

```bash
# Definir función
saludo() {
    echo "¡Hola $1! ¿Cómo estás?"
}

# Usar función
saludo "María"    # Salida: ¡Hola María! ¿Cómo estás?

# Función con múltiples parámetros
crear_backup() {
    origen=$1
    destino=$2
    cp -r "$origen" "$destino"
    echo "Backup creado: $origen -> $destino"
}

crear_backup "/home/usuario/documentos" "/backup/documentos"
```

### 🎯 Script Completo de Ejemplo
```bash
#!/bin/bash
# Mi primer script útil

echo "=== GESTOR DE ARCHIVOS ==="
echo "1. Listar archivos"
echo "2. Crear archivo"
echo "3. Eliminar archivo"
echo "4. Salir"

read -p "¿Qué quieres hacer? (1-4): " opcion

case $opcion in
    1)
        echo "Archivos en el directorio actual:"
        ls -la
        ;;
    2)
        read -p "Nombre del archivo a crear: " nombre
        touch "$nombre"
        echo "Archivo '$nombre' creado exitosamente"
        ;;
    3)
        read -p "Nombre del archivo a eliminar: " nombre
        if [ -f "$nombre" ]; then
            rm "$nombre"
            echo "Archivo '$nombre' eliminado"
        else
            echo "El archivo '$nombre' no existe"
        fi
        ;;
    4)
        echo "¡Hasta luego!"
        exit 0
        ;;
    *)
        echo "Opción no válida"
        ;;
esac
```

## 💡 Pipes y Redirección: Conectando Comandos

### 🔀 Pipes `|` - Pasar Salida Entre Comandos
```bash
# Listar archivos y buscar uno específico
ls -la | grep "documento"

# Contar líneas de archivos .txt
cat *.txt | wc -l

# Ver procesos y buscar uno específico
ps aux | grep "firefox"

# Ordenar y mostrar solo los primeros 10
ls -la | sort | head -10
```

### 📄 Redirección: Guardar Salidas
```bash
# Guardar salida en archivo (sobrescribe)
ls -la > lista_archivos.txt

# Añadir salida al final del archivo
echo "Nueva línea" >> archivo.txt

# Redirigir errores
comando_que_falla 2> errores.txt

# Redirigir salida normal Y errores
comando > todo.txt 2>&1

## 🏋️ Ejercicios Prácticos

### 🎯 Ejercicio 1: Organizador de Archivos
**Tarea:** Crear un script que organice archivos por extensión

```bash
#!/bin/bash
# organizador.sh

echo "Organizando archivos por tipo..."

# Crear carpetas si no existen
mkdir -p imagenes documentos videos audios

# Mover archivos por extensión
mv *.jpg *.png *.gif imagenes/ 2>/dev/null
mv *.pdf *.txt *.doc documentos/ 2>/dev/null  
mv *.mp4 *.avi *.mkv videos/ 2>/dev/null
mv *.mp3 *.wav *.flac audios/ 2>/dev/null

echo "¡Archivos organizados!"
```

### 🎯 Ejercicio 2: Monitor de Sistema Simple
```bash
#!/bin/bash
# monitor.sh

echo "=== MONITOR DEL SISTEMA ==="
echo "Fecha y hora: $(date)"
echo "Usuario actual: $USER"
echo "Directorio actual: $PWD"
echo ""
echo "Uso de disco:"
df -h | head -5
echo ""
echo "Memoria RAM:"
free -h
echo ""
echo "Procesos que más CPU usan:"
ps aux --sort=-%cpu | head -5
```

### 🎯 Ejercicio 3: Calculadora Simple
```bash
#!/bin/bash
# calculadora.sh

echo "=== CALCULADORA SIMPLE ==="
read -p "Primer número: " num1
read -p "Operación (+, -, *, /): " op
read -p "Segundo número: " num2

case $op in
    +) resultado=$((num1 + num2)) ;;
    -) resultado=$((num1 - num2)) ;;
    \*) resultado=$((num1 * num2)) ;;
    /) 
        if [ $num2 -eq 0 ]; then
            echo "Error: División por cero"
            exit 1
        fi
        resultado=$((num1 / num2)) 
        ;;
    *) echo "Operación no válida"; exit 1 ;;
esac

echo "Resultado: $num1 $op $num2 = $resultado"
```

### 🎯 Ejercicio 4: Backup Automático
```bash
#!/bin/bash
# backup.sh

ORIGEN="$HOME/Documentos"
DESTINO="$HOME/Backups"
FECHA=$(date +%Y%m%d_%H%M%S)

# Crear directorio de backup si no existe
mkdir -p "$DESTINO"

# Crear backup con fecha
tar -czf "$DESTINO/backup_$FECHA.tar.gz" "$ORIGEN"

if [ $? -eq 0 ]; then
    echo "✅ Backup creado exitosamente: backup_$FECHA.tar.gz"
    
    # Mantener solo los 5 backups más recientes
    cd "$DESTINO"
    ls -t backup_*.tar.gz | tail -n +6 | xargs rm -f
    echo "🧹 Backups antiguos eliminados"
else
    echo "❌ Error al crear backup"
fi
```

## 🚨 Troubleshooting: Errores Comunes

### ❌ Error: "Permission denied"
```bash
# Problema: No tienes permisos para ejecutar
./script.sh
# bash: ./script.sh: Permission denied

# Solución: Dar permisos de ejecución  
chmod +x script.sh
```

### ❌ Error: "No such file or directory"
```bash
# Problema: El archivo o ruta no existe
cat archivo_inexistente.txt

# Solución: Verificar que existe
if [ -f "archivo.txt" ]; then
    cat archivo.txt
else
    echo "El archivo no existe"
fi
```

### ❌ Error: "command not found"
```bash
# Problema: El comando no está instalado o no está en PATH
micomando
# bash: micomando: command not found

# Solución 1: Verificar instalación
which micomando
whereis micomando

# Solución 2: Instalar si es necesario (Ubuntu/Debian)
sudo apt install paquete

# Solución 3: Usar ruta completa
/usr/bin/micomando
```

### ❌ Error en Variables
```bash
# ❌ INCORRECTO (espacios alrededor del =)
nombre = "Juan"

# ✅ CORRECTO (sin espacios)
nombre="Juan"

# ❌ INCORRECTO (olvidar $ para usar variable)
echo nombre

# ✅ CORRECTO
echo $nombre
```

### 🔧 Debugging: Encontrar Errores
```bash
# Ejecutar script mostrando cada comando
bash -x script.sh

# Añadir al inicio del script para debug completo
#!/bin/bash -x

# Debug manual con echo
echo "DEBUG: Variable nombre = $nombre"
echo "DEBUG: Llegó hasta aquí"
```

## 🎯 Mejores Prácticas

### ✅ Buenas Prácticas
```bash
# 1. Siempre usa #!/bin/bash al inicio
#!/bin/bash

# 2. Comenta tu código
# Este script hace backup de documentos

# 3. Usa nombres descriptivos
archivo_configuracion="config.txt"
# No: f="config.txt"

# 4. Valida inputs
if [ $# -eq 0 ]; then
    echo "Uso: $0 <archivo>"
    exit 1
fi

# 5. Entrecomilla variables con espacios
cp "$archivo con espacios" "$destino"

# 6. Verifica comandos críticos
if ! cp "$origen" "$destino"; then
    echo "Error: No se pudo copiar archivo"
    exit 1
fi
```

### 🛡️ Seguridad Básica
```bash
# Evitar inyección de comandos con input del usuario
read -p "Nombre de archivo: " archivo
# No hagas: rm $archivo (peligroso)
# Haz: rm "$archivo" (seguro)

# Validar input
if [[ "$archivo" =~ ^[a-zA-Z0-9._-]+$ ]]; then
    rm "$archivo"
else
    echo "Nombre de archivo no válido"
fi
```

## 🎓 Recursos para Seguir Aprendiendo

### 📚 Comandos de Ayuda
```bash
man comando        # Manual completo del comando
comando --help     # Ayuda rápida
type comando       # Qué tipo de comando es
which comando      # Dónde está ubicado el comando
```

### 🌐 Recursos Online
- **ShellCheck.net**: Verificador de scripts Bash online
- **Bash Guide**: guides.bash.hackers.nz
- **Linux Command**: linuxcommand.org
- **Práctica**: overthewire.org/wargames/bandit

### 📖 Próximos Pasos
1. **Practica diariamente** - Usa terminal para tareas cotidianas
2. **Automatiza tareas repetitivas** - Convierte clicks en scripts
3. **Lee scripts de otros** - GitHub tiene miles de ejemplos
4. **Aprende herramientas avanzadas** - sed, awk, grep con regex
5. **Estudia administración de sistemas** - cron, systemd, logs

---
**🎯 Recuerda:** La práctica hace al maestro. ¡Empieza con comandos simples y ve avanzando poco a poco!

**Elaborado por:** Elvis  
**Última actualización:** 13 de octubre de 2025
```
	



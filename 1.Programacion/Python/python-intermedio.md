# 🚀 Python Intermedio - Apuntes de Elvis

> **Ya sabes lo básico, ahora vamos por más** - Para quien domina variables, listas y operaciones

---

## 🎯 ¿Qué vas a aprender?

En estas 4-5 semanas aprenderás:
- Condicionales (tomar decisiones)
- Bucles (repetir cosas automáticamente)
- Funciones (crear tus propias herramientas)
- Diccionarios (organizar mejor los datos)
- Manejo de errores (que no explote tu programa)
- Archivos (guardar y leer información)

**Requisito:** Debes dominar variables, listas básicas, operaciones y f-strings.

---

## 🤔 Condicionales - Tomar decisiones

### If básico

```python
edad = 18

# Decisión simple
if edad >= 18:
    print("Eres mayor de edad")
    print("Puedes votar")

# Si no cumple la condición, no hace nada
```

### If-else

```python
edad = 16

if edad >= 18:
    print("Puedes votar")
else:
    print("Aún no puedes votar")
    print(f"Te faltan {18 - edad} años")

# Siempre hace una de las dos cosas
```

### If-elif-else (múltiples opciones)

```python
nota = 85

if nota >= 90:
    calificacion = "Excelente"
    mensaje = "¡Felicidades!"
elif nota >= 80:
    calificacion = "Muy bueno"
    mensaje = "Buen trabajo"
elif nota >= 70:
    calificacion = "Bueno"
    mensaje = "Puedes mejorar"
elif nota >= 60:
    calificacion = "Suficiente"
    mensaje = "Necesitas estudiar más"
else:
    calificacion = "Insuficiente"
    mensaje = "Hay que esforzarse más"

print(f"Tu nota: {nota}")
print(f"Calificación: {calificacion}")
print(mensaje)
```

### Operadores de comparación

```python
a = 10
b = 5

print(a == b)   # False (igual a)
print(a != b)   # True (diferente de)
print(a > b)    # True (mayor que)
print(a < b)    # False (menor que)
print(a >= b)   # True (mayor o igual)
print(a <= b)   # False (menor o igual)

# Con texto
nombre1 = "Ana"
nombre2 = "ana"
print(nombre1 == nombre2)  # False (distingue mayúsculas)
print(nombre1.lower() == nombre2.lower())  # True
```

### Operadores lógicos

```python
edad = 25
tiene_licencia = True
tiene_experiencia = False

# AND - AMBAS condiciones deben ser verdaderas
puede_manejar_taxi = edad >= 21 and tiene_licencia and tiene_experiencia
print(f"¿Puede manejar taxi? {puede_manejar_taxi}")  # False

# OR - AL MENOS UNA condición debe ser verdadera
puede_trabajar = edad >= 18 or tiene_experiencia
print(f"¿Puede trabajar? {puede_trabajar}")  # True

# NOT - invierte el resultado
no_tiene_experiencia = not tiene_experiencia
print(f"¿No tiene experiencia? {no_tiene_experiencia}")  # True

# Combinando con paréntesis
puede_conducir = edad >= 18 and (tiene_licencia or edad >= 21)
```

### Verificar contenido

```python
# En listas
frutas_disponibles = ["manzana", "banana", "naranja"]
fruta_deseada = "manzana"

if fruta_deseada in frutas_disponibles:
    print(f"Sí tenemos {fruta_deseada}")
else:
    print(f"No tenemos {fruta_deseada}")

# En texto
email = "usuario@gmail.com"

if "@" in email and "." in email:
    print("El email parece válido")
else:
    print("El email no es válido")

# Múltiples verificaciones
if email.endswith(".com") or email.endswith(".es") or email.endswith(".org"):
    print("Dominio válido")
```

---

## 🔄 Bucles - Repetir cosas automáticamente

### For - Cuando sabes cuántas veces repetir

```python
# Repetir números
print("Contando del 0 al 4:")
for i in range(5):
    print(f"Número: {i}")

# Con inicio y fin
print("\nDel 2 al 7:")
for i in range(2, 8):
    print(i)

# Con saltos
print("\nNúmeros pares del 0 al 10:")
for i in range(0, 11, 2):
    print(i)
```

### For con listas

```python
# Recorrer elementos
frutas = ["manzana", "banana", "naranja", "uva"]

print("Mis frutas favoritas:")
for fruta in frutas:
    print(f"- {fruta}")

# Con índice y elemento
print("\nCon numeración:")
for indice, fruta in enumerate(frutas, 1):
    print(f"{indice}. {fruta}")

# Modificar mientras recorres (crear nueva lista)
frutas_mayusculas = []
for fruta in frutas:
    frutas_mayusculas.append(fruta.upper())
print(f"En mayúsculas: {frutas_mayusculas}")
```

### For con texto

```python
nombre = "Python"

print("Letra por letra:")
for letra in nombre:
    print(f"-> {letra}")

# Contar vocales
vocales = "aeiou"
contador_vocales = 0

for letra in nombre.lower():
    if letra in vocales:
        contador_vocales += 1

print(f"'{nombre}' tiene {contador_vocales} vocales")
```

### While - Mientras algo sea verdadero

```python
# Contador simple
contador = 1
print("Contando hasta 5:")

while contador <= 5:
    print(f"Vuelta {contador}")
    contador += 1  # ¡IMPORTANTE! Sin esto es bucle infinito

print("Terminé de contar")
```

### While para validar datos

```python
# Pedir edad válida
while True:
    edad_input = input("¿Cuál es tu edad? ")
    
    if edad_input.isdigit():  # ¿Es solo números?
        edad = int(edad_input)
        if 0 <= edad <= 120:  # ¿Es una edad razonable?
            break  # Salir del bucle
        else:
            print("Edad no válida. Debe ser entre 0 y 120.")
    else:
        print("Por favor, ingresa solo números.")

print(f"Tu edad es {edad} años")
```

### Control de bucles

```python
# Continue - saltar a la siguiente iteración
print("Números impares del 1 al 10:")
for i in range(1, 11):
    if i % 2 == 0:  # Si es par
        continue    # Salta el resto y va al siguiente
    print(i)

print("\nBuscando el número 7:")
numeros = [1, 3, 5, 7, 9, 11, 13]
for numero in numeros:
    if numero == 7:
        print(f"¡Encontré el {numero}!")
        break  # Sale del bucle completamente
    print(f"Revisando: {numero}")
```

### Bucles anidados

```python
# Tabla de multiplicar
print("Tabla del 1 al 3:")
for tabla in range(1, 4):
    print(f"\nTabla del {tabla}:")
    for multiplicador in range(1, 6):
        resultado = tabla * multiplicador
        print(f"{tabla} × {multiplicador} = {resultado}")
```

---

## ⚙️ Funciones - Crear tus propias herramientas

### Funciones básicas

```python
# Función simple
def saludar():
    print("¡Hola a todos!")

# Usarla
saludar()  # ¡Hola a todos!
saludar()  # ¡Hola a todos!

# Función con parámetro
def saludar_persona(nombre):
    print(f"¡Hola, {nombre}!")

saludar_persona("Ana")    # ¡Hola, Ana!
saludar_persona("Carlos") # ¡Hola, Carlos!
```

### Funciones que devuelven valores

```python
# Función que calcula y devuelve
def sumar(a, b):
    resultado = a + b
    return resultado

# Usarla
total = sumar(5, 3)
print(f"5 + 3 = {total}")  # 8

# Más directa
def multiplicar(a, b):
    return a * b

print(f"4 × 7 = {multiplicar(4, 7)}")  # 28
```

### Parámetros por defecto

```python
def presentar(nombre, edad=25, ciudad="Madrid"):
    return f"Hola, soy {nombre}, tengo {edad} años y vivo en {ciudad}"

# Diferentes formas de usarla
print(presentar("Juan"))  # Usa valores por defecto
print(presentar("Ana", 30))  # Cambia solo edad
print(presentar("Luis", ciudad="Barcelona"))  # Parámetro específico
print(presentar("María", 28, "Valencia"))  # Todos los valores
```

### Funciones útiles del día a día

```python
def calcular_propina(total_cuenta, porcentaje=15):
    """Calcula la propina basada en el total y porcentaje"""
    propina = total_cuenta * (porcentaje / 100)
    total_final = total_cuenta + propina
    return propina, total_final

# Usar la función
cuenta = 50.0
propina, total = calcular_propina(cuenta)
print(f"Cuenta: ${cuenta:.2f}")
print(f"Propina (15%): ${propina:.2f}")
print(f"Total a pagar: ${total:.2f}")

def es_email_valido(email):
    """Verifica si un email tiene formato básico válido"""
    return "@" in email and "." in email and len(email) > 5

# Probar emails
emails = ["juan@gmail.com", "correo_malo", "ana@hotmail.es"]
for email in emails:
    valido = es_email_valido(email)
    print(f"{email}: {'✓ Válido' if valido else '✗ Inválido'}")
```

### Funciones con múltiples retornos

```python
def analizar_numeros(lista_numeros):
    """Analiza una lista de números y devuelve estadísticas"""
    if not lista_numeros:  # Si está vacía
        return 0, 0, 0
    
    minimo = min(lista_numeros)
    maximo = max(lista_numeros)
    promedio = sum(lista_numeros) / len(lista_numeros)
    
    return minimo, maximo, promedio

# Usar la función
numeros = [10, 5, 8, 15, 3, 12]
min_val, max_val, prom = analizar_numeros(numeros)

print(f"Números: {numeros}")
print(f"Mínimo: {min_val}")
print(f"Máximo: {max_val}")
print(f"Promedio: {prom:.2f}")
```

---

## 📚 Diccionarios - Organizar datos como una agenda

### Crear y usar diccionarios

```python
# Crear diccionario
persona = {
    "nombre": "Ana",
    "edad": 28,
    "ciudad": "Madrid",
    "tiene_mascotas": True
}

print(persona)

# Diccionario vacío
contactos = {}
```

### Acceder y modificar

```python
estudiante = {
    "nombre": "Carlos",
    "edad": 20,
    "materias": ["Matemáticas", "Física"],
    "promedio": 8.5
}

# Acceder a valores
nombre = estudiante["nombre"]
edad = estudiante["edad"]
print(f"{nombre} tiene {edad} años")

# Forma segura (no da error si no existe)
telefono = estudiante.get("telefono", "No registrado")
print(f"Teléfono: {telefono}")

# Agregar/modificar
estudiante["telefono"] = "123-456-789"
estudiante["edad"] = 21  # Cambiar valor existente

print(estudiante)
```

### Operaciones con diccionarios

```python
productos = {
    "laptop": 1200,
    "mouse": 25,
    "teclado": 80,
    "monitor": 300
}

# Ver claves, valores o todo
print("Productos:", list(productos.keys()))
print("Precios:", list(productos.values()))

# Verificar si existe
if "laptop" in productos:
    print(f"La laptop cuesta ${productos['laptop']}")

# Recorrer diccionario
print("\nCatálogo completo:")
for producto, precio in productos.items():
    print(f"{producto}: ${precio}")

# Agregar múltiples elementos
nuevos_productos = {"webcam": 150, "altavoces": 80}
productos.update(nuevos_productos)

# Eliminar elementos
precio_mouse = productos.pop("mouse", 0)  # Elimina y devuelve valor
print(f"Eliminé el mouse que costaba ${precio_mouse}")
```

### Diccionarios anidados

```python
empresa = {
    "nombre": "TechCorp",
    "empleados": {
        "ana": {"edad": 30, "puesto": "Desarrolladora"},
        "luis": {"edad": 25, "puesto": "Diseñador"},
        "maria": {"edad": 35, "puesto": "Gerente"}
    },
    "oficinas": ["Madrid", "Barcelona", "Valencia"]
}

# Acceder a datos anidados
print(f"Empresa: {empresa['nombre']}")
print(f"Ana es {empresa['empleados']['ana']['puesto']}")
print(f"Oficinas: {', '.join(empresa['oficinas'])}")

# Agregar nuevo empleado
empresa["empleados"]["carlos"] = {"edad": 28, "puesto": "Analista"}
```

---

## 🛡️ Manejo de errores - Que no explote tu programa

### Try-except básico

```python
# Sin manejo de errores (malo)
# numero = int(input("Número: "))  # ¡Error si escriben texto!

# Con manejo de errores (bueno)
try:
    numero = int(input("Escribe un número: "))
    doble = numero * 2
    print(f"El doble de {numero} es {doble}")
except ValueError:
    print("Eso no es un número válido")

print("El programa continúa sin problemas")
```

### Diferentes tipos de errores

```python
def division_segura(a, b):
    try:
        resultado = a / b
        return resultado
    except ZeroDivisionError:
        print("No se puede dividir entre cero")
        return None
    except TypeError:
        print("Solo puedo dividir números")
        return None

# Probar diferentes casos
print(division_segura(10, 2))    # 5.0
print(division_segura(10, 0))    # None (error controlado)
print(division_segura(10, "a"))  # None (error controlado)
```

### Try-except completo con finally

```python
def leer_lista_compras():
    nombre_archivo = "compras.txt"
    
    try:
        with open(nombre_archivo, "r") as archivo:
            contenido = archivo.read()
            return contenido.split("\n")
    except FileNotFoundError:
        print(f"No existe el archivo {nombre_archivo}")
        return []
    except PermissionError:
        print("No tienes permisos para leer el archivo")
        return []
    except Exception as e:
        print(f"Error inesperado: {e}")
        return []
    finally:
        print("Operación de archivo terminada")

# compras = leer_lista_compras()
```

### Función robusta con validación

```python
def obtener_edad_valida():
    """Pide edad hasta que sea válida"""
    intentos = 0
    max_intentos = 3
    
    while intentos < max_intentos:
        try:
            edad_input = input(f"Tu edad (intento {intentos + 1}/3): ")
            edad = int(edad_input)
            
            if 0 <= edad <= 120:
                return edad
            else:
                print("La edad debe estar entre 0 y 120 años")
                
        except ValueError:
            print("Por favor, ingresa solo números")
            
        intentos += 1
    
    print("Demasiados intentos fallidos")
    return None

# edad = obtener_edad_valida()
# if edad is not None:
#     print(f"Tu edad es {edad}")
```

---

## 📁 Trabajar con archivos - Guardar y leer información

### Escribir archivos

```python
# Escribir texto simple
with open("mi_diario.txt", "w") as archivo:
    archivo.write("Hoy aprendí Python intermedio\n")
    archivo.write("Los diccionarios son geniales\n")
    archivo.write("Mañana practicaré más\n")

print("Diario guardado")

# Escribir lista de elementos
tareas = ["Estudiar Python", "Hacer ejercicio", "Leer un libro"]

with open("tareas.txt", "w") as archivo:
    for tarea in tareas:
        archivo.write(f"- {tarea}\n")

print("Tareas guardadas")
```

### Leer archivos

```python
# Leer todo el archivo
try:
    with open("mi_diario.txt", "r") as archivo:
        contenido = archivo.read()
        print("=== MI DIARIO ===")
        print(contenido)
except FileNotFoundError:
    print("El archivo no existe")

# Leer línea por línea
try:
    with open("tareas.txt", "r") as archivo:
        print("=== MIS TAREAS ===")
        for numero, linea in enumerate(archivo, 1):
            tarea = linea.strip()  # Quita el \n del final
            print(f"{numero}. {tarea}")
except FileNotFoundError:
    print("No hay archivo de tareas")
```

### Agregar al final del archivo

```python
# Agregar sin borrar lo anterior
nueva_entrada = input("¿Qué hiciste hoy? ")

with open("mi_diario.txt", "a") as archivo:  # 'a' de append
    archivo.write(f"\n{nueva_entrada}\n")

print("Entrada agregada al diario")
```

### Proyecto práctico: Gestor de contactos

```python
def cargar_contactos():
    """Carga contactos desde archivo"""
    try:
        contactos = {}
        with open("contactos.txt", "r") as archivo:
            for linea in archivo:
                if ":" in linea:
                    nombre, telefono = linea.strip().split(":", 1)
                    contactos[nombre] = telefono
        return contactos
    except FileNotFoundError:
        return {}

def guardar_contactos(contactos):
    """Guarda contactos en archivo"""
    with open("contactos.txt", "w") as archivo:
        for nombre, telefono in contactos.items():
            archivo.write(f"{nombre}:{telefono}\n")

def mostrar_contactos(contactos):
    """Muestra todos los contactos"""
    if not contactos:
        print("No hay contactos guardados")
        return
    
    print("\n=== MIS CONTACTOS ===")
    for nombre, telefono in contactos.items():
        print(f"{nombre}: {telefono}")

def agregar_contacto(contactos):
    """Agrega un nuevo contacto"""
    nombre = input("Nombre: ").strip()
    telefono = input("Teléfono: ").strip()
    
    if nombre and telefono:
        contactos[nombre] = telefono
        print(f"✓ Contacto {nombre} agregado")
    else:
        print("Nombre y teléfono son obligatorios")

def buscar_contacto(contactos):
    """Busca un contacto por nombre"""
    nombre = input("¿Qué contacto buscas? ").strip()
    
    if nombre in contactos:
        print(f"{nombre}: {contactos[nombre]}")
    else:
        print(f"No encontré el contacto '{nombre}'")

# Programa principal
def main():
    contactos = cargar_contactos()
    
    while True:
        print("\n=== GESTOR DE CONTACTOS ===")
        print("1. Ver contactos")
        print("2. Agregar contacto")
        print("3. Buscar contacto")
        print("4. Salir")
        
        opcion = input("Elige opción: ").strip()
        
        if opcion == "1":
            mostrar_contactos(contactos)
        elif opcion == "2":
            agregar_contacto(contactos)
            guardar_contactos(contactos)
        elif opcion == "3":
            buscar_contacto(contactos)
        elif opcion == "4":
            print("¡Hasta luego!")
            break
        else:
            print("Opción no válida")

# Ejecutar solo si es el archivo principal
# if __name__ == "__main__":
#     main()
```

---

## 🎯 Ejercicios para practicar

### 1. Calculadora con menú

```python
def calculadora_avanzada():
    while True:
        print("\n=== CALCULADORA AVANZADA ===")
        print("1. Sumar")
        print("2. Restar")
        print("3. Multiplicar")
        print("4. Dividir")
        print("5. Potencia")
        print("6. Salir")
        
        opcion = input("Elige operación: ")
        
        if opcion == "6":
            print("¡Adiós!")
            break
        
        if opcion not in ["1", "2", "3", "4", "5"]:
            print("Opción no válida")
            continue
        
        try:
            a = float(input("Primer número: "))
            b = float(input("Segundo número: "))
            
            if opcion == "1":
                resultado = a + b
                operacion = "+"
            elif opcion == "2":
                resultado = a - b
                operacion = "-"
            elif opcion == "3":
                resultado = a * b
                operacion = "×"
            elif opcion == "4":
                if b == 0:
                    print("No se puede dividir entre cero")
                    continue
                resultado = a / b
                operacion = "÷"
            elif opcion == "5":
                resultado = a ** b
                operacion = "^"
            
            print(f"{a} {operacion} {b} = {resultado}")
            
        except ValueError:
            print("Por favor, ingresa números válidos")

# calculadora_avanzada()
```

### 2. Juego de adivinanza mejorado

```python
import random

def juego_adivinanza():
    print("🎯 ¡ADIVINA EL NÚMERO!")
    
    # Elegir dificultad
    print("Elige dificultad:")
    print("1. Fácil (1-50, 10 intentos)")
    print("2. Medio (1-100, 7 intentos)")
    print("3. Difícil (1-200, 5 intentos)")
    
    while True:
        dificultad = input("Dificultad (1-3): ")
        if dificultad == "1":
            max_num, intentos = 50, 10
            break
        elif dificultad == "2":
            max_num, intentos = 100, 7
            break
        elif dificultad == "3":
            max_num, intentos = 200, 5
            break
        else:
            print("Elige 1, 2 o 3")
    
    numero_secreto = random.randint(1, max_num)
    intentos_usados = 0
    
    print(f"\nAdivina el número entre 1 y {max_num}")
    print(f"Tienes {intentos} intentos")
    
    while intentos_usados < intentos:
        try:
            adivinanza = int(input(f"\nIntento {intentos_usados + 1}: "))
            intentos_usados += 1
            
            if adivinanza == numero_secreto:
                print(f"🎉 ¡CORRECTO! Era {numero_secreto}")
                print(f"Lo adivinaste en {intentos_usados} intento(s)")
                return
            elif adivinanza < numero_secreto:
                print("📈 Más alto")
            else:
                print("📉 Más bajo")
            
            restantes = intentos - intentos_usados
            if restantes > 0:
                print(f"Te quedan {restantes} intentos")
            
        except ValueError:
            print("Ingresa un número válido")
            intentos_usados -= 1  # No contar como intento
    
    print(f"\n💀 ¡Perdiste! El número era {numero_secreto}")

# juego_adivinanza()
```

### 3. Analizador de texto avanzado

```python
def analizar_texto_completo():
    texto = input("Escribe un texto para analizar: ")
    
    # Estadísticas básicas
    caracteres_total = len(texto)
    caracteres_sin_espacios = len(texto.replace(" ", ""))
    palabras = texto.split()
    num_palabras = len(palabras)
    num_lineas = texto.count("\n") + 1
    
    # Análisis de caracteres
    vocales = "aeiouáéíóúAEIOUÁÉÍÓÚ"
    consonantes = "bcdfghjklmnpqrstvwxyzBCDFGHJKLMNPQRSTVWXYZ"
    numeros = "0123456789"
    
    num_vocales = sum(1 for char in texto if char in vocales)
    num_consonantes = sum(1 for char in texto if char in consonantes)
    num_numeros = sum(1 for char in texto if char in numeros)
    num_espacios = texto.count(" ")
    
    # Análisis de palabras
    if palabras:
        palabra_mas_larga = max(palabras, key=len)
        palabra_mas_corta = min(palabras, key=len)
        promedio_longitud = sum(len(palabra) for palabra in palabras) / len(palabras)
    else:
        palabra_mas_larga = palabra_mas_corta = "N/A"
        promedio_longitud = 0
    
    # Frecuencia de palabras
    frecuencia_palabras = {}
    for palabra in palabras:
        palabra_limpia = palabra.lower().strip(".,!?;:")
        if palabra_limpia:
            frecuencia_palabras[palabra_limpia] = frecuencia_palabras.get(palabra_limpia, 0) + 1
    
    # Mostrar resultados
    print(f"\n{'='*50}")
    print("ANÁLISIS COMPLETO DEL TEXTO")
    print(f"{'='*50}")
    
    print(f"\n📊 ESTADÍSTICAS GENERALES:")
    print(f"Caracteres totales: {caracteres_total}")
    print(f"Caracteres sin espacios: {caracteres_sin_espacios}")
    print(f"Palabras: {num_palabras}")
    print(f"Líneas: {num_lineas}")
    print(f"Espacios: {num_espacios}")
    
    print(f"\n🔤 ANÁLISIS DE CARACTERES:")
    print(f"Vocales: {num_vocales}")
    print(f"Consonantes: {num_consonantes}")
    print(f"Números: {num_numeros}")
    
    print(f"\n📝 ANÁLISIS DE PALABRAS:")
    print(f"Palabra más larga: '{palabra_mas_larga}' ({len(palabra_mas_larga)} caracteres)")
    print(f"Palabra más corta: '{palabra_mas_corta}' ({len(palabra_mas_corta)} caracteres)")
    print(f"Promedio de longitud: {promedio_longitud:.1f} caracteres")
    
    if frecuencia_palabras:
        palabra_frecuente = max(frecuencia_palabras.items(), key=lambda x: x[1])
        print(f"Palabra más frecuente: '{palabra_frecuente[0]}' ({palabra_frecuente[1]} veces)")
        
        print(f"\n🔄 PALABRAS REPETIDAS:")
        repetidas = {palabra: freq for palabra, freq in frecuencia_palabras.items() if freq > 1}
        if repetidas:
            for palabra, freq in sorted(repetidas.items(), key=lambda x: x[1], reverse=True):
                print(f"'{palabra}': {freq} veces")
        else:
            print("No hay palabras repetidas")

# analizar_texto_completo()
```

---

## 💡 Consejos y mejores prácticas

### Nombres de funciones y variables

```python
# ❌ Malo
def f(x, y):
    return x + y

# ✅ Bueno
def calcular_suma(numero1, numero2):
    return numero1 + numero2

# ❌ Variables confusas
d = {"n": "Juan", "e": 25}

# ✅ Variables claras
persona = {"nombre": "Juan", "edad": 25}
```

### Docstrings (documentar funciones)

```python
def calcular_precio_final(precio_base, descuento=0, impuesto=0.21):
    """
    Calcula el precio final de un producto aplicando descuento e impuestos.
    
    Args:
        precio_base (float): Precio original del producto
        descuento (float): Descuento a aplicar (0.1 = 10%)
        impuesto (float): Impuesto a aplicar (0.21 = 21%)
    
    Returns:
        float: Precio final con descuento e impuestos aplicados
    
    Example:
        >>> calcular_precio_final(100, 0.1, 0.21)
        108.9
    """
    precio_con_descuento = precio_base * (1 - descuento)
    precio_final = precio_con_descuento * (1 + impuesto)
    return precio_final
```

### Validación de datos

```python
def crear_usuario(nombre, edad, email):
    """Crea un usuario validando los datos"""
    errores = []
    
    # Validar nombre
    if not nombre or len(nombre.strip()) < 2:
        errores.append("El nombre debe tener al menos 2 caracteres")
    
    # Validar edad
    if not isinstance(edad, int) or edad < 0 or edad > 120:
        errores.append("La edad debe ser un número entre 0 y 120")
    
    # Validar email
    if not email or "@" not in email or "." not in email:
        errores.append("El email no tiene un formato válido")
    
    if errores:
        print("Errores encontrados:")
        for error in errores:
            print(f"- {error}")
        return None
    
    usuario = {
        "nombre": nombre.strip().title(),
        "edad": edad,
        "email": email.lower().strip()
    }
    
    return usuario

# Ejemplo de uso
usuario = crear_usuario("juan", 25, "juan@email.com")
if usuario:
    print(f"Usuario creado: {usuario}")
```

---

## 🚀 ¿Qué sigue después?

Una vez que domines TODO lo de esta guía, estarás listo para **Python AVANZADO**:

- Clases y programación orientada a objetos
- Decoradores y generadores
- Manejo avanzado de archivos (JSON, CSV)
- Librerías especializadas (requests, pandas, etc.)
- Frameworks web (Flask, Django)
- APIs y bases de datos

**Mi consejo:** Practica mucho con proyectos reales. Haz un gestor de tareas, una calculadora avanzada, un analizador de archivos, etc.

---

## 📚 Recursos recomendados

**Para seguir practicando:**
- HackerRank (ejercicios de programación)
- Codewars (desafíos de código)
- LeetCode (problemas algorítmicos)
- Real Python (tutoriales avanzados)

**Proyectos para hacer:**
- Sistema de gestión de biblioteca
- Calculadora científica
- Analizador de logs
- Juegos simples (ahorcado, tres en raya)
- Bot de chat básico

---

**📝 Nota de Elvis:** "El intermedio es donde Python se vuelve realmente poderoso. Ya no solo haces cálculos simples, sino que creas programas que resuelven problemas reales. ¡La magia está empezando!"

*Creado: 18 de octubre de 2025*  
*Nivel: Intermedio*  
*Duración: 4-5 semanas de práctica constante*  
*Requisito: Dominar Python principiante*

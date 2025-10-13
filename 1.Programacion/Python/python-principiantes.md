# Python para Principiantes - Mis Apuntes

Estos son mis apuntes de cuando empecé a aprender Python. Lo escribo como si le estuviera explicando a un amigo que nunca ha programado.

---

## ¿Qué es Python?

Python es un lenguaje de programación que es súper fácil de leer y escribir. Es como hablar con la computadora en un inglés simplificado.

**¿Por qué Python?**
- Es fácil de aprender (perfecto para empezar)
- Se usa para TODO: páginas web, inteligencia artificial, análisis de datos, automatización
- Hay un montón de librerías ya hechas
- La comunidad es súper amigable

---

## Primeros pasos

### Instalar Python

Ve a [python.org](https://python.org) y descarga la versión más nueva. En Windows, asegúrate de marcar "Add to PATH" cuando instales.

### Tu primer programa

```python
print("¡Hola mundo!")
```

Eso es todo. Así de simple. `print()` le dice a Python "muestra esto en pantalla".

### Comentarios

```python
# Esto es un comentario - Python lo ignora
print("Esto sí se ejecuta")  # También puedes poner comentarios al final

"""
Esto es un comentario
de varias líneas
"""
```

Los comentarios son para ti (y otros programadores) para explicar qué hace el código.

---

## Variables - Cajitas para guardar cosas

Una variable es como una cajita donde guardas información con un nombre.

```python
# Números
edad = 25
precio = 19.99

# Texto (strings)
nombre = "Juan"
mensaje = 'También puedes usar comillas simples'

# Verdadero/Falso (booleanos)
es_estudiante = True
tiene_trabajo = False

# Mostrar las variables
print(nombre)           # Juan
print("Mi edad es", edad)  # Mi edad es 25
```

**Reglas para nombres de variables:**
- No puede empezar con número: ❌ `2nombre` ✅ `nombre2`
- Sin espacios: ❌ `mi nombre` ✅ `mi_nombre`
- Sin caracteres especiales: ❌ `edad@` ✅ `edad`

### Tipos de datos básicos

```python
# Enteros (int)
numero = 42
print(type(numero))  # <class 'int'>

# Decimales (float)
precio = 15.99
print(type(precio))  # <class 'float'>

# Texto (string)
saludo = "Hola"
print(type(saludo))  # <class 'str'>

# Booleano (bool)
activo = True
print(type(activo))  # <class 'bool'>
```

**Truco:** `type()` te dice qué tipo de dato es algo.

---

## Trabajando con números

### Operaciones básicas

```python
# Las operaciones matemáticas normales
suma = 10 + 5        # 15
resta = 10 - 5       # 5
multiplicacion = 10 * 5  # 50
division = 10 / 5    # 2.0 (siempre da decimal)

# División entera (sin decimales)
division_entera = 10 // 3  # 3

# Resto/módulo
resto = 10 % 3       # 1 (el resto de dividir 10 entre 3)

# Potencia
potencia = 2 ** 3    # 8 (2 elevado a 3)

print(f"10 + 5 = {suma}")
print(f"10 / 3 = {10/3}")      # 3.3333...
print(f"10 // 3 = {10//3}")    # 3
print(f"10 % 3 = {10%3}")      # 1
```

### Operaciones útiles

```python
# Valor absoluto
numero = -5
positivo = abs(numero)  # 5

# Redondear
pi = 3.14159
redondeado = round(pi, 2)  # 3.14 (2 decimales)

# Máximo y mínimo
mayor = max(10, 5, 8, 20)  # 20
menor = min(10, 5, 8, 20)  # 5

# Convertir tipos
texto = "123"
numero = int(texto)      # 123 (convierte texto a número)
decimal = float(texto)   # 123.0

numero = 456
texto = str(numero)      # "456" (convierte número a texto)

print(f"El número {numero} como texto es '{texto}'")
```

---

## Trabajando con texto (strings)

### Operaciones básicas con texto

```python
nombre = "Juan"
apellido = "Pérez"

# Unir textos (concatenar)
nombre_completo = nombre + " " + apellido
print(nombre_completo)  # Juan Pérez

# Repetir texto
risa = "ja" * 3
print(risa)  # jajaja

# Longitud del texto
mensaje = "Hola mundo"
cuantas_letras = len(mensaje)
print(f"'{mensaje}' tiene {cuantas_letras} caracteres")  # 10
```

### Métodos útiles para strings

```python
texto = "Hola Mundo Python"

# Mayúsculas y minúsculas
print(texto.upper())      # HOLA MUNDO PYTHON
print(texto.lower())      # hola mundo python
print(texto.title())      # Hola Mundo Python
print(texto.capitalize()) # Hola mundo python

# Buscar en el texto
print("Python" in texto)     # True
print("Java" in texto)       # False
print(texto.find("Mundo"))   # 5 (posición donde está)
print(texto.count("o"))      # 2 (cuántas veces aparece "o")

# Reemplazar
nuevo_texto = texto.replace("Python", "Java")
print(nuevo_texto)  # Hola Mundo Java

# Dividir el texto
palabras = texto.split(" ")  # Divide por espacios
print(palabras)  # ['Hola', 'Mundo', 'Python']

# Quitar espacios
texto_con_espacios = "  Hola mundo  "
limpio = texto_con_espacios.strip()
print(f"'{limpio}'")  # 'Hola mundo'
```

### Formatear strings (f-strings)

```python
nombre = "Ana"
edad = 28
altura = 1.65

# La forma más fácil (f-strings)
mensaje = f"Hola, soy {nombre}, tengo {edad} años y mido {altura}m"
print(mensaje)

# Con operaciones dentro
precio = 100
descuento = 0.15
mensaje = f"Precio final: ${precio * (1 - descuento):.2f}"
print(mensaje)  # Precio final: $85.00

# Otras formas (más viejas, pero las verás por ahí)
mensaje = "Hola, soy {}, tengo {} años".format(nombre, edad)
mensaje = "Hola, soy %s, tengo %d años" % (nombre, edad)
```

**Tip:** Usa f-strings, son más fáciles y modernas.

---

## Listas - Como cajones con compartimentos

Las listas son para guardar varios elementos en orden.

```python
# Crear listas
frutas = ["manzana", "banana", "naranja"]
numeros = [1, 2, 3, 4, 5]
mixta = ["Juan", 25, True, 3.14]  # Puede tener tipos diferentes

# Lista vacía
vacia = []
tambien_vacia = list()

print(frutas)  # ['manzana', 'banana', 'naranja']
```

### Acceder a elementos

```python
frutas = ["manzana", "banana", "naranja", "uva"]

# Por posición (empezando desde 0)
primera = frutas[0]     # "manzana"
segunda = frutas[1]     # "banana"
ultima = frutas[-1]     # "uva" (desde el final)
penultima = frutas[-2]  # "naranja"

print(f"Primera fruta: {primera}")
print(f"Última fruta: {ultima}")

# Cuántos elementos hay
cantidad = len(frutas)
print(f"Hay {cantidad} frutas")
```

### Modificar listas

```python
colores = ["rojo", "verde", "azul"]

# Cambiar un elemento
colores[1] = "amarillo"
print(colores)  # ['rojo', 'amarillo', 'azul']

# Agregar al final
colores.append("morado")
print(colores)  # ['rojo', 'amarillo', 'azul', 'morado']

# Agregar en posición específica
colores.insert(0, "negro")  # Agregar al inicio
print(colores)  # ['negro', 'rojo', 'amarillo', 'azul', 'morado']

# Quitar elementos
colores.remove("amarillo")  # Quitar por valor
print(colores)  # ['negro', 'rojo', 'azul', 'morado']

ultimo = colores.pop()      # Quitar el último
print(f"Quité: {ultimo}")   # Quité: morado
print(colores)              # ['negro', 'rojo', 'azul']

# Quitar por posición
segundo = colores.pop(1)    # Quitar el índice 1
print(f"Quité: {segundo}")  # Quité: rojo
```

### Operaciones útiles con listas

```python
numeros = [3, 1, 4, 1, 5, 9, 2]

# Ordenar
numeros_ordenados = sorted(numeros)  # Nueva lista ordenada
print(numeros_ordenados)  # [1, 1, 2, 3, 4, 5, 9]

numeros.sort()  # Modifica la lista original
print(numeros)  # [1, 1, 2, 3, 4, 5, 9]

# Voltear
numeros.reverse()
print(numeros)  # [9, 5, 4, 3, 2, 1, 1]

# Encontrar cosas
animales = ["gato", "perro", "pez", "gato"]
print("gato" in animales)           # True
print(animales.index("perro"))      # 1 (posición)
print(animales.count("gato"))       # 2 (cuántas veces)

# Unir listas
lista1 = [1, 2, 3]
lista2 = [4, 5, 6]
juntas = lista1 + lista2
print(juntas)  # [1, 2, 3, 4, 5, 6]

# O extender
lista1.extend(lista2)
print(lista1)  # [1, 2, 3, 4, 5, 6]
```

### Slicing (rebanar listas)

```python
letras = ['a', 'b', 'c', 'd', 'e', 'f', 'g']

# Tomar pedazos
primeras_tres = letras[0:3]    # ['a', 'b', 'c']
desde_tercera = letras[2:]     # ['c', 'd', 'e', 'f', 'g']
hasta_cuarta = letras[:4]      # ['a', 'b', 'c', 'd']
ultimas_dos = letras[-2:]      # ['f', 'g']

# Con saltos
cada_dos = letras[::2]         # ['a', 'c', 'e', 'g']
alreves = letras[::-1]         # ['g', 'f', 'e', 'd', 'c', 'b', 'a']

print(f"Primeras 3: {primeras_tres}")
print(f"Cada 2: {cada_dos}")
print(f"Al revés: {alreves}")
```

---

## Tuplas - Listas que no cambian

Las tuplas son como listas, pero no puedes modificarlas después de crearlas.

```python
# Crear tuplas
coordenadas = (10, 20)
colores = ("rojo", "verde", "azul")
datos = ("Juan", 25, True)

# Sin paréntesis también funciona
punto = 5, 10
print(punto)  # (5, 10)

# Tupla de un elemento (necesita la coma)
un_elemento = (42,)
print(type(un_elemento))  # <class 'tuple'>

# Acceder igual que las listas
x = coordenadas[0]  # 10
y = coordenadas[1]  # 20

# Desempaquetar
nombre, edad, activo = datos
print(f"{nombre} tiene {edad} años")

# Intercambiar variables (súper útil)
a = 5
b = 10
a, b = b, a  # ¡Magia!
print(f"a={a}, b={b}")  # a=10, b=5
```

**¿Cuándo usar tuplas?**
- Para datos que no van a cambiar (coordenadas, RGB)
- Para devolver múltiples valores de una función
- Como claves de diccionarios

---

## Diccionarios - Como una agenda telefónica

Los diccionarios guardan pares de clave-valor, como una agenda donde el nombre es la clave y el teléfono es el valor.

```python
# Crear diccionarios
persona = {
    "nombre": "Ana",
    "edad": 28,
    "ciudad": "Madrid",
    "activo": True
}

# Diccionario vacío
vacio = {}
tambien_vacio = dict()

print(persona)
```

### Trabajar con diccionarios

```python
estudiante = {
    "nombre": "Carlos",
    "edad": 20,
    "materias": ["Matemáticas", "Física", "Química"],
    "promedio": 8.5
}

# Acceder a valores
nombre = estudiante["nombre"]
edad = estudiante["edad"]
print(f"{nombre} tiene {edad} años")

# Forma segura (no da error si no existe)
telefono = estudiante.get("telefono", "No tiene")
print(f"Teléfono: {telefono}")

# Agregar/modificar
estudiante["telefono"] = "123-456-789"
estudiante["edad"] = 21  # Cambiar valor existente

# Quitar elementos
del estudiante["activo"]  # Si existe
telefono = estudiante.pop("telefono", None)  # Forma segura

print(estudiante)
```

### Operaciones útiles

```python
productos = {
    "laptop": 1200,
    "mouse": 25,
    "teclado": 80,
    "monitor": 300
}

# Ver todas las claves, valores o pares
claves = productos.keys()
valores = productos.values()
pares = productos.items()

print("Productos:", list(claves))
print("Precios:", list(valores))

# Verificar si existe una clave
if "laptop" in productos:
    print(f"La laptop cuesta ${productos['laptop']}")

# Recorrer diccionario
for producto, precio in productos.items():
    print(f"{producto}: ${precio}")

# Actualizar con otro diccionario
nuevos_productos = {"webcam": 150, "altavoces": 80}
productos.update(nuevos_productos)
print(productos)
```

---

## Estructuras de control

### If - Tomar decisiones

```python
edad = 18

# If básico
if edad >= 18:
    print("Eres mayor de edad")

# If-else
if edad >= 18:
    print("Puedes votar")
else:
    print("Aún no puedes votar")

# If-elif-else (múltiples condiciones)
nota = 85

if nota >= 90:
    calificacion = "A"
elif nota >= 80:
    calificacion = "B"
elif nota >= 70:
    calificacion = "C"
elif nota >= 60:
    calificacion = "D"
else:
    calificacion = "F"

print(f"Tu calificación es: {calificacion}")
```

### Operadores de comparación

```python
a = 10
b = 5

print(a == b)   # False (igual)
print(a != b)   # True (diferente)
print(a > b)    # True (mayor que)
print(a < b)    # False (menor que)
print(a >= b)   # True (mayor o igual)
print(a <= b)   # False (menor o igual)

# Con strings
nombre1 = "Ana"
nombre2 = "ana"
print(nombre1 == nombre2)        # False (case sensitive)
print(nombre1.lower() == nombre2.lower())  # True
```

### Operadores lógicos

```python
edad = 25
tiene_licencia = True
tiene_auto = False

# AND - ambas condiciones deben ser verdaderas
puede_manejar = edad >= 18 and tiene_licencia
print(f"¿Puede manejar? {puede_manejar}")

# OR - al menos una condición debe ser verdadera
puede_ir_al_trabajo = tiene_auto or edad < 30
print(f"¿Puede ir al trabajo? {puede_ir_al_trabajo}")

# NOT - invierte el valor
no_tiene_auto = not tiene_auto
print(f"¿No tiene auto? {no_tiene_auto}")

# Combinando
if edad >= 18 and (tiene_licencia or tiene_auto):
    print("Puede considerar manejar")
```

### Verificar contenido

```python
# Con listas
frutas = ["manzana", "banana", "naranja"]
if "manzana" in frutas:
    print("Tenemos manzanas")

# Con strings
email = "usuario@gmail.com"
if "@" in email and "." in email:
    print("El email parece válido")

# Con diccionarios
persona = {"nombre": "Juan", "edad": 30}
if "edad" in persona:
    print(f"La edad es {persona['edad']}")
```

---

## Bucles - Repetir cosas

### For - Cuando sabes cuántas veces repetir

```python
# Repetir con números
for i in range(5):
    print(f"Vuelta número {i}")  # 0, 1, 2, 3, 4

# range con inicio y fin
for i in range(2, 8):
    print(i)  # 2, 3, 4, 5, 6, 7

# range con saltos
for i in range(0, 10, 2):
    print(i)  # 0, 2, 4, 6, 8

# Recorrer listas
frutas = ["manzana", "banana", "naranja"]
for fruta in frutas:
    print(f"Me gusta la {fruta}")

# Con índice y valor
for indice, fruta in enumerate(frutas):
    print(f"{indice}: {fruta}")

# Recorrer strings
nombre = "Python"
for letra in nombre:
    print(letra)  # P, y, t, h, o, n

# Recorrer diccionarios
edades = {"Ana": 25, "Luis": 30, "María": 28}

# Solo las claves
for nombre in edades:
    print(nombre)

# Claves y valores
for nombre, edad in edades.items():
    print(f"{nombre} tiene {edad} años")
```

### While - Mientras algo sea verdadero

```python
# Contar hasta 5
contador = 1
while contador <= 5:
    print(f"Contador: {contador}")
    contador += 1  # Igual que contador = contador + 1

# Pedir datos hasta que sean válidos
while True:
    edad = input("¿Cuál es tu edad? ")
    if edad.isdigit() and int(edad) > 0:
        edad = int(edad)
        break  # Salir del bucle
    print("Por favor, ingresa un número válido")

print(f"Tu edad es {edad}")

# Buscar en una lista
numeros = [1, 3, 7, 12, 15, 20]
objetivo = 12
posicion = 0

while posicion < len(numeros):
    if numeros[posicion] == objetivo:
        print(f"Encontrado en posición {posicion}")
        break
    posicion += 1
else:
    print("No encontrado")  # Se ejecuta si no se usó break
```

### Control de bucles

```python
# Continue - saltar a la siguiente iteración
for i in range(10):
    if i % 2 == 0:  # Si es par
        continue    # Saltar el resto
    print(f"Número impar: {i}")

# Break - salir del bucle completamente
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
for numero in numeros:
    if numero > 5:
        break  # Parar cuando encuentra el primer número > 5
    print(numero)

# Bucles anidados
for i in range(3):
    for j in range(3):
        print(f"i={i}, j={j}")
```

---

## Funciones - Reutilizar código

Las funciones son como recetas que puedes usar una y otra vez.

### Funciones básicas

```python
# Definir una función
def saludar():
    print("¡Hola!")

# Usar la función
saludar()  # ¡Hola!

# Función con parámetros
def saludar_persona(nombre):
    print(f"¡Hola, {nombre}!")

saludar_persona("Ana")  # ¡Hola, Ana!

# Función que devuelve algo
def sumar(a, b):
    resultado = a + b
    return resultado

total = sumar(5, 3)
print(f"5 + 3 = {total}")  # 5 + 3 = 8

# Más corto
def restar(a, b):
    return a - b

print(restar(10, 4))  # 6
```

### Parámetros por defecto

```python
def presentar(nombre, edad=25, ciudad="Madrid"):
    print(f"Hola, soy {nombre}, tengo {edad} años y vivo en {ciudad}")

# Diferentes formas de llamarla
presentar("Juan")  # Usa valores por defecto
presentar("Ana", 30)  # Cambia solo la edad
presentar("Luis", ciudad="Barcelona")  # Parámetro nombrado
presentar("María", 28, "Valencia")  # Todos los parámetros
```

### Funciones útiles

```python
# Validar email
def es_email_valido(email):
    return "@" in email and "." in email

print(es_email_valido("juan@gmail.com"))  # True
print(es_email_valido("correo-malo"))     # False

# Calcular área de círculo
def area_circulo(radio):
    pi = 3.14159
    return pi * radio ** 2

print(f"Área: {area_circulo(5):.2f}")  # 78.54

# Encontrar el mayor de una lista
def encontrar_mayor(numeros):
    if not numeros:  # Lista vacía
        return None
    
    mayor = numeros[0]
    for numero in numeros:
        if numero > mayor:
            mayor = numero
    return mayor

lista = [3, 7, 2, 9, 1]
print(f"El mayor es: {encontrar_mayor(lista)}")  # 9

# Contar palabras
def contar_palabras(texto):
    palabras = texto.split()
    return len(palabras)

frase = "Python es un lenguaje genial"
print(f"La frase tiene {contar_palabras(frase)} palabras")  # 5
```

### Múltiples valores de retorno

```python
def estadisticas(numeros):
    if not numeros:
        return 0, 0, 0
    
    minimo = min(numeros)
    maximo = max(numeros)
    promedio = sum(numeros) / len(numeros)
    
    return minimo, maximo, promedio

datos = [1, 5, 3, 9, 2, 7]
min_val, max_val, prom = estadisticas(datos)

print(f"Mínimo: {min_val}")
print(f"Máximo: {max_val}")
print(f"Promedio: {prom:.2f}")
```

---

## Manejo de errores

A veces las cosas salen mal, y hay que estar preparado.

### Try-except básico

```python
# Sin manejo de errores (malo)
# numero = int(input("Ingresa un número: "))  # ¡Error si no es número!

# Con manejo de errores (bueno)
try:
    texto = input("Ingresa un número: ")
    numero = int(texto)
    print(f"El doble es: {numero * 2}")
except ValueError:
    print("Eso no es un número válido")

print("El programa continúa...")
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
        print("Los valores deben ser números")
        return None

print(division_segura(10, 2))    # 5.0
print(division_segura(10, 0))    # None
print(division_segura(10, "a"))  # None

# Múltiples excepciones
def acceder_lista(lista, indice):
    try:
        valor = lista[indice]
        return valor
    except IndexError:
        print(f"Índice {indice} fuera de rango")
        return None
    except TypeError:
        print("El índice debe ser un número")
        return None

mi_lista = [1, 2, 3, 4, 5]
print(acceder_lista(mi_lista, 2))    # 3
print(acceder_lista(mi_lista, 10))   # None
print(acceder_lista(mi_lista, "a"))  # None
```

### Try-except completo

```python
def leer_archivo_seguro(nombre_archivo):
    try:
        with open(nombre_archivo, 'r') as archivo:
            contenido = archivo.read()
        return contenido
    except FileNotFoundError:
        print(f"El archivo {nombre_archivo} no existe")
        return None
    except PermissionError:
        print(f"No tienes permisos para leer {nombre_archivo}")
        return None
    except Exception as e:
        print(f"Error inesperado: {e}")
        return None
    else:
        print("Archivo leído correctamente")  # Se ejecuta si no hay errores
    finally:
        print("Operación de archivo terminada")  # Siempre se ejecuta

# contenido = leer_archivo_seguro("mi_archivo.txt")
```

---

## Trabajar con archivos

### Leer archivos

```python
# Leer todo el archivo
try:
    with open("mi_archivo.txt", "r") as archivo:
        contenido = archivo.read()
        print(contenido)
except FileNotFoundError:
    print("El archivo no existe")

# Leer línea por línea
try:
    with open("mi_archivo.txt", "r") as archivo:
        for numero_linea, linea in enumerate(archivo, 1):
            print(f"Línea {numero_linea}: {linea.strip()}")
except FileNotFoundError:
    print("El archivo no existe")

# Leer todas las líneas en una lista
try:
    with open("mi_archivo.txt", "r") as archivo:
        lineas = archivo.readlines()
        print(f"El archivo tiene {len(lineas)} líneas")
except FileNotFoundError:
    print("El archivo no existe")
```

### Escribir archivos

```python
# Escribir (sobrescribe el archivo)
with open("nuevo_archivo.txt", "w") as archivo:
    archivo.write("Hola mundo\n")
    archivo.write("Esta es la segunda línea\n")

# Agregar al final (no sobrescribe)
with open("nuevo_archivo.txt", "a") as archivo:
    archivo.write("Esta línea se agrega al final\n")

# Escribir múltiples líneas
lineas = ["Primera línea\n", "Segunda línea\n", "Tercera línea\n"]
with open("lineas.txt", "w") as archivo:
    archivo.writelines(lineas)

# Forma práctica
datos = ["Juan", "Ana", "Luis", "María"]
with open("nombres.txt", "w") as archivo:
    for nombre in datos:
        archivo.write(f"{nombre}\n")
```

### Ejemplo práctico: Lista de tareas

```python
def cargar_tareas():
    """Carga las tareas desde archivo"""
    try:
        with open("tareas.txt", "r") as archivo:
            tareas = []
            for linea in archivo:
                tarea = linea.strip()
                if tarea:  # Si no está vacía
                    tareas.append(tarea)
            return tareas
    except FileNotFoundError:
        return []  # Lista vacía si no existe el archivo

def guardar_tareas(tareas):
    """Guarda las tareas en archivo"""
    with open("tareas.txt", "w") as archivo:
        for tarea in tareas:
            archivo.write(f"{tarea}\n")

def mostrar_tareas(tareas):
    """Muestra todas las tareas"""
    if not tareas:
        print("No hay tareas pendientes")
        return
    
    print("\n--- MIS TAREAS ---")
    for i, tarea in enumerate(tareas, 1):
        print(f"{i}. {tarea}")

def agregar_tarea(tareas):
    """Agrega una nueva tarea"""
    nueva_tarea = input("Nueva tarea: ").strip()
    if nueva_tarea:
        tareas.append(nueva_tarea)
        print(f"✅ Agregada: {nueva_tarea}")

def completar_tarea(tareas):
    """Marca una tarea como completada (la elimina)"""
    mostrar_tareas(tareas)
    if not tareas:
        return
    
    try:
        numero = int(input("¿Qué tarea completaste? (número): "))
        if 1 <= numero <= len(tareas):
            tarea_completada = tareas.pop(numero - 1)
            print(f"🎉 Completada: {tarea_completada}")
        else:
            print("Número inválido")
    except ValueError:
        print("Ingresa un número válido")

# Programa principal
def main():
    tareas = cargar_tareas()
    
    while True:
        print("\n--- GESTOR DE TAREAS ---")
        print("1. Ver tareas")
        print("2. Agregar tarea")
        print("3. Completar tarea")
        print("4. Salir")
        
        opcion = input("Elige una opción: ").strip()
        
        if opcion == "1":
            mostrar_tareas(tareas)
        elif opcion == "2":
            agregar_tarea(tareas)
            guardar_tareas(tareas)
        elif opcion == "3":
            completar_tarea(tareas)
            guardar_tareas(tareas)
        elif opcion == "4":
            print("¡Hasta luego!")
            break
        else:
            print("Opción inválida")

# Ejecutar solo si es el archivo principal
if __name__ == "__main__":
    main()
```

---

## Módulos y librerías

Python tiene miles de librerías ya hechas que puedes usar.

### Importar módulos

```python
# Importar módulo completo
import math

print(math.pi)           # 3.141592653589793
print(math.sqrt(16))     # 4.0
print(math.ceil(4.2))    # 5 (redondear hacia arriba)
print(math.floor(4.8))   # 4 (redondear hacia abajo)

# Importar funciones específicas
from math import pi, sqrt, sin, cos

print(pi)        # 3.141592653589793
print(sqrt(25))  # 5.0

# Importar con alias (nombre más corto)
import datetime as dt

ahora = dt.datetime.now()
print(f"Fecha y hora actual: {ahora}")

# Importar todo (no recomendado)
from math import *
print(tan(pi/4))  # 1.0
```

### Módulos útiles

```python
# random - números aleatorios
import random

print(random.randint(1, 10))        # Número entre 1 y 10
print(random.choice(['a', 'b', 'c']))  # Elemento aleatorio
print(random.random())               # Decimal entre 0 y 1

# Barajar lista
cartas = ['A', 'K', 'Q', 'J']
random.shuffle(cartas)
print(cartas)

# datetime - fechas y tiempo
from datetime import datetime, date, timedelta

hoy = date.today()
print(f"Hoy es: {hoy}")

ahora = datetime.now()
print(f"Ahora son las: {ahora.strftime('%H:%M:%S')}")

mañana = hoy + timedelta(days=1)
print(f"Mañana será: {mañana}")

# os - interactuar con el sistema operativo
import os

print(f"Directorio actual: {os.getcwd()}")
print(f"Usuario: {os.getenv('USER', 'Desconocido')}")

# Listar archivos
archivos = os.listdir('.')
print(f"Archivos en directorio actual: {archivos}")
```

### Crear tu propio módulo

Crea un archivo `utilidades.py`:

```python
# utilidades.py

def es_par(numero):
    """Verifica si un número es par"""
    return numero % 2 == 0

def es_primo(numero):
    """Verifica si un número es primo"""
    if numero < 2:
        return False
    for i in range(2, int(numero ** 0.5) + 1):
        if numero % i == 0:
            return False
    return True

def formatear_dinero(cantidad):
    """Formatea un número como dinero"""
    return f"${cantidad:,.2f}"

# Variable del módulo
PI = 3.14159

if __name__ == "__main__":
    # Código que solo se ejecuta si corres este archivo directamente
    print("Probando utilidades...")
    print(f"5 es par: {es_par(5)}")
    print(f"7 es primo: {es_primo(7)}")
    print(f"Dinero: {formatear_dinero(1234.56)}")
```

Luego en otro archivo:

```python
# main.py
import utilidades

print(utilidades.es_par(8))           # True
print(utilidades.es_primo(17))        # True
print(utilidades.formatear_dinero(500))  # $500.00
print(utilidades.PI)                  # 3.14159

# O importar funciones específicas
from utilidades import es_par, formatear_dinero

print(es_par(10))                     # True
print(formatear_dinero(999.99))       # $999.99
```

---

## Ejercicios prácticos

### 1. Calculadora

```python
def calculadora():
    print("=== CALCULADORA ===")
    
    while True:
        print("\nOperaciones:")
        print("1. Sumar")
        print("2. Restar") 
        print("3. Multiplicar")
        print("4. Dividir")
        print("5. Salir")
        
        opcion = input("Elige operación: ")
        
        if opcion == "5":
            print("¡Adiós!")
            break
            
        if opcion not in ["1", "2", "3", "4"]:
            print("Opción inválida")
            continue
            
        try:
            a = float(input("Primer número: "))
            b = float(input("Segundo número: "))
            
            if opcion == "1":
                resultado = a + b
                print(f"{a} + {b} = {resultado}")
            elif opcion == "2":
                resultado = a - b
                print(f"{a} - {b} = {resultado}")
            elif opcion == "3":
                resultado = a * b
                print(f"{a} × {b} = {resultado}")
            elif opcion == "4":
                if b == 0:
                    print("No se puede dividir entre cero")
                else:
                    resultado = a / b
                    print(f"{a} ÷ {b} = {resultado}")
                    
        except ValueError:
            print("Ingresa números válidos")

# calculadora()
```

### 2. Juego de adivinanza

```python
import random

def juego_adivinanza():
    numero_secreto = random.randint(1, 100)
    intentos = 0
    max_intentos = 7
    
    print("🎯 ¡Adivina el número!")
    print(f"Estoy pensando en un número entre 1 y 100")
    print(f"Tienes {max_intentos} intentos")
    
    while intentos < max_intentos:
        try:
            adivinanza = int(input(f"\nIntento {intentos + 1}: "))
            intentos += 1
            
            if adivinanza == numero_secreto:
                print(f"🎉 ¡Correcto! Era {numero_secreto}")
                print(f"Lo adivinaste en {intentos} intento(s)")
                return
            elif adivinanza < numero_secreto:
                print("📈 Más alto")
            else:
                print("📉 Más bajo")
                
            print(f"Te quedan {max_intentos - intentos} intentos")
            
        except ValueError:
            print("Por favor, ingresa un número válido")
            intentos -= 1  # No contar intento inválido
    
    print(f"\n💀 ¡Perdiste! El número era {numero_secreto}")

# juego_adivinanza()
```

### 3. Analizador de texto

```python
def analizar_texto():
    texto = input("Ingresa un texto para analizar: ")
    
    # Estadísticas básicas
    caracteres = len(texto)
    caracteres_sin_espacios = len(texto.replace(" ", ""))
    palabras = len(texto.split())
    lineas = texto.count('\n') + 1
    
    # Contar vocales y consonantes
    vocales = "aeiouáéíóúAEIOUÁÉÍÓÚ"
    num_vocales = sum(1 for char in texto if char in vocales)
    num_consonantes = sum(1 for char in texto if char.isalpha() and char not in vocales)
    
    # Palabra más larga y más corta
    palabras_lista = texto.split()
    if palabras_lista:
        palabra_mas_larga = max(palabras_lista, key=len)
        palabra_mas_corta = min(palabras_lista, key=len)
    else:
        palabra_mas_larga = palabra_mas_corta = "N/A"
    
    # Frecuencia de palabras
    frecuencia = {}
    for palabra in palabras_lista:
        palabra_limpia = palabra.lower().strip('.,!?";')
        frecuencia[palabra_limpia] = frecuencia.get(palabra_limpia, 0) + 1
    
    # Mostrar resultados
    print(f"\n=== ANÁLISIS DEL TEXTO ===")
    print(f"Caracteres (con espacios): {caracteres}")
    print(f"Caracteres (sin espacios): {caracteres_sin_espacios}")
    print(f"Palabras: {palabras}")
    print(f"Líneas: {lineas}")
    print(f"Vocales: {num_vocales}")
    print(f"Consonantes: {num_consonantes}")
    print(f"Palabra más larga: '{palabra_mas_larga}' ({len(palabra_mas_larga)} caracteres)")
    print(f"Palabra más corta: '{palabra_mas_corta}' ({len(palabra_mas_corta)} caracteres)")
    
    if frecuencia:
        palabra_frecuente = max(frecuencia.items(), key=lambda x: x[1])
        print(f"Palabra más frecuente: '{palabra_frecuente[0]}' ({palabra_frecuente[1]} veces)")

# analizar_texto()
```

---

## Conceptos extras importantes

### List comprehensions (súper útil)

```python
# Forma tradicional
numeros = []
for i in range(10):
    if i % 2 == 0:
        numeros.append(i ** 2)

# Con list comprehension (más elegante)
numeros = [i ** 2 for i in range(10) if i % 2 == 0]
print(numeros)  # [0, 4, 16, 36, 64]

# Más ejemplos
frutas = ["manzana", "banana", "naranja"]
mayusculas = [fruta.upper() for fruta in frutas]
print(mayusculas)  # ['MANZANA', 'BANANA', 'NARANJA']

# Filtrar
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
pares = [n for n in numeros if n % 2 == 0]
print(pares)  # [2, 4, 6, 8, 10]

# Con strings
frase = "Hola mundo"
solo_vocales = [char for char in frase if char.lower() in 'aeiou']
print(solo_vocales)  # ['o', 'a', 'u', 'o']
```

### Enumerate y zip

```python
# enumerate - obtener índice y valor
frutas = ["manzana", "banana", "naranja"]
for i, fruta in enumerate(frutas):
    print(f"{i}: {fruta}")

# zip - combinar listas
nombres = ["Ana", "Luis", "María"]
edades = [25, 30, 28]
ciudades = ["Madrid", "Barcelona", "Valencia"]

for nombre, edad, ciudad in zip(nombres, edades, ciudades):
    print(f"{nombre}, {edad} años, vive en {ciudad}")

# Crear diccionario con zip
personas = dict(zip(nombres, edades))
print(personas)  # {'Ana': 25, 'Luis': 30, 'María': 28}
```

### Lambda (funciones pequeñas)

```python
# Función normal
def cuadrado(x):
    return x ** 2

# Lambda (función anónima)
cuadrado_lambda = lambda x: x ** 2

print(cuadrado(5))        # 25
print(cuadrado_lambda(5)) # 25

# Útil con map, filter, sorted
numeros = [1, 2, 3, 4, 5]

# map - aplicar función a todos los elementos
cuadrados = list(map(lambda x: x ** 2, numeros))
print(cuadrados)  # [1, 4, 9, 16, 25]

# filter - filtrar elementos
pares = list(filter(lambda x: x % 2 == 0, numeros))
print(pares)  # [2, 4]

# sorted - ordenar con criterio personalizado
palabras = ["python", "java", "c", "javascript"]
por_longitud = sorted(palabras, key=lambda palabra: len(palabra))
print(por_longitud)  # ['c', 'java', 'python', 'javascript']
```

---

## Consejos y buenas prácticas

### Nombres de variables

```python
# ❌ Malo
a = 25
x = "Juan"
lista1 = [1, 2, 3, 4, 5]

# ✅ Bueno
edad = 25
nombre = "Juan"
numeros_pares = [2, 4, 6, 8, 10]

# ❌ Muy largo
la_edad_del_usuario_que_ingreso_al_sistema = 25

# ✅ Descriptivo pero conciso
edad_usuario = 25
```

### Comentarios útiles

```python
# ❌ Comentario inútil
edad = 25  # Asignar 25 a la variable edad

# ✅ Comentario útil
edad = 25  # Edad mínima para acceder al sistema

# ✅ Explicar el "por qué", no el "qué"
tiempo_espera = 0.1  # Pausa necesaria para evitar spam en la API

def calcular_precio(precio_base, descuento=0):
    """
    Calcula el precio final aplicando descuento.
    
    Args:
        precio_base (float): Precio antes del descuento
        descuento (float): Descuento entre 0 y 1 (ej: 0.1 = 10%)
    
    Returns:
        float: Precio final con descuento aplicado
    """
    return precio_base * (1 - descuento)
```

### Organización del código

```python
# Al inicio del archivo
"""
Sistema de gestión de biblioteca
Autor: Tu nombre
Fecha: 2025-10-13
"""

# Imports al principio
import os
import sys
from datetime import datetime

# Constantes (en mayúsculas)
MAX_LIBROS = 100
ARCHIVO_DATOS = "biblioteca.txt"
VERSION = "1.0.0"

# Funciones
def cargar_libros():
    pass

def guardar_libros():
    pass

# Función principal
def main():
    print("Sistema iniciado")

# Solo ejecutar si es el archivo principal
if __name__ == "__main__":
    main()
```

### Debugging (encontrar errores)

```python
# Print para debugging (temporal)
def procesar_datos(datos):
    print(f"DEBUG: Datos recibidos: {datos}")  # Temporal
    
    resultado = []
    for item in datos:
        print(f"DEBUG: Procesando {item}")  # Temporal
        procesado = item * 2
        resultado.append(procesado)
    
    print(f"DEBUG: Resultado final: {resultado}")  # Temporal
    return resultado

# Usar assert para verificar suposiciones
def dividir(a, b):
    assert b != 0, "El divisor no puede ser cero"
    assert isinstance(a, (int, float)), "El dividendo debe ser un número"
    assert isinstance(b, (int, float)), "El divisor debe ser un número"
    
    return a / b

# dividir(10, 0)  # AssertionError: El divisor no puede ser cero
```

---

## Lo que aprendí

Después de todo esto, ya puedo:

- ✅ **Entender qué es Python** y por qué es tan popular
- ✅ **Trabajar con variables** de todos los tipos básicos
- ✅ **Manipular texto y números** como un pro
- ✅ **Usar listas y diccionarios** para organizar datos
- ✅ **Tomar decisiones** con if/else
- ✅ **Repetir tareas** con bucles for y while
- ✅ **Crear funciones** para reutilizar código
- ✅ **Manejar errores** sin que el programa explote
- ✅ **Leer y escribir archivos** para guardar información
- ✅ **Usar librerías** que otros ya crearon
- ✅ **Hacer programas completos** que realmente funcionan

**Nota personal:** Python se vuelve adictivo. Una vez que empiezas a automatizar cosas aburridas, no puedes parar. La clave es practicar mucho y no tener miedo a equivocarse.

**Lo que sigue:** Con esta base ya puedes aprender Python intermedio: clases, decoradores, generadores, y frameworks como Django o Flask para páginas web.

**Proyectos para practicar:**
- Sistema de inventario
- Analizador de logs
- Bot de Telegram
- Scraper de páginas web
- API REST básica

---
**Mis apuntes de:** Elvis  
**Fecha:** 13 de octubre de 2025  
**Nivel:** Principiante  
**Tiempo para dominarlo:** 2-3 meses practicando regularmente
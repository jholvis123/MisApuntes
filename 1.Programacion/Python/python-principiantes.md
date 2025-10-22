# 🐍 Python para Principiantes - Apuntes de Elvis

> **Para quien empieza desde CERO** - Solo lo básico que necesitas saber

---

## 🎯 ¿Qué vas a aprender?

En estas 2-3 semanas aprenderás:
- Variables básicas (guardar información)
- Operaciones simples (matemáticas y texto)
- Listas (organizar datos)
- Input básico (pedirle cosas al usuario)

**NO veremos:** Funciones, loops, if/else, diccionarios (eso es intermedio)

---

## 📦 Variables - Cajas para guardar cosas

### Los 4 tipos básicos

```python
# 1. NÚMEROS ENTEROS (int)
edad = 25
hermanos = 3

# 2. NÚMEROS DECIMALES (float)
altura = 1.75
precio = 19.99

# 3. TEXTO (string)
nombre = "Elvis"
ciudad = "Madrid"

# 4. VERDADERO/FALSO (boolean)
es_estudiante = True
tiene_carro = False
```

### Crear variables

```python
# Así se crean
mi_nombre = "Juan"
mi_edad = 28
mi_altura = 1.80

# Mostrar en pantalla
print(mi_nombre)  # Juan
print(mi_edad)    # 28
```

### Reglas para nombres

✅ **Bueno:**
```python
nombre = "Ana"
mi_edad = 25
precio_total = 100
```

❌ **Malo:**
```python
2edad = 25        # No empezar con número
mi edad = 25      # No espacios
nombre@ = "Ana"   # No símbolos raros
```

---

## 🔢 Matemáticas básicas

### Operaciones normales

```python
# Las básicas
suma = 10 + 5       # 15
resta = 10 - 5      # 5
multi = 10 * 5      # 50
division = 10 / 5   # 2.0

# Mostrar resultado
print(f"10 + 5 = {suma}")
```

### Operaciones especiales

```python
# División entera (sin decimales)
resultado = 10 // 3    # 3

# Resto (lo que sobra)
resto = 10 % 3         # 1

# Potencia
potencia = 2 ** 3      # 8

# ¿Es par o impar?
numero = 8
es_par = numero % 2 == 0
print(f"¿{numero} es par? {es_par}")  # True
```

### Convertir entre tipos

```python
# De texto a número
edad_texto = "25"
edad_numero = int(edad_texto)    # 25

# De número a texto
puntos = 1500
mensaje = f"Tienes {puntos} puntos"
```

---

## 📝 Trabajar con texto

### Operaciones básicas

```python
nombre = "Elvis"
apellido = "Coder"

# Unir textos
nombre_completo = nombre + " " + apellido
print(nombre_completo)  # Elvis Coder

# Repetir texto
linea = "-" * 10
print(linea)  # ----------

# ¿Cuántas letras?
print(len(nombre))  # 5
```

### Trucos útiles

```python
texto = "Hola Mundo"

# Mayúsculas/minúsculas
print(texto.upper())    # HOLA MUNDO
print(texto.lower())    # hola mundo

# Buscar
print("Mundo" in texto)     # True
print("Python" in texto)   # False

# Cambiar palabras
nuevo = texto.replace("Mundo", "Python")
print(nuevo)  # Hola Python
```

### F-strings (súper útil)

```python
nombre = "Ana"
edad = 25

# La forma fácil de mezclar texto y variables
mensaje = f"Hola, soy {nombre} y tengo {edad} años"
print(mensaje)

# Con operaciones
precio = 100
final = f"Precio con descuento: ${precio * 0.8:.2f}"
print(final)  # Precio con descuento: $80.00
```

---

## 📋 Listas - Organizar información

### Crear y usar listas

```python
# Crear listas
frutas = ["manzana", "banana", "naranja"]
numeros = [1, 2, 3, 4, 5]
mixta = ["Juan", 25, True]  # Puede mezclar tipos

# Lista vacía
compras = []

print(frutas)  # ['manzana', 'banana', 'naranja']
```

### Acceder a elementos

```python
colores = ["rojo", "verde", "azul", "amarillo"]

# Python cuenta desde 0
primero = colores[0]      # "rojo"
segundo = colores[1]      # "verde"

# Desde el final
ultimo = colores[-1]      # "amarillo"
penultimo = colores[-2]   # "azul"

print(f"Primer color: {primero}")
print(f"Último color: {ultimo}")

# ¿Cuántos elementos?
print(f"Tengo {len(colores)} colores")
```

### Modificar listas

```python
animales = ["gato", "perro", "pez"]

# Cambiar elemento
animales[1] = "loro"
print(animales)  # ['gato', 'loro', 'pez']

# Agregar al final
animales.append("conejo")
print(animales)  # ['gato', 'loro', 'pez', 'conejo']

# Quitar elemento
animales.remove("loro")
print(animales)  # ['gato', 'pez', 'conejo']

# Quitar el último
ultimo = animales.pop()
print(f"Quité: {ultimo}")  # conejo
```

### Operaciones útiles

```python
numeros = [3, 1, 4, 1, 5]

# Buscar
print(3 in numeros)          # True
print(10 in numeros)         # False

# Ordenar (nueva lista)
ordenados = sorted(numeros)
print(ordenados)  # [1, 1, 3, 4, 5]

# Contar elementos
print(numeros.count(1))  # 2 (aparece 2 veces)
```

---

## 💬 Interactuar con el usuario

### Pedir información

```python
# Pedir texto
nombre = input("¿Cómo te llamas? ")
print(f"Hola {nombre}")

# Pedir números
edad_texto = input("¿Cuántos años tienes? ")
edad = int(edad_texto)  # Convertir a número
print(f"Tienes {edad} años")

# Más directo
edad = int(input("Tu edad: "))
```

### Ejemplo práctico

```python
# Calculadora simple
print("=== CALCULADORA ===")

numero1 = float(input("Primer número: "))
numero2 = float(input("Segundo número: "))

suma = numero1 + numero2
resta = numero1 - numero2
multi = numero1 * numero2

print(f"\n{numero1} + {numero2} = {suma}")
print(f"{numero1} - {numero2} = {resta}")
print(f"{numero1} × {numero2} = {multi}")
```

---

## 🎯 Ejercicios para practicar

### 1. Información personal

```python
# Pide datos y muestra información
nombre = input("Tu nombre: ")
edad = int(input("Tu edad: "))
ciudad = input("Tu ciudad: ")

print(f"\nHola {nombre}!")
print(f"Tienes {edad} años")
print(f"Vives en {ciudad}")

# Calcular año de nacimiento
año_actual = 2025
año_nacimiento = año_actual - edad
print(f"Naciste en {año_nacimiento}")
```

### 2. Lista de compras

```python
# Crear lista de compras
compras = []

print("=== LISTA DE COMPRAS ===")
compras.append(input("Primer producto: "))
compras.append(input("Segundo producto: "))
compras.append(input("Tercer producto: "))

print(f"\nTu lista de compras:")
print(f"1. {compras[0]}")
print(f"2. {compras[1]}")
print(f"3. {compras[2]}")
print(f"\nTotal de productos: {len(compras)}")
```

### 3. Analizador de nombre

```python
# Analizar un nombre
nombre = input("Escribe tu nombre completo: ")

print(f"\n=== ANÁLISIS DE '{nombre}' ===")
print(f"Tiene {len(nombre)} caracteres")
print(f"En mayúsculas: {nombre.upper()}")
print(f"En minúsculas: {nombre.lower()}")
print(f"Primera letra: {nombre[0]}")
print(f"Última letra: {nombre[-1]}")

# Verificar si tiene espacios
if " " in nombre:
    print("Es un nombre completo (tiene espacios)")
else:
    print("Es solo un nombre (sin espacios)")
```

---

## 💡 Consejos importantes

### Errores comunes

❌ **Python cuenta desde 0**
```python
lista = ["a", "b", "c"]
print(lista[1])  # "b", no "a"!
```

❌ **Olvidar convertir tipos**
```python
edad = input("Tu edad: ")  # Es texto
# edad + 10  # ¡ERROR!
edad = int(edad)  # Convertir primero
```

❌ **Nombres de variables confusos**
```python
x = "Juan"     # ¿Qué es x?
nombre = "Juan"  # ✅ Mejor
```

### Mi rutina de práctica

**Día 1-7:** Variables y operaciones básicas
```python
# Ejercicio diario
nombre = input("Tu nombre: ")
edad = int(input("Tu edad: "))
print(f"Hola {nombre}, tienes {edad} años")
```

**Día 8-14:** Texto y f-strings
```python
# Ejercicio diario
producto = input("Producto: ")
precio = float(input("Precio: "))
print(f"El {producto} cuesta ${precio:.2f}")
```

**Día 15-21:** Listas básicas
```python
# Ejercicio diario
mis_cosas = []
mis_cosas.append(input("Cosa favorita 1: "))
mis_cosas.append(input("Cosa favorita 2: "))
print(f"Mis favoritas: {mis_cosas}")
```

---

## 🚀 ¿Qué sigue?

Cuando domines TODO lo de arriba, estarás listo para **Python INTERMEDIO**:

- Condicionales (if/elif/else)
- Bucles (for/while)
- Funciones propias
- Diccionarios
- Archivos

**Mi consejo:** NO pases a intermedio hasta que puedas hacer todo esto sin mirar los apuntes.

---

## 📚 Recursos útiles

**Para practicar:**
- Python.org (oficial)
- Codecademy (interactivo)
- w3schools.com (ejemplos)

**Cuando tengas dudas:**
- Google: "python [tu duda] ejemplo"
- Stack Overflow
- YouTube: busca "python principiantes"
- Foros de programación (no recomiendo usar ias para aprender lo básico)
---

**📝 Nota de Elvis:** "Estas 2-3 semanas son las MÁS importantes. Si dominas esto, el resto es pan comido. ¡Practica 15 minutos todos los días!"

*Creado: 16 de octubre de 2025*  
*Nivel: Principiante total*  
*Duración: 2-3 semanas de práctica*

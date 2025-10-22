# ⚡ Python Avanzado - Apuntes de Elvis

> **Para programadores que ya dominan lo básico** 

---

## 🎯 ¿Qué vas a entender?

En estas semanas comprenderás:
- **Clases:** Por qué crear tus propios tipos de datos
- **Módulos:** Cómo organizar código como los profesionales
- **Librerías:** Usar herramientas que otros ya crearon
- **Decoradores:** Hacer funciones más inteligentes
- **Generadores:** Manejar millones de datos sin romper la computadora
- **Datos:** Trabajar con JSON y CSV como un experto

**Requisito:** Ya dominas funciones, diccionarios, listas y manejo de errores.

---

## 🏗️ Clases y Objetos - Tu propia fábrica de datos

### ¿Qué problema resuelven las clases?

**Imagínate:** Tienes que manejar información de 1000 empleados. Con diccionarios sería un caos:

```python
empleado1 = {"nombre": "Ana", "salario": 3000}
empleado2 = {"nombre": "Carlos", "salario": 3500}
# ... y así 1000 veces
```

**Con clases es más ordenado:**

```python
class Empleado:
    def __init__(self, nombre, salario):
        self.nombre = nombre
        self.salario = salario
    
    def dar_aumento(self, porcentaje):
        self.salario *= (1 + porcentaje/100)

ana = Empleado("Ana", 3000)
ana.dar_aumento(10)  # Ana ahora gana 3300
```

### ¿Por qué usar clases?

✅ **Organización:** Todo relacionado con "Empleado" está en un lugar  
✅ **Reutilización:** Una vez creada, puedes hacer miles de empleados  
✅ **Mantenimiento:** Si cambias algo, se aplica a todos  
✅ **Claridad:** `ana.dar_aumento(10)` es más claro que funciones sueltas

### Herencia - Clases que "heredan" de otras

**La idea:** Si tienes una clase "Vehículo", puedes crear "Carro" y "Moto" que hereden las características básicas.

```python
class Vehiculo:
    def __init__(self, marca):
        self.marca = marca
    
    def encender(self):
        print(f"{self.marca} encendido")

class Carro(Vehiculo):  # Hereda de Vehiculo
    def tocar_claxon(self):
        print("¡BEEP!")

mi_carro = Carro("Toyota")
mi_carro.encender()      # Heredó esto de Vehiculo
mi_carro.tocar_claxon()  # Esto es propio del Carro
```

**¿Por qué es útil?** No repites código. Si todos los vehículos se encienden igual, lo escribes una vez en la clase padre.

---

## 📦 Módulos - Organizar tu código como un profesional

### ¿Qué son los módulos?

**Simple:** Un módulo es un archivo `.py` con funciones que puedes usar en otros archivos.

**¿Por qué usarlos?**
- Tu código no se vuelve un archivo gigante imposible de leer
- Puedes reutilizar funciones en varios proyectos
- Es más fácil trabajar en equipo

### Crear tu primer módulo

**Archivo `utilidades.py`:**
```python
def formatear_dinero(cantidad):
    return f"${cantidad:,.2f}"

def es_par(numero):
    return numero % 2 == 0
```

**Archivo `main.py`:**
```python
import utilidades

precio = 1234.56
print(utilidades.formatear_dinero(precio))  # $1,234.56
```

**Dato curioso:** Todo archivo `.py` es un módulo. Python viene con cientos ya incluidos.

### Librerías que cambiarán tu vida

**`random` - Para cosas aleatorias:**
```python
import random
numero = random.randint(1, 10)  # Número entre 1 y 10
```

**`datetime` - Para fechas:**
```python
from datetime import datetime
ahora = datetime.now()
print(ahora.strftime("%d/%m/%Y"))  # 22/10/2025
```

**`os` - Para interactuar con archivos:**
```python
import os
print(os.listdir("."))  # Lista archivos del directorio actual
```

---

## 📊 JSON y CSV - Intercambiar datos con el mundo

### JSON - El lenguaje universal de datos

**¿Qué es?** Un formato de texto que entienden todos los lenguajes de programación.

**¿Cuándo lo usas?** APIs, configuraciones, intercambiar datos entre programas.

```python
import json

# Python a JSON
persona = {"nombre": "Ana", "edad": 28}
texto_json = json.dumps(persona)  # '{"nombre": "Ana", "edad": 28}'

# JSON a Python
datos = json.loads(texto_json)
print(datos["nombre"])  # Ana
```

**Truco profesional:** Usa `indent=2` para que se vea bonito cuando lo guardas en archivos.

### CSV - Como Excel pero más simple

**¿Qué es?** Archivos donde cada línea es una fila y las columnas están separadas por comas.

**¿Cuándo lo usas?** Datos de Excel, bases de datos, informes.

```python
import csv

# Escribir CSV
with open("datos.csv", "w", newline="") as archivo:
    escritor = csv.writer(archivo)
    escritor.writerow(["Nombre", "Edad"])  # Encabezado
    escritor.writerow(["Ana", 28])         # Datos
```

**Pro tip:** Excel puede abrir archivos CSV directamente.

---

## 🎨 Decoradores - Superpoderes para funciones

### ¿Qué son?

**Analogía:** Un decorador es como ponerle una capa extra a una función sin cambiar lo que hace por dentro.

**¿Para qué sirven?**
- Medir cuánto tiempo tarda una función
- Validar datos antes de que la función los use
- Registrar qué funciones se ejecutan (logs)
- Requerir permisos antes de ejecutar algo

### Ejemplo súper simple

```python
def medir_tiempo(funcion):
    def nueva_funcion():
        print("Iniciando...")
        resultado = funcion()
        print("Terminó!")
        return resultado
    return nueva_funcion

@medir_tiempo
def saludar():
    return "Hola mundo"

saludar()  # Imprime: Iniciando... Terminó!
```

**Lo genial:** `saludar()` sigue siendo la misma función, pero ahora tiene superpoderes.

---

## 🔄 Generadores - Manejar millones sin romper tu computadora

### ¿Cuál es el problema?

Si quieres procesar 1 millón de números, crear una lista de 1 millón de elementos consume mucha memoria.

### ¿Cuál es la solución?

Los generadores crean los números "uno por uno" cuando los necesitas, no todos a la vez.

```python
# Malo: Crea toda la lista en memoria
def numeros_lista(maximo):
    return [x for x in range(maximo)]

# Bueno: Crea números bajo demanda
def numeros_generador(maximo):
    for x in range(maximo):
        yield x  # "yield" = entregar uno por uno

# Usar el generador
gen = numeros_generador(1000000)
for numero in gen:
    if numero > 10:
        break  # Solo procesé 11 números, no 1 millón
```

**¿Cuándo usarlos?** Archivos enormes, procesar mucha data, cuando no sabes cuántos elementos necesitarás.

---

## 🌐 Librerías que debes conocer

### `requests` - Para hablar con internet

```python
import requests
respuesta = requests.get("https://api.github.com/users/octocat")
datos = respuesta.json()
print(datos["name"])
```

**¿Para qué?** APIs, descargar archivos, automatizar interacciones web.

### `collections` - Estructuras de datos especiales

```python
from collections import Counter
palabras = ["python", "es", "genial", "python", "es", "fácil"]
contador = Counter(palabras)
print(contador)  # Counter({'python': 2, 'es': 2, 'genial': 1, 'fácil': 1})
```

**¿Para qué?** Contar elementos automáticamente, diccionarios con valores por defecto.

---

## 💡 Consejos para código avanzado

### 1. Type Hints - Documenta qué tipos esperas

```python
def sumar(a: int, b: int) -> int:
    return a + b
```

**¿Por qué?** Tu editor te ayuda mejor, otros entienden tu código más fácil.

### 2. Docstrings - Explica qué hace tu función

```python
def calcular_propina(total: float, porcentaje: float = 15) -> float:
    """
    Calcula la propina basada en el total de la cuenta.
    
    Args:
        total: Total de la cuenta
        porcentaje: Porcentaje de propina (por defecto 15%)
    
    Returns:
        Cantidad de propina a dejar
    """
    return total * (porcentaje / 100)
```

### 3. Logging - Rastrea qué hace tu programa

```python
import logging
logging.info("El programa inició correctamente")
logging.error("Algo salió mal aquí")
```

**¿Por qué?** En vez de `print()` por todos lados, usas logs que puedes activar/desactivar.

---

## 🎯 Proyecto práctico: Sistema de tareas con clases

**La idea:** Un gestor de tareas personal con fechas de vencimiento.

### Lo que aprenderás haciendo esto:

✅ **Clases:** `Tarea`, `GestorTareas`  
✅ **Módulos:** Separar lógica en archivos  
✅ **JSON:** Guardar/cargar tareas  
✅ **Datetime:** Manejar fechas de vencimiento  
✅ **Type hints:** Documentar tu código  

### Estructura básica:

```python
from datetime import datetime

class Tarea:
    def __init__(self, titulo: str, descripcion: str = ""):
        self.titulo = titulo
        self.descripcion = descripcion
        self.completada = False
        self.fecha_creacion = datetime.now()
    
    def marcar_completada(self):
        self.completada = True
    
    def __str__(self):
        estado = "✅" if self.completada else "⏳"
        return f"{estado} {self.titulo}"

# Usar la clase
tarea = Tarea("Aprender Python avanzado")
print(tarea)  # ⏳ Aprender Python avanzado
tarea.marcar_completada()
print(tarea)  # ✅ Aprender Python avanzado
```

**El poder:** Una vez que tienes la clase `Tarea`, puedes crear miles de tareas fácilmente.

---

## 🚀 ¿Qué sigue después?

Con Python avanzado ya puedes construir cosas reales:

**🌐 Desarrollo Web:**
- Flask: Crear sitios web simples
- Django: Sitios web complejos
- APIs REST para móviles

**📊 Ciencia de datos:**
- Pandas: Analizar datos como Excel pero más poderoso
- Matplotlib: Crear gráficos profesionales
- Machine Learning: Hacer predicciones

**🤖 Automatización:**
- Selenium: Controlar navegadores automáticamente
- Scripts: Automatizar tareas aburridas
- Bots: Para Telegram, Discord, etc.

**🎮 Proyectos divertidos:**
- Juegos con pygame
- Aplicaciones de escritorio
- Scrapers web (extraer datos de sitios)

---

## 💬 Reflexión final de Elvis

**Lo que me costó entender al inicio:** Python avanzado no es escribir código más complicado, es escribir código más **organizado** y **reutilizable**.

**Mi error más grande:** Tratar de usar todas las características avanzadas a la vez. **Mejor:** Domina una cosa, úsala en un proyecto real, luego aprende la siguiente.

**Lo que cambió mi forma de programar:**
1. **Clases:** Dejé de tener variables sueltas por todos lados
2. **Módulos:** Mi código se volvió más limpio y reutilizable  
3. **Type hints:** Menos bugs, más claridad
4. **Logging:** Puedo ver qué hace mi programa sin imprimir mil cosas

**Consejo final:** No memorices sintaxis, entiende **cuándo** y **por qué** usar cada herramienta.

---

**📝 Nota de Elvis:** "Python avanzado es como tener una caja de herramientas profesional. No necesitas todas las herramientas para cada trabajo, pero cuando las necesitas, ¡hacen toda la diferencia!"

*Creado: 22 de octubre de 2025*  
*Nivel: Avanzado*  
*Enfoque: Conceptos > Código*  
*Requisito: Dominar Python intermedio*

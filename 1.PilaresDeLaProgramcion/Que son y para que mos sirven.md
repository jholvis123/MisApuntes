# 📚 Apuntes: Los Pilares de la Programación

*Estos son mis apuntes mientras aprendo programación. Los escribo como si me estuviera explicando a mí mismo.*

---

## 🤔 ¿Qué diablos son los "pilares" de la programación?

Bueno, cuando empecé a programar pensaba que solo era escribir código que funcione. Pero resulta que no... hay formas "correctas" de hacerlo para que no se vuelva un desastre después.

Los **pilares** son como las reglas básicas que siguen los programadores profesionales para no volverse locos. Es como cuando aprendes a manejar: no solo es acelerar y frenar, también está seguir las señales, mantener distancia, etc.

**Los pilares que he identificado (hasta ahora):**
1. **Abstracción** - Esconder lo complicado
2. **Modularidad** - Dividir en pedacitos
3. **Reutilización** - No reinventar la rueda
4. **Legibilidad** - Que se entienda fácil
5. **Manejo de errores** - Que no se rompa todo
6. **Testing** - Probar que funciona
7. **Documentación** - Explicar qué hace

---

## 1. 🎭 Abstracción (o "esconder la magia")

### Mi entendimiento:
Es como usar un microondas. Yo solo aprieto botones y sale comida caliente. No necesito saber cómo funcionan las microondas por dentro. El fabricante "abstrajo" toda esa complejidad.

### En programación:
Crear funciones que hagan algo útil sin que tengas que pensar en todos los detalles internos cada vez.

### Ejemplo de mi vida:
**Antes (todo mezclado):**
```python
# Calcular el promedio de mis calificaciones
calificaciones = [85, 92, 78, 88, 95]
suma_total = 85 + 92 + 78 + 88 + 95
promedio = suma_total / 5
print(f"Mi promedio es: {promedio}")

# Más tarde necesito el promedio de otros datos...
# ¡Tengo que escribir todo de nuevo! 😩
```

**Después (abstraído):**
```python
def calcular_promedio(numeros):
    """Calcula el promedio de una lista de números"""
    return sum(numeros) / len(numeros)

# Ahora lo uso para cualquier cosa
mis_calificaciones = [85, 92, 78, 88, 95]
print(f"Promedio escuela: {calcular_promedio(mis_calificaciones)}")

gastos_semana = [50, 30, 80, 25]
print(f"Gasto promedio: {calcular_promedio(gastos_semana)}")
```

**¿Por qué es mejor?**
- Solo escribes la lógica una vez
- Si hay un error, lo arreglas en un lugar
- Es más claro qué estás haciendo

### Mi regla personal:
*Si estoy copiando y pegando código, probablemente necesito una función.*

---

## 2. 🧩 Modularidad (divide y vencerás)

### Mi entendimiento:
Como armar un LEGO. No haces toda la nave espacial de una pieza gigante. Haces las alas, el cuerpo, la cabina por separado, y luego los unes.

### En programación:
Dividir tu programa en partes pequeñas donde cada una hace una cosa específica.

### Ejemplo de mi experiencia:
Estaba haciendo un programa para organizar mi música, y al principio tenía todo en un archivo gigante:

**Mi primer intento (todo junto):**
- Descargar información de canciones ✓
- Organizar por género ✓  
- Crear playlists ✓
- Actualizar metadatos ✓
- **Resultado:** 500 líneas en un archivo, imposible de mantener

**Mi segundo intento (modular):**
- `descargador.py` - Solo descargar info
- `organizador.py` - Solo organizar archivos  
- `playlist.py` - Solo crear listas
- `metadatos.py` - Solo actualizar tags
- `main.py` - Coordinar todo

**Ventajas que descubrí:**
- Si rompo algo en playlists, el resto sigue funcionando
- Puedo probar cada parte por separado
- Otro amigo puede ayudarme con solo una parte
- Puedo reutilizar `descargador.py` en otros proyectos

### Mi regla personal:
*Si una función hace más de una cosa bien definida, probablemente necesito dividirla.*

---

## 3. ♻️ Reutilización (no reinventes la rueda)

### Mi entendimiento:
¿Para qué hacer tu propia calculadora si ya existe una? ¿Para qué escribir código para leer archivos JSON si Python ya lo tiene?

### En programación:
Usar código que ya existe (librerías) y escribir tu código de manera que puedas usarlo en otros proyectos.

### Ejemplos de mi aprendizaje:

**Cosas que ya no programo desde cero:**
- Leer/escribir archivos JSON → uso `import json`
- Hacer peticiones web → uso `import requests`  
- Fechas y horas → uso `import datetime`
- Matemáticas complejas → uso `import math`

**Código mío que reutilizo:**
```python
# Esta función la uso en varios proyectos
def limpiar_texto(texto):
    """Quita espacios extra y convierte a minúsculas"""
    return texto.strip().lower()

# La uso para limpiar nombres de archivos, inputs del usuario, etc.
```

### Mi regla personal:
*Antes de escribir algo, googleo si ya existe. Antes de escribir lo mismo dos veces, hago una función.*

---

## 4. 📖 Legibilidad (código que se entiende)

### Mi entendimiento:
Es escribir código como si fueras a explicárselo a tu abuela. Claro, directo, sin palabras raras innecesarias.

### Lo que he aprendido por las malas:

**Código que escribí hace 6 meses (no entiendo ni yo):**
```python
def f(x, y):
    return x * 0.16 + y * 1.2 if x > 1000 else x * 0.12 + y
```

**El mismo código pero legible:**
```python
def calcular_precio_con_impuestos(precio_base, envio):
    """
    Calcula el precio final con impuestos según el monto.
    Si es más de $1000, usa impuesto premium (16%)
    """
    IMPUESTO_NORMAL = 0.12
    IMPUESTO_PREMIUM = 0.16
    LIMITE_PREMIUM = 1000
    
    if precio_base > LIMITE_PREMIUM:
        return precio_base * IMPUESTO_PREMIUM + envio * 1.2
    else:
        return precio_base * IMPUESTO_NORMAL + envio
```

### Reglas que me funcionan:
- **Nombres descriptivos:** `edad_usuario` en vez de `x`
- **Funciones cortas:** máximo 20-30 líneas  
- **Comentarios cuando no es obvio:** especialmente con números mágicos
- **Espacios en blanco:** para separar bloques lógicos

### Mi regla personal:
*Si tengo que leer mi código 3 veces para entenderlo, está mal escrito.*

---

## 5. ⚠️ Manejo de errores (que no se rompa todo)

### Mi entendimiento:
Es como poner cinturón de seguridad. Esperas no necesitarlo, pero si pasa algo, te salva.

### Lo que aprendí cuando mi programa se rompía:

**Mi primer código (ingenuo):**
```python
def leer_mi_archivo():
    archivo = open("datos.txt")
    contenido = archivo.read()
    return contenido
```
**Resultado:** Si el archivo no existe → BOOM, programa muerto 💀

**Ahora (con manejo de errores):**
```python
def leer_mi_archivo():
    try:
        with open("datos.txt", "r") as archivo:
            return archivo.read()
    except FileNotFoundError:
        print("⚠️ El archivo no existe, creando uno nuevo...")
        return ""
    except Exception as error:
        print(f"❌ Error inesperado: {error}")
        return None
```

### Tipos de errores que he encontrado:
- **Archivos que no existen**
- **Internet que se cae**  
- **Usuarios que escriben cosas raras**
- **Quedarse sin memoria/espacio**
- **Permisos insuficientes**

### Mi regla personal:
*Siempre pregunto: "¿Qué puede salir mal aquí?" y preparo un plan B.*

---

## 6. 🧪 Testing (probar que funciona)

### Mi entendimiento:
Es como revisar tu examen antes de entregarlo. Verificas que tus respuestas sean correctas.

### Por qué empecé a hacer tests:
Porque me cansé de que mi código "funcionara en mi computadora" pero se rompiera en otros lados.

### Ejemplo simple:
```python
def multiplicar(a, b):
    return a * b

# Mi test básico
def probar_multiplicar():
    # Caso normal
    assert multiplicar(3, 4) == 12
    
    # Casos especiales  
    assert multiplicar(0, 5) == 0
    assert multiplicar(-2, 3) == -6
    
    print("✅ Todos los tests pasaron")

probar_multiplicar()
```

### Tipos de tests que hago:
- **¿Funciona con datos normales?**
- **¿Funciona con datos raros?** (negativos, cero, texto vacío)
- **¿Se rompe elegantemente?** (no crash horrible)

### Mi regla personal:
*Si es importante, lo pruebo. Si lo cambio, vuelvo a probarlo.*

---

## 7. 📝 Documentación (explicar qué hace)

### Mi entendimiento:
Es como dejarle una nota a tu yo del futuro explicando qué estabas pensando.

### Lo que documento:
- **¿Qué hace este código?**
- **¿Por qué lo hice así?**  
- **¿Cómo se usa?**
- **¿Qué necesita para funcionar?**

### Mi formato típico:
```python
def procesar_ventas(archivo_csv, descuento=0):
    """
    Procesa un archivo de ventas y aplica descuentos.
    
    Args:
        archivo_csv: Ruta al archivo con datos de ventas
        descuento: Porcentaje de descuento (0-100)
    
    Returns:
        Diccionario con totales procesados
        
    Raises:
        ValueError: Si el descuento está fuera de rango
        FileNotFoundError: Si el archivo no existe
    """
```

### Mi regla personal:
*Si me tomó más de 10 minutos entenderlo/escribirlo, merece documentación.*

---

## 🎯 Mi plan de mejora personal

### Semana 1-2: Practicar abstracción
- [ ] Convertir 3 scripts míos en funciones reutilizables
- [ ] Identificar código duplicado y crear funciones comunes

### Semana 3-4: Mejorar modularidad  
- [ ] Dividir mi proyecto más grande en módulos
- [ ] Practicar separar lógica de presentación

### Semana 5-6: Añadir robustez
- [ ] Agregar manejo de errores a mis funciones críticas
- [ ] Escribir tests básicos para mis funciones principales

### Semana 7+: Pulir
- [ ] Mejorar nombres de variables y funciones
- [ ] Documentar mejor mis proyectos importantes
- [ ] Crear READMEs para mis repositorios

---

## 💭 Reflexiones personales

### Lo que me ha funcionado:
- **Empezar simple:** No trato de aplicar todos los pilares a la vez
- **Refactorizar gradualmente:** Mejoro código existente poco a poco
- **Leer código de otros:** GitHub es una mina de oro de ejemplos
- **No ser perfeccionista:** Mejor código funcionando que código perfecto que no existe

### Errores que he cometido:
- **Sobre-ingenierizar:** Hacer todo súper complejo desde el inicio
- **No documentar:** "Ya me acordaré para qué era esto" (spoiler: no me acuerdo)
- **Evitar los tests:** "Es código simple, ¿qué puede salir mal?" (todo puede salir mal)

### Mi filosofía actual:
*"Haz que funcione, hazlo claro, hazlo rápido (en ese orden)"*

---

## 📚 Recursos que me han ayudado

- **"Clean Code" de Robert Martin** - Para legibilidad
- **Stack Overflow** - Para aprender de errores de otros  
- **GitHub** - Para ver código real de proyectos exitosos
- **Documentación oficial de Python** - Ejemplos bien escritos
- **Code reviews con amigos** - Feedback honesto sobre mi código

---

**Nota final de Elvis:**
*Estos pilares no son reglas sagradas. Son guías que me ayudan a escribir mejor código. Los voy aplicando gradualmente, según voy aprendiendo. Lo importante es empezar y ir mejorando.*

*¡Ahora a seguir codificando! 🚀*

# Manejo de Archivos en Python

Esta guía proporciona una introducción a las operaciones básicas de manejo de archivos en Python, incluyendo archivos de texto, CSV y JSON, así como el manejo de excepciones y la organización de proyectos.

## Archivos de Texto

Los archivos de texto guardan datos en formato legible por humanos. Sus extensiones más comunes son las siguientes:

* **.txt:** Uno de los más comunes, ideales para guardar información básica como la que se guarda en un bloc de notas personal.
* **.log:** Usados como almacenadores de registros o logs de eventos, como un historial de actividades.
* **.csv:** Son archivos de texto diseñados para organizar tablas, usando comas como separador. Se usan principalmente para almacenar información estructurada.
* **.md:** Usados en la documentación, como los que se encuentran en proyectos en GitHub.
* **.json:** Utilizados para almacenar datos estructurados en formato texto, pero con una estructura de clave-valor.

## Apertura y Cierre de Archivos

Para abrir un archivo en Python, se utiliza la función incorporada `open()`. Esta función puede ser utilizada por la aplicación de manera automática o por el usuario ingresando la ruta del archivo, y ademas 
```python
archivo = open("Archivo.txt")

archivo.close()
```
tambien usamos .close() para cerrar el archivo y liberar recursos ademas de  asegurar que se guarden los cambios. Para hacerlo, usamos el método 

# Lectura de Archivos
Una vez que un archivo está abierto, se puede leer su contenido utilizando los siguientes métodos:

* **read():** Lee todo el contenido del archivo y lo devuelve como una cadena (string).

```Python

with open("Archivo.txt") as archivo:
    contenido = archivo.read()
    print(contenido)
```
* **readline():** Lee una línea del archivo a la vez y devuelve esa línea como una cadena (string).

```Python

with open("Archivo.txt") as archivo:
    linea = archivo.readline()
    while linea:
        print(linea.strip())  # Elimina espacios o saltos de línea
        linea = archivo.readline()
```
* **readlines():** Lee todas las líneas del archivo y las devuelve en forma de una lista, donde cada línea es un elemento de la lista.

```Python

with open("Archivo.txt") as archivo:
    lineas = archivo.readlines()
    for linea in lineas:
        print(linea.strip())

```

# Escritura de Archivos

Al igual que en la lectura de archivos, Python nos ofrece métodos (también llamados modos) para escribir en un archivo. Estos son:

* **Modo "w" (escritura):** Abre el archivo para escribir y sobrescribe el contenido existente. Si el archivo no existe, lo crea.

```Python

with open("Archivo.txt", "w") as archivo:
    archivo.write("Esto reemplaza todo el contenido del archivo.\n")
    archivo.write("Línea nueva agregada.\n")
```
* **Modo "a" (adición):** Abre el archivo para agregar contenido al final. Si el archivo no existe, lo crea.

```Python

with open("Archivo.txt", "a") as archivo:
    archivo.write("Esta línea se agrega al final del archivo.\n")
```
* **Modo "r+" (lectura y escritura):** Abre el archivo para lectura y escritura. Permite modificar partes específicas del contenido existente. El archivo debe existir previamente.

```Python

with open("Archivo.txt", "r+") as archivo:
    contenido = archivo.read()
    archivo.seek(0)          # Vuelve al inicio del archivo
    archivo.write("Inicio modificado del archivo.\n")
    archivo.write(contenido)  # Escribe el contenido original después
```

# Uso de bloques `try` y `except` para manejo de excepciones

Los bloques try y except se usan en el manejo de excepciones, que son errores que pueden ocurrir durante la ejecución de un programa. Estos bloques ayudan a evitar que el programa se interrumpa de forma abrupta y permiten tomar acciones apropiadas cuando sucede un error.

Usamos try como contenedor del código que podría generar la excepción y except define el manejo de la excepción si llegara a ocurrir. Puede capturar errores específicos o generales.


```Python

try:
    archivo = open("archivo_inexistente.txt", "r")
except Exception as e:  # 'e' almacena detalles del error. En este caso, usamos except para capturar errores generales.
    print(f"Ocurrió un error: {e}")
```

# Comprobación de la existencia de archivos

En Python se puede comprobar la existencia de archivos usando dos módulos: el módulo `os` o el módulo más moderno `pathlib`.

* **Módulo os:** El módulo os proporciona la función os.path.exists() para verificar si un archivo o directorio existe.

```Python

import os


if os.path.exists("Archivo.txt"):
    print("El archivo existe.")
else:
    print("El archivo no existe.")
```

* **Módulo pathlib:** El módulo pathlib es más recomendado, ya que ofrece una forma más orientada a objetos para manejar rutas y archivos. Puedes usar el método .exists() de un objeto Path.

```Python

from pathlib import Path

archivo = Path("Archivo.txt")


if archivo.exists():
    print("El archivo existe.")
else:
    print("El archivo no existe.")
```
# Lectura y escritura usando la biblioteca csv

La biblioteca csv en Python es muy útil para trabajar con archivos CSV (Comma-Separated Values), que son archivos de texto que almacenan datos en formato tabular. 

* **Lectura CSV:** Para leer un archivo CSV, podemos usar el método `csv.reader()`. Este método convierte cada línea en una lista de valores separados por comas.

```Python

import csv


with open("datos.csv", "r") as archivo:
    lector = csv.reader(archivo)
    for fila in lector:
        print(fila)  # Cada fila es una lista
```
* **Escritura CSV:** Para escribir en un archivo CSV, usamos el método `csv.writer()`, que permite escribir filas en el archivo. El uso de `newline=""` es para evitar líneas en blanco adicionales en algunos sistemas operativos.

```Python

import csv


with open("datos.csv", "w", newline="") as archivo:
    escritor = csv.writer(archivo)
    escritor.writerow(["Nombre", "Edad", "Ciudad"])  # Escribir una fila de encabezado
    escritor.writerow(["Rafael", 22, "San Fernando"])
    escritor.writerow(["Luis", 30, "Caracas"])
```

# Ejemplo de manejo de archivos usando CSV:

En este ejemplo, usamos como base un presupuesto categorizado de manera simplificada en alimentación, transporte y entretenimiento.

```Python

import csv

# Paso 1: Crear el archivo CSV con datos iniciales
with open("presupuesto.csv", "w", newline="") as archivo_csv:
    escritor = csv.writer(archivo_csv)
    escritor.writerow(["Categoría", "Gasto"])  # Encabezados
    escritor.writerow(["Alimentación", 50])
    escritor.writerow(["Transporte", 20])
    escritor.writerow(["Entretenimiento", 30])
    escritor.writerow(["Alimentación", 25])
    escritor.writerow(["Transporte", 15])
print("Archivo 'presupuesto.csv' creado con datos iniciales.")

# Paso 2: Leer el archivo CSV y calcular el presupuesto
gastos_por_categoria = {}
gasto_total = 0

with open("presupuesto.csv", "r") as archivo_csv:
    lector = csv.reader(archivo_csv)
    next(lector)  # Saltar los encabezados
    
    for fila in lector:
        categoria, gasto = fila[0], float(fila[1])  # Convertir el gasto a número
        gasto_total += gasto  # Sumar al total de gastos
        
        # Agrupar gastos por categoría
        if categoria in gastos_por_categoria:
            gastos_por_categoria[categoria] += gasto
        else:
            gastos_por_categoria[categoria] = gasto

# Paso 3: Mostrar los resultados
print(f"\nGasto total: ${gasto_total:.2f}")
print("Gastos por categoría:")
for categoria, gasto in gastos_por_categoria.items():
    print(f"  {categoria}: ${gasto:.2f}")
```

# Introducción a la biblioteca json para serialización/deserialización

La biblioteca json es una útil herramienta a la hora de trabajar con datos en formato .json (JavaScript Object Notation), dado que es un formato ligero y legible que se utiliza comúnmente para intercambiar datos entre aplicaciones, especialmente en APIs y servicios web.

* **La Serialización:** Es el proceso de convertir datos de Python (como diccionarios o listas) en una cadena JSON. Esto es útil para guardar datos en archivos o enviarlos a través de una red.

* **La Deserialización:** Es el proceso inverso, donde una cadena JSON se convierte nuevamente en objetos de Python.

Esta biblioteca proporciona varias funciones para serializar y deserializar datos:

`json.dump()` y `json.dumps()` (Serialización):

`dump()`: Escribe datos serializados directamente en un archivo.

`dumps()`: Devuelve los datos serializados como una cadena.

`json.load()` y `json.loads()` (Deserialización):

`load()`: Lee datos JSON desde un archivo y los convierte en objetos de Python.

`loads()`: Convierte una cadena JSON en objetos de Python.

```Python

import json

# Datos en formato de Python
datos = {
    "nombre": "Juan",
    "edad": 30,
    "ciudad": "Caracas",
    "hobbies": ["leer", "programar", "viajar"]
}

# Guardar los datos en un archivo JSON
with open("datos.json", "w") as archivo:
    json.dump(datos, archivo)  # Serialización
print("Datos guardados en 'datos.json'")

# Leer los datos desde el archivo JSON
with open("datos.json", "r") as archivo:
    datos_cargados = json.load(archivo)  # Deserialización
print("Datos cargados:", datos_cargados)
```
# Ejemplo de carga y guardado de datos en formato .json.

En este ejemplo usamos una lista de respuesto de autos

```Python

import json

# Datos iniciales sobre repuestos de autos
repuestos = [
    {"repuesto": "Aceite de motor", "cantidad": 2, "precio_unitario": 20.50},
    {"repuesto": "Filtro de aire", "cantidad": 1, "precio_unitario": 15.75},
    {"repuesto": "Batería", "cantidad": 1, "precio_unitario": 85.90},
    {"repuesto": "Pastillas de freno", "cantidad": 4, "precio_unitario": 30.25},
]

# Guardar los datos en un archivo JSON
with open("repuestos.json", "w") as archivo_json:
    json.dump(repuestos, archivo_json, indent=4)  # Indentación para una mejor legibilidad
print("Datos guardados en 'repuestos.json'")

# Leer los datos desde el archivo JSON
with open("repuestos.json", "r") as archivo_json:
    repuestos_cargados = json.load(archivo_json)

# Mostrar los datos cargados
print("Datos cargados:")
for repuesto in repuestos_cargados:
    print(f"Repuesto: {repuesto['repuesto']}, Cantidad: {repuesto['cantidad']}, Precio Unitario: ${repuesto['precio_unitario']:.2f}")
```

# Cierre automático de archivos usando `with`

El cierre automático de archivos se usa mediante el bloque with, que hemos estado utilizando en los ejemplos anteriores en los que trabajamos con archivos. Esta es una manera eficiente de manejar recursos como archivos. Cuando usamos with, Python se asegura de cerrar el archivo automáticamente al salir del bloque, incluso si ocurre un error durante la ejecución. Esto ayuda a evitar problemas como archivos que quedan abiertos accidentalmente o pérdida de datos.

```Python
with open("ejemplo.txt", "r") as archivo:
    contenido = archivo.read()  # Leer todo el contenido del archivo
    print(contenido)
# Aquí el archivo ya está cerrado automáticamente
```
# Organización y estructura de archivos en proyectos

La organización y estructura de archivos en proyectos es fundamental para mantener el código limpio, fácil de entender y escalable. Aquí tienes una guía sobre cómo estructurar archivos en proyectos de software:

* **1. Directorio raíz del proyecto**

El directorio raíz debe contener los archivos y carpetas principales del proyecto. Por ejemplo:
```
mi_proyecto/
├── src/
├── tests/
├── docs/
├── config/
├── data/
├── README.md
├── requirements.txt
└── .gitignore
```
* **2. Carpetas principales**

**src/:** Contiene el código fuente del proyecto. Aquí se organizan los módulos y paquetes.

**tests/:** Contiene los archivos de prueba para garantizar la calidad del código.

**docs/:** Documentación del proyecto, como guías de uso, diagramas y especificaciones.

**config/:** Archivos de configuración, como settings.json o config.yaml.

**data/:** Archivos de datos, como bases de datos, CSV o JSON.

* **3. Archivos importantes**

**README.md:** Explica el propósito del proyecto, cómo instalarlo y usarlo.

**requirements.txt:** Lista de dependencias necesarias para ejecutar el proyecto.

**.gitignore:** Define los archivos y carpetas que deben ignorarse en el control de versiones.


* **4. Organización de módulos**

Si el proyecto es grande, divide el código en módulos o paquetes. Por ejemplo:
```
src/
├── __init__.py
├── utils.py
├── models/
│   ├── __init__.py
│   ├── user.py
│   └── product.py
├── controllers/
│   ├── __init__.py
│   ├── user_controller.py
│   └── product_controller.py
```
Ejemplo práctico:
```
inventario/
├── src/
│   ├── main.py
│   ├── utils.py
│   ├── models/
│   │   ├── producto.py
│   │   └── cliente.py
│   └── controllers/
│       ├── producto_controller.py
│       └── cliente_controller.py
├── tests/
│   ├── test_producto.py
│   └── test_cliente.py
├── docs/
│   ├── manual_usuario.md
│   └── diagrama_clases.png
├── config/
│   └── settings.json
├── data/
│   ├── inventario.csv
│   └── clientes.json
├── README.md
├── requirements.txt
└── .gitignore
```

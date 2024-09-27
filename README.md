# Reto13
## Punto 1 
```pythondef imprimir_valores():
    # Bucle infinito para asegurar que el usuario ingrese un número entero válido
    while True:
        try:
            # Solicita al usuario la cantidad de elementos que tendrá el diccionario
            cantidad_elementos = int(input("Por favor ingrese la cantidad de elementos(clave-valor) que va a tener el diccionario"))
        except ValueError:
            # Si el usuario ingresa un valor no entero, muestra un mensaje de error
            print("Por favor ingrese un numero entero.")
        break  # Sale del bucle después de la primera iteración, independientemente de si el valor es válido o no

    diccionario = {}

    # Bucle para llenar el diccionario con la cantidad de elementos especificada
    for i in range(cantidad_elementos):
        clave = input("Por favor ingrese una clave: ")  # Solicita la clave al usuario
        diccionario[clave] = input("Por favor ingresa el valor de esa clave:")  # Solicita el valor correspondiente a la clave

    valores = list(diccionario.values())  # Obtiene todos los valores del diccionario
    valores_ordenados = sorted(valores)  # Ordena los valores de forma ascendente

    # Imprime los valores ordenados
    print(f"Los valores del diccionario ordenados de forma ascendente son {valores_ordenados}")


if __name__ == "__main__":
    imprimir_valores()  

```
## Punto 2 
```python
def unir_diccionarios(diccionario1: dict, diccionario2: dict):
    diccionario_nuevo = {}  # Inicializa un nuevo diccionario vacío
    diccionario_nuevo.update(diccionario1)  # Copia todos los elementos de diccionario1 a diccionario_nuevo
    for key in diccionario2:  # Itera sobre cada clave en diccionario2
        if key not in diccionario_nuevo:  # Si la clave no está en diccionario_nuevo
            diccionario_nuevo[key] = diccionario2[key]  # Añade la clave y su valor correspondiente de diccionario2 a diccionario_nuevo
        else:
            continue  # Si la clave ya está en diccionario_nuevo, no hace nada y continúa con la siguiente clave
    return diccionario_nuevo  # Devuelve el diccionario combinado

if __name__ == "__main__":
    diccionario = {
        "david" : 17,
        "isabella" : 18,
        "carlos": 60,
        "juan": 27,
        "maria jose": 20 
    }

    diccionario1 = {
        "david" : 21,
        "mariana" : 18,
        "diego": 60,
        "juan": 32,
        "ariana": 20 
    }
    # Imprime el diccionario resultante de unir diccionario y diccionario1
    print(f"El diccionario resultante es {unir_diccionarios(diccionario, diccionario1)}")
```
## Punto 3
```python
import json

# Ruta del archivo JSON que contiene los datos
archivo = r"C:\Users\USUARIO\Documents\UNAL\Programacion de computadoras\Reto 13\archivo.json"

def deporte():
    # Solicita al usuario el nombre de un deporte
    deporte = str(input("Ingrese el nombre del deporte: "))
    
    # Abre el archivo JSON en modo lectura y carga los datos en un diccionario
    with open(archivo, "r") as read_file:
        datos = dict(json.load(read_file))

    diccionario = {}  # Inicializa un diccionario vacío

    # Itera sobre cada persona y su información en los datos
    for persona, info in datos.items():
        # Si el deporte ingresado está en la lista de deportes de la persona
        if deporte in info['deportes']:
            diccionario["nombres"] = info["nombres"]  # Añade el nombre al diccionario
            diccionario["apellidos"] = info["apellidos"]  # Añade el apellido al diccionario
            # Imprime el nombre completo de la persona
            print(f"Nombre completo: {diccionario['nombres']} {diccionario['apellidos']}")

def edades():
    # Solicita al usuario el rango de edades
    x = int(input("Ingrese el limite inferior del rango de edades: "))
    y = int(input("Ingrese el limite superior del rango de edades: "))
    
    # Abre el archivo JSON en modo lectura y carga los datos en un diccionario
    with open(archivo, "r") as read_file:
        datos = dict(json.load(read_file))
    
    diccionario = {} 

    # Itera sobre cada persona y su información en los datos
    for persona, info in datos.items():
        # Si la edad de la persona está dentro del rango especificado
        if int(info["edad"]) in range(x, y):
            diccionario["nombres"] = info["nombres"]  
            diccionario["apellidos"] = info["apellidos"]  
            # Imprime el nombre completo de la persona
            print(f"Nombre completo: {diccionario['nombres']} {diccionario['apellidos']}")


if __name__ == "__main__":
    deporte()  # 
    edades()  
```

## Punto 4 
```python

import json
from datetime import datetime

# Función para convertir una marca de tiempo UTC a un formato de fecha y hora legible.
def utc_a_datos(utc_timestamp) -> str:
    return datetime.utcfromtimestamp(utc_timestamp / 1000).strftime('%Y-%m-%d %H:%M:%S')


# Función para detectar alertas en los datos proporcionados
def detectar_alertas(data):

    # Lista de posibles alertas que se van a buscar en los datos.
    alertas = ["alertPrecip", "alertAlertas", "alertVelViento", "alertTmpMax", "alertTmpMin"]
    # Itera sobre cada tipo de alerta definida en la lista anterior.
    for alerta in alertas:
        for i in range(len(data['dt'])):
            alerta_value = data[alerta][str(i)]

            # Si el valor de la alerta es "X", significa que hay una alerta activa
            if alerta_value == "X":
                fecha = utc_a_datos(data['date'][str(i)])

                # Si el valor de la alerta es "X", significa que hay una alerta activa
                if alerta == "alertPrecip":
                    descripcion = data['weather'][str(i)][0]['description'] if data['weather'][str(i)] else "No disponible"
                    print(f"Fecha: {fecha}, Alerta: {alerta}, Descripción: {descripcion}")

                
                elif alerta == "alertVelViento":
                    vel_viento = data['velViento'][str(i)]
                    print(f"Fecha: {fecha}, Alerta: {alerta}, Velocidad del viento: {vel_viento} m/s")
                
            
                elif alerta == "alertTmpMax":
                    tmp_max = data['tmpMax'][str(i)]
                    print(f"Fecha: {fecha}, Alerta: {alerta}, Temperatura máxima: {tmp_max}°C")

                
                elif alerta == "alertTmpMin":
                    tmp_min = data['tmpMin'][str(i)]
                    print(f"Fecha: {fecha}, Alerta: {alerta}, Temperatura mínima: {tmp_min}°C")
                
                
                else:
                    # Si no es ninguna de las alertas anteriores, continúa con la siguiente.
                    continue

if __name__ == "__main__":
    
    jsonString = '''
{\"dt\": {\"0\": 1685116800, \"1\": 1685203200, \"2\": 1685289600, \"3\": 1685376000, \"4\": 1685462400, \"5\": 1685548800, \"6\": 1685635200, \"7\": 1685721600}, \"sunrise\": {\"0\": 1685097348, \"1\": 1685183745, \"2\": 1685270143, \"3\": 1685356542, \"4\": 1685442942, \"5\": 1685529342, \"6\": 1685615743, \"7\": 1685702145}, \"sunset\": {\"0\": 1685143042, \"1\": 1685229458, \"2\": 1685315875, \"3\": 1685402291, \"4\": 1685488708, \"5\": 1685575124, \"6\": 1685661541, \"7\": 1685747958}, \"moonrise\": {\"0\": 1685118300, \"1\": 1685207460, \"2\": 1685296620, \"3\": 1685385720, \"4\": 1685474880, \"5\": 1685564220, \"6\": 1685653740, \"7\": 1685743500}, \"moonset\": {\"0\": 0, \"1\": 1685164320, \"2\": 1685253000, \"3\": 1685341560, \"4\": 1685430120, \"5\": 1685518740, \"6\": 1685607600, \"7\": 1685696640}, \"moon_phase\": {\"0\": 0.22, \"1\": 0.25, \"2\": 0.28, \"3\": 0.31, \"4\": 0.35, \"5\": 0.38, \"6\": 0.41, \"7\": 0.45}, \"pressure\": {\"0\": 1011, \"1\": 1012, \"2\": 1012, \"3\": 1012, \"4\": 1012, \"5\": 1012, \"6\": 1012, \"7\": 1011}, \"humidity\": {\"0\": 85, \"1\": 61, \"2\": 68, \"3\": 74, \"4\": 84, \"5\": 66, \"6\": 81, \"7\": 82}, \"dew_point\": {\"0\": 23.93, \"1\": 22.5, \"2\": 23.67, \"3\": 23.35, \"4\": 24.22, \"5\": 22.73, \"6\": 23.18, \"7\": 22.93}, \"velViento\": {\"0\": 3.56, \"1\": 5.07, \"2\": 5.38, \"3\": 3.95, \"4\": 4.74, \"5\": 3.75, \"6\": 4.08, \"7\": 5.94}, \"dirViento\": {\"0\": 188, \"1\": 14, \"2\": 21, \"3\": 23, \"4\": 40, \"5\": 330, \"6\": 176, \"7\": 168}, \"wind_gust\": {\"0\": 6.47, \"1\": 8.86, \"2\": 8.95, \"3\": 6.12, \"4\": 7.17, \"5\": 5.4, \"6\": 5.13, \"7\": 9.67}, \"weather\": {\"0\": [{\"id\": 501, \"main\": \"Rain\", \"description\": \"lluvia moderada\", \"icon\": \"10d\"}], \"1\": [{\"id\": 500, \"main\": \"Rain\", \"description\": \"lluvia ligera\", \"icon\": \"10d\"}], \"2\": [{\"id\": 501, \"main\": \"Rain\", \"description\": \"lluvia moderada\", \"icon\": \"10d\"}], \"3\": [{\"id\": 500, \"main\": \"Rain\", \"description\": \"lluvia ligera\", \"icon\": \"10d\"}], \"4\": [{\"id\": 501, \"main\": \"Rain\", \"description\": \"lluvia moderada\", \"icon\": \"10d\"}], \"5\": [{\"id\": 500, \"main\": \"Rain\", \"description\": \"lluvia ligera\", \"icon\": \"10d\"}], \"6\": [{\"id\": 500, \"main\": \"Rain\", \"description\": \"lluvia ligera\", \"icon\": \"10d\"}], \"7\": [{\"id\": 500, \"main\": \"Rain\", \"description\": \"lluvia ligera\", \"icon\": \"10d\"}]}, \"clouds\": {\"0\": 100, \"1\": 82, \"2\": 99, \"3\": 100, \"4\": 100, \"5\": 59, \"6\": 100, \"7\": 100}, \"pop\": {\"0\": 1.0, \"1\": 0.65, \"2\": 0.98, \"3\": 0.86, \"4\": 1.0, \"5\": 0.62, \"6\": 0.93, \"7\": 0.95}, \"prcp\": {\"0\": 40.0, \"1\": 1.65, \"2\": 14.01, \"3\": 5.07, \"4\": 16.55, \"5\": 2.17, \"6\": 2.77, \"7\": 1.73}, \"uvi\": {\"0\": 10.14, \"1\": 12.78, \"2\": 12.73, \"3\": 8.44, \"4\": 0.59, \"5\": 1.0, \"6\": 1.0, \"7\": 1.0}, \"temp.day\": {\"0\": 26.62, \"1\": 30.95, \"2\": 30.17, \"3\": 28.37, \"4\": 27.22, \"5\": 29.78, \"6\": 26.83, \"7\": 26.36}, \"tmpMin\": {\"0\": 25.64, \"1\": 24.64, \"2\": 25.84, \"3\": 25.56, \"4\": 25.72, \"5\": 24.86, \"6\": 25.96, \"7\": 25.47}, \"tmpMax\": {\"0\": 27.16, \"1\": 31.1, \"2\": 30.2, \"3\": 29.5, \"4\": 28.87, \"5\": 29.78, \"6\": 28.96, \"7\": 28.25}, \"temp.night\": {\"0\": 25.67, \"1\": 27.39, \"2\": 26.24, \"3\": 27.2, \"4\": 25.92, \"5\": 27.14, \"6\": 26.56, \"7\": 25.66}, \"temp.eve\": {\"0\": 25.91, \"1\": 28.73, \"2\": 27.42, \"3\": 28.27, \"4\": 27.94, \"5\": 29.29, \"6\": 28.96, \"7\": 28.12}, \"temp.morn\": {\"0\": 26.5, \"1\": 24.64, \"2\": 26.13, \"3\": 25.72, \"4\": 26.04, \"5\": 24.86, \"6\": 25.98, \"7\": 25.57}, \"feels_like.day\": {\"0\": 26.62, \"1\": 34.99, \"2\": 34.96, \"3\": 32.03, \"4\": 30.67, \"5\": 33.62, \"6\": 29.45, \"7\": 26.36}, \"feels_like.night\": {\"0\": 26.56, \"1\": 30.98, \"2\": 26.24, \"3\": 30.62, \"4\": 26.84, \"5\": 30.16, \"6\": 26.56, \"7\": 26.45}, \"feels_like.eve\": {\"0\": 26.85, \"1\": 32.49, \"2\": 30.94, \"3\": 31.8, \"4\": 31.51, \"5\": 33.17, \"6\": 32.64, \"7\": 31.18}, \"feels_like.morn\": {\"0\": 26.5, \"1\": 25.48, \"2\": 26.13, \"3\": 26.62, \"4\": 26.04, \"5\": 25.73, \"6\": 25.98, \"7\": 26.4}, \"date\": {\"0\": 1685098800000, \"1\": 1685185200000, \"2\": 1685271600000, \"3\": 1685358000000, \"4\": 1685444400000, \"5\": 1685530800000, \"6\": 1685617200000, \"7\": 1685703600000}, \"main\": {\"0\": \"Rain\", \"1\": \"Rain\", \"2\": \"Rain\", \"3\": \"Rain\", \"4\": \"Rain\", \"5\": \"Rain\", \"6\": \"Rain\", \"7\": \"Rain\"}, \"description\": {\"0\": \"lluvia moderada\", \"1\": \"lluvia ligera\", \"2\": \"lluvia moderada\", \"3\": \"lluvia ligera\", \"4\": \"lluvia moderada\", \"5\": \"lluvia ligera\", \"6\": \"lluvia ligera\", \"7\": \"lluvia ligera\"}, \"icono\": {\"0\": \"10d\", \"1\": \"10d\", \"2\": \"10d\", \"3\": \"10d\", \"4\": \"10d\", \"5\": \"10d\", \"6\": \"10d\", \"7\": \"10d\"}, \"alertPrecip\": {\"0\": \"X\", \"1\": \"-\", \"2\": \"-\", \"3\": \"-\", \"4\": \"-\", \"5\": \"-\", \"6\": \"-\", \"7\": \"-\"}, \"alertAlertas\": {\"0\": \"-\", \"1\": \"-\", \"2\": \"-\", \"3\": \"-\", \"4\": \"-\", \"5\": \"-\", \"6\": \"-\", \"7\": \"-\"}, \"alertVelViento\": {\"0\": \"-\", \"1\": \"-\", \"2\": \"X\", \"3\": \"-\", \"4\": \"-\", \"5\": \"-\", \"6\": \"-\", \"7\": \"-\"}, \"alertTmpMax\": {\"0\": \"-\", \"1\": \"-\", \"2\": \"-\", \"3\": \"-\", \"4\": \"-\", \"5\": \"X\", \"6\": \"-\", \"7\": \"-\"}, \"alertTmpMin\": {\"0\": \"-\", \"1\": \"X\", \"2\": \"-\", \"3\": \"-\", \"4\": \"-\", \"5\": \"-\", \"6\": \"-\", \"7\": \"-\"}, \"recomendaciones\": {\"lluvias\": \"Realice una revisi\\u00f3n y limpieza a la red de desague y canales existentes ENTER8 Cuente con una estaci\\u00f3n de bombeo, que debe estar ubicada en el punto m\\u00e1s bajo del predio. Aseg\\u00farese de encender y probar el sistema de bombeo al menos una vez al mes y hacer un mantenimiento mensual al equipo de bombeoENTER8 Los productos alojados en zonas de almacenamiento deben mantenersen sobre estibas - estanterias, con el fin de que no entren en contacto directo con el agua.\", \"vientos\": \"-\", \"temperatura\": \"-\"}}
'''
    datos = json.loads(jsonString)
    detectar_alertas(datos)
```

## Punto 5
``` python
import json
import requests

# Función para imprimir pares clave : valor
def imprimir_pares(data):
    for key, value in data.items(): # Itera sobre los elementos del diccionario.
        print(f"{key}: {value}") # Imprime cada clave con su respectivo valor.

# API 1: Open Trivia Database
trivia_url = 'https://opentdb.com/api.php?amount=1'
trivia_response = requests.get(trivia_url) # Realiza la solicitud GET a la API.
if trivia_response.status_code == 200: # Se verifica si hubo una respuesta exitosa.
    trivia_data = trivia_response.json() # Convierte la respuesta en un diccionario de Python.
    print("Datos de la API Open trivia Database:")
    imprimir_pares(trivia_data) # Llama a la función para imprimir los datos de la API.
else:
    print(f"Error al acceder a Open Trivia Database") # Mensaje de error, por si la solicitud falla.

# API 2: Datos sobre la última misión espacial de SpaceX
spacex_url = "https://api.spacexdata.com/v4/launches/latest"
spacex_response = requests.get(spacex_url) # Realiza la solicitud GET a la API.
if spacex_response.status_code == 200: # Se verifica si hubo una respuesta exitosa.
    spacex_data = spacex_response.json() # Convierte la respuesta en un diccionario de Python.
    print("Datos de la API SpaceX (Último lanzamiento):")
    imprimir_pares(spacex_data) # Llama a la función para imprimir los datos de la API.
else:
    print(f"Error al acceder a SpaceX API") # Mensaje de error, por si la solicitud falla.


cripto_url = "https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum&vs_currencies=usd" #Se hace exactamente lo mismo que con las dos API anteriores.
cripto_response = requests.get(cripto_url) 
if cripto_response.status_code == 200: 
    cripto_data = cripto_response.json() 
    print("Datos de la API cripto:")
    imprimir_pares(cripto_data) 
else:
    print(f"Error al acceder a cripto") 
```
 

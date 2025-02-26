# The-last-Journey

=============================================================

El objetivo era resolver el reto 11, el cual consistia en
usar metodos y operadores de diccionarios, 
su funcion,
A continuacion adjunto la imagen del repo del profe Felipe:

=============================================================

![image](https://github.com/user-attachments/assets/b4b59ae4-883a-497c-b70d-5f4cfd7e0586)

=============================================================

#Punto 1

``` python

def crear_diccionario():
    diccionario = {}
    n = int(input("Ingrese la cantidad de elementos para el diccionario: "))
    
    if n <= 0:
        print("Debe ingresar al menos un elemento.")
        return {}
    
    tipo_dato = input("¿Qué tipo de datos desea ingresar? (entero/decimal/texto): ").lower()
    
    for i in range(n):
        clave = input(f"Ingrese la clave {i+1}: ")
        
        if clave in diccionario:
            print(f"La clave '{clave}' ya existe. Intente con otra clave.")
            i -= 1
            continue
        
        if tipo_dato == "entero":
            valor_str = input(f"Ingrese el valor para '{clave}' (debe ser un entero): ")
            if valor_str.lstrip('-').isdigit():  # Permitir números negativos
                valor = int(valor_str)
                diccionario[clave] = valor
            else:
                print("Error: El valor ingresado no es un entero. Intente nuevamente.")
                i -= 1
        
        elif tipo_dato == "decimal":
            valor_str = input(f"Ingrese el valor para '{clave}' (debe ser un decimal): ")
            # Verificar si es un número decimal válido
            if valor_str.replace('.', '', 1).lstrip('-').isdigit() and valor_str.count('.') <= 1:
                valor = float(valor_str)
                diccionario[clave] = valor
            else:
                print("Error: El valor ingresado no es un decimal. Intente nuevamente.")
                i -= 1
        
        elif tipo_dato == "texto":
            valor = input(f"Ingrese el valor para '{clave}' (texto): ")
            diccionario[clave] = valor
        
        else:
            print("Tipo de dato no válido. Use entero, decimal o texto.")
            return {}
    
    return diccionario

def imprimir_valores_ascendente(diccionario):
    # Extraer todos los valores del diccionario
    valores = list(diccionario.values())
    
    # Verificar si el diccionario está vacío
    if len(valores) == 0:
        print("El diccionario está vacío.")
        return
    
    # Ordenar los valores de manera ascendente
    valores_ordenados = sorted(valores)
    
    # Imprimir los valores ordenados
    print("\nValores ordenados de manera ascendente:")
    for valor in valores_ordenados:
        print(valor)

def mostrar_diccionario(diccionario):
    if len(diccionario) == 0:
        print("\nEl diccionario está vacío.")
    else:
        print("\nContenido actual del diccionario:")
        for clave, valor in diccionario.items():
            print(f" {clave}: {valor}")

# Menú principal
def main():
    mi_diccionario = {}
    
    while True:
        print("\n=== GESTIÓN DE DICCIONARIO ===")
        print("1. Crear nuevo diccionario")
        print("2. Mostrar diccionario")
        print("3. Imprimir valores en orden ascendente")
        print("4. Salir")
        
        opcion = input("\nElige una opción (1-4): ")
        
        if opcion == "1":
            mi_diccionario = crear_diccionario()
        elif opcion == "2":
            mostrar_diccionario(mi_diccionario)
        elif opcion == "3":
            imprimir_valores_ascendente(mi_diccionario)
        elif opcion == "4":
            print("¡Hasta luego!")
            break
        else:
            print("Opción no válida. Inténtalo de nuevo.")

if __name__ == "__main__":
    main()
```

# Punto 2
``` python

def mezclar_diccionarios(dict1, dict2):
    """
    Mezcla dos diccionarios en uno nuevo. Si hay claves repetidas,
    se mantiene el valor de la clave en el primer diccionario.
    
    Args:
        dict1: Primer diccionario
        dict2: Segundo diccionario
        
    Returns:
        Diccionario mezclado
    """
    # Creamos una copia del segundo diccionario
    resultado = dict2.copy()
    
    # Añadimos los elementos del primer diccionario
    # Si hay claves repetidas, se sobreescribirán los valores del segundo diccionario
    for clave, valor in dict1.items():
        resultado[clave] = valor
        
    return resultado

# Función para crear un diccionario interactivamente
def crear_diccionario():
    """
    Permite al usuario crear un diccionario introduciendo pares clave-valor.
    
    Returns:
        Diccionario creado por el usuario
    """
    diccionario = {}
    print("Creación de diccionario (introduce una clave vacía para terminar):")
    
    while True:
        clave = input("Introduce una clave: ")
        if clave == "":
            break
            
        valor = input(f"Introduce el valor para '{clave}': ")
        diccionario[clave] = valor
        
    return diccionario

# Ejemplo de uso
if __name__ == "__main__":
    print("Vamos a crear el primer diccionario")
    dict1 = crear_diccionario()
    
    print("\nVamos a crear el segundo diccionario")
    dict2 = crear_diccionario()
    
    resultado = mezclar_diccionarios(dict1, dict2)
    
    print("\nPrimer diccionario:", dict1)
    print("Segundo diccionario:", dict2)
    print("Diccionario mezclado:", resultado)

```
#Punto 3

``` python

import json

def cargar_datos(nombre_archivo):
    with open(nombre_archivo, 'r', encoding='utf-8') as archivo:
        return json.load(archivo)

def buscar_por_deporte(datos, deporte):
    resultados = []
    
    for usuario, info in datos.items():
        # Convertimos el deporte a minúsculas para hacer búsqueda insensible a mayúsculas/minúsculas
        deportes_minusculas = [d.lower() for d in info["deportes"]]
        
        if deporte.lower() in deportes_minusculas:
            nombre_completo = f"{info['nombres']} {info['apellidos']}"
            resultados.append(nombre_completo)
    
    return resultados

def buscar_por_rango_edad(datos, edad_min, edad_max):
    resultados = []
    
    for usuario, info in datos.items():
        if edad_min <= info["edad"] <= edad_max:
            nombre_completo = f"{info['nombres']} {info['apellidos']}"
            resultados.append(nombre_completo)
    
    return resultados

def main():
    # Nombre del archivo JSON
    nombre_archivo = "personas.json"
    
    # Cargar los datos
    datos = cargar_datos(nombre_archivo)
    
    while True:
        print("\n--- Menú ---")
        print("1. Buscar por deporte")
        print("2. Buscar por rango de edad")
        print("3. Salir")
        
        opcion = input("\nSeleccione una opción (1-3): ")
        
        if opcion == "1":
            deporte = input("Ingrese el deporte a buscar: ")
            resultados = buscar_por_deporte(datos, deporte)
            
            if resultados:
                print(f"\nPersonas que practican {deporte}:")
                for nombre in resultados:
                    print(f"- {nombre}")
            else:
                print(f"\nNo se encontraron personas que practiquen {deporte}")
                
        elif opcion == "2":
            edad_min = int(input("Ingrese la edad mínima: "))
            edad_max = int(input("Ingrese la edad máxima: "))
            
            if edad_min > edad_max:
                print("Error: La edad mínima no puede ser mayor que la edad máxima")
                continue
            
            resultados = buscar_por_rango_edad(datos, edad_min, edad_max)
            
            if resultados:
                print(f"\nPersonas entre {edad_min} y {edad_max} años:")
                for nombre in resultados:
                    print(f"- {nombre}")
            else:
                print(f"\nNo se encontraron personas en el rango de edad {edad_min}-{edad_max}")
                
        elif opcion == "3":
            print("¡Hasta luego!")
            break
            
        else:
            print("Opción no válida. Por favor, seleccione una opción del 1 al 3.")
            
if __name__ == "__main__":
    main()

```

#Punto 4

``` python

import json
import datetime

# Cargar el JSON desde un archivo
def load_json_file(file_path):
    with open(file_path, 'r', encoding='utf-8') as file:
        return json.load(file)

# Ruta al archivo JSON (ajusta esto según donde tengas el archivo)
json_file_path = 'datos_clima.json'

# Cargar los datos desde el archivo
data = load_json_file(json_file_path)

# Función para convertir timestamp UTC a fecha legible
def convert_utc_to_date(utc_timestamp):
    return datetime.datetime.fromtimestamp(utc_timestamp).strftime('%Y-%m-%d %H:%M:%S')

# Campos de alerta a revisar
alert_fields = ['alertAlertas', 'alertPrecip', 'alertVelViento', 'alertTmpMax', 'alertTmpMin']

# Mapeo de tipos de alerta con sus parámetros relevantes
alert_params = {
    'alertAlertas': ['weather'],
    'alertPrecip': ['prcp', 'description'],
    'alertVelViento': ['velViento', 'dirViento', 'wind_gust'],
    'alertTmpMax': ['tmpMax', 'feels_like.day'],
    'alertTmpMin': ['tmpMin', 'feels_like.night']
}

# Para almacenar las alertas encontradas
alerts_found = []

# Revisar cada campo de alerta
for alert_field in alert_fields:
    # Revisar cada día
    for day in data[alert_field]:
        # Si hay una alerta (marcada con "X")
        if data[alert_field][day] == "X":
            # Obtener la fecha UTC y convertirla
            date_utc = data['dt'][day]
            date_readable = convert_utc_to_date(date_utc)
            
            # Crear diccionario para esta alerta
            alert_info = {
                'fecha': date_readable,
                'tipo_alerta': alert_field,
                'parametros': {}
            }
            
            # Recopilar los parámetros relevantes para este tipo de alerta
            for param in alert_params[alert_field]:
                if param in data:
                    # Para campos simples
                    alert_info['parametros'][param] = data[param][day]
                elif '.' in param:
                    # Para campos con punto (como feels_like.day)
                    main_field, sub_field = param.split('.')
                    if main_field in data:
                        field_key = f"{main_field}.{sub_field}"
                        if field_key in data:
                            alert_info['parametros'][param] = data[field_key][day]
                
                # Caso especial para weather que es una lista de diccionarios
                if param == 'weather' and param in data:
                    alert_info['parametros'][param] = data[param][day][0]['description']
            
            # Añadir esta alerta a nuestra lista
            alerts_found.append(alert_info)

# Imprimir las alertas encontradas
print("ALERTAS CLIMÁTICAS ENCONTRADAS:")
print("-" * 50)

for alert in alerts_found:
    print(f"Fecha: {alert['fecha']}")
    
    # Traducir el tipo de alerta a algo más legible
    alert_type = alert['tipo_alerta']
    if alert_type == 'alertPrecip':
        print("Tipo: Alerta de Precipitación")
    elif alert_type == 'alertVelViento':
        print("Tipo: Alerta de Velocidad del Viento")
    elif alert_type == 'alertTmpMax':
        print("Tipo: Alerta de Temperatura Máxima")
    elif alert_type == 'alertTmpMin':
        print("Tipo: Alerta de Temperatura Mínima")
    elif alert_type == 'alertAlertas':
        print("Tipo: Alerta General")
    
    # Imprimir parámetros
    print("Parámetros:")
    for param, value in alert['parametros'].items():
        if param == 'prcp':
            print(f"  - Nivel de lluvia: {value} mm")
        elif param == 'description':
            print(f"  - Descripción: {value}")
        elif param == 'velViento':
            print(f"  - Velocidad del viento: {value} m/s")
        elif param == 'dirViento':
            print(f"  - Dirección del viento: {value}°")
        elif param == 'wind_gust':
            print(f"  - Ráfagas de viento: {value} m/s")
        elif param == 'tmpMax':
            print(f"  - Temperatura máxima: {value}°C")
        elif param == 'tmpMin':
            print(f"  - Temperatura mínima: {value}°C")
        elif param == 'feels_like.day':
            print(f"  - Sensación térmica (día): {value}°C")
        elif param == 'feels_like.night':
            print(f"  - Sensación térmica (noche): {value}°C")
        elif param == 'weather':
            print(f"  - Condición meteorológica: {value}")
    
    print("-" * 50)

```

#Punto 5

``` python
import requests
import json
from pprint import pprint

def connect_to_api(url, params=None, headers=None):
    response = requests.get(url, params=params, headers=headers)
    return response.json()

def extract_key_value_pairs(json_data, prefix=""):
    pairs = []
    
    if isinstance(json_data, dict):
        for key, value in json_data.items():
            current_key = f"{prefix}.{key}" if prefix else key
            
            if isinstance(value, (dict, list)):
                pairs.extend(extract_key_value_pairs(value, current_key))
            else:
                pairs.append((current_key, value))
                
    elif isinstance(json_data, list):
        for i, item in enumerate(json_data):
            current_key = f"{prefix}[{i}]"
            pairs.extend(extract_key_value_pairs(item, current_key))
            
    return pairs

def print_api_data(api_name, json_data):
    print("\n" + "="*50)
    print(f"DATOS DE LA API: {api_name}")
    print("="*50)
    
    print("\nJSON COMPLETO:")
    pprint(json_data)
    
    print("\nPARES CLAVE-VALOR:")
    pairs = extract_key_value_pairs(json_data)
    for key, value in pairs[:20]:
        print(f"{key}: {value}")
    
    if len(pairs) > 20:
        print(f"... y {len(pairs) - 20} pares más")

print("\nConectando a Bored API...")
bored_api_url = "https://www.boredapi.com/api/activity"
bored_data = connect_to_api(bored_api_url)
print_api_data("Bored API (Sugerencias de actividades)", bored_data)

print("\nConectando a Open Brewery DB...")
brewery_api_url = "https://api.openbrewerydb.org/breweries/random"
brewery_data = connect_to_api(brewery_api_url)
print_api_data("Open Brewery DB (Cervecerías aleatorias)", brewery_data)

print("\nConectando a CoinDesk API...")
coindesk_api_url = "https://api.coindesk.com/v1/bpi/currentprice.json"
coindesk_data = connect_to_api(coindesk_api_url)
print_api_data("CoinDesk API (Precio de Bitcoin)", coindesk_data)

print("\nPrograma finalizado")

```

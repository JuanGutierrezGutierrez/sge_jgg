# Proyecto de la 1ª Evaluación
## Sistema de gestión de tareas pendientes


```python
# Una tarea es un diccionario
# La lista de tareas es una lista de diccionarios

import datetime
import csv

CAMPOS_TAREA = ["indice", "nombre", "prioridad", "fecha", "esta_completada"] #Campos del archivo CSV
lista_tareas = []


#Funciones auxiliares
def indice_tarea_en_lista_indices(tarea, lista_indices):
    '''Comprueba si el indice de una tarea se encuentra en una lista de indices. Se utiliza para borrar multiples tareas a la vez'''
    return tarea["indice"] in lista_indices

def actualizar_indice_tarea(tarea, lista_tareas):
    '''Dada una tarea y una lista de tareas, devuelve una copia de la tarea con su posicion dentro de la lista asignado como indice. Se utiliza para actualizar los indices despues de borrar tareas'''
    tarea_output = dict(tarea)
    tarea_output["indice"] = str(lista_tareas.index(tarea) + 1)
    return tarea_output

def tiene_indice(tarea, indice):
    '''Dada una tarea y un indice, devuelve verdadero si la tarea tiene ese valor de indice. Se utiliza para filtrar la lista de tareas a partir del indice'''
    return tarea["indice"] == indice
    
#Generar e insertar tareas en la lista 
def importar_tarea(indice, nombre, prioridad, fecha, esta_completada):
    '''Dada la informacion de una tarea obtenida de un fichero csv, devuelve un diccionario que representa una tarea'''
    return {
        "indice" : indice,
        "nombre" : nombre,
        "prioridad" : prioridad,
        "fecha" : fecha,
        "esta_completada" : esta_completada
    }

def generar_tarea(nombre, prioridad, fecha): 
    '''Dada la informacion de una tarea introducida por el usuario, devuelve un diccionario que representa una tarea'''
    return {
        "indice" : str(len(lista_tareas) + 1),
        "nombre" : nombre,
        "prioridad" : prioridad,
        "fecha" : fecha,
        "esta_completada" : "Pendiente"
    }
    
    
def insertar_tarea(lista_tareas, tarea):
    '''Dada una lista de tareas, devuelve una copia de la lista tras insertar una tarea'''
    lista_tareas_output = lista_tareas[:]
    lista_tareas_output.append(tarea)
    return lista_tareas_output
    

#Borrar tareas de la lista
def borrar_tarea(lista_tareas, indice):
    '''Dado un indice y una lista de tareas, devuelve una copia de la lista tras borrar la tarea situada en ese indice'''
    lista_tareas_output = lista_tareas[:]
    lista_tareas_output.pop(int(indice) - 1)

    lista_tareas_output = actualizar_indices(lista_tareas_output)
    return lista_tareas_output

def borrar_multiples_tareas(lista_tareas, lista_indices):
    '''Dada una lista de indices y una lista de tareas, devuelve una copia de la lista tras borrarlas tareas situadas en esos indices'''
    lista_tareas_output = lista_tareas[:]
    lista_tareas_output = [tarea for tarea in lista_tareas_output if not indice_tarea_en_lista_indices(tarea, lista_indices)]
    
    lista_tareas_output = actualizar_indices(lista_tareas_output)
    return lista_tareas_output


#Modificar el estado de completado de tareas
def completar_tarea(tarea):
    '''Dada una tarea, devuelve una copia de la tarea con el estado de completada'''
    tarea_output = dict(tarea)
    tarea_output["esta_completada"] = "Completada"
    return tarea_output

def completar_tarea_en_lista(lista_tareas, indice):
    '''Dada una lista de tareas y un indice, devuelve una copia de la lista en la cual la tarea en ese indice se encuentra completada'''
    lista_tareas_output = lista_tareas[:]
    lista_tareas_output[int(indice) - 1] = completar_tarea(lista_tareas_output[int(indice) - 1])
    return lista_tareas_output


#Actualizar los indices de las tareas
def actualizar_indices(lista_tareas):
    '''Dada una lista de tareas, devuelve una copia con los indices de las tareas modificados para reflejar su posicion dentro de la lista. Se utiliza despues de acciones de borrado'''
    lista_tareas_output = lista_tareas[:]
    lista_tareas_output = [actualizar_indice_tarea(tarea, lista_tareas_output) for tarea in lista_tareas_output]
    return lista_tareas_output

#Validar el input del usuario
def validar_prioridad(prioridad):
    '''Dada una prioridad, comprueba si su valor se encuentra dentro de los valores admitidos'''
    return prioridad in ["ALTA", "MEDIA", "BAJA"]

def validar_nombre(nombre):
    '''Dado un nombre, comprueba que su valor sea aceptable y no esté vacio'''
    return ((type(nombre) == str) and nombre != "")

def validar_fecha(fecha):
    '''Dada una fecha, comprobar que utiliza el formato dia/mes/año, y sus valores son validos'''
    componentes_fecha = fecha.split("/")
    if(len(componentes_fecha) != 3):
        return False
    
    #Si al castear a entero lanza una excepcion, habia algun caracter no numerico en el input
    try:    
        dia = int(componentes_fecha[0])
        mes = int(componentes_fecha[1])
        anyo = int(componentes_fecha[2])
    except Exception:
        return False
        
    #Intenta generar una fecha con los valores indicados. Si lanza excepcion es que no es una fecha real, segun la validacion del paquete datetime
    try:
        datetime.date(anyo, mes, dia)
    except ValueError:
        return False
    
    #Si llega aqui es que ha pasado todos los controles, y es una fecha valida
    return True

    
def validar_creacion_tarea(nombre, prioridad, fecha):
    '''Dados los inputs de un usuario para crear una tarea, comprueba que todos los valores sean validos'''
    return validar_nombre(nombre) and validar_prioridad(prioridad) and validar_fecha(fecha)
    

def validar_input_creacion_tarea(cadena_input):
    '''Dada una cadena introducida por el usuario, comprueba que cumple el formato de entrada requerido'''
    componentes_input = cadena_input.split(",");
    if(len(componentes_input) != 3):
        return False
    return True

def validar_indice_en_lista_tareas(lista_tareas, indice):
    '''Dada una lista de tareas y un indice, devuelve verdadero si existe alguna tarea en la lista con ese indice asociado'''
    lista_filtrada = [tarea for tarea in lista_tareas if tiene_indice(tarea, indice)]
    if(len(lista_filtrada) != 1):
        return False
    
    return True

def validar_input_borrar_tarea(lista_tareas, indice_input):
    '''Dado un indice introducido por el usuario y una lista de tareas, comprueba que es un indice borrable en la lista'''
    #Si no se introduce un numero, es un input invalido
    try:
        indice = int(indice_input)
    except Exception:
        return False

    #Al tener indices unicos, deberia haber una unica tarea con ese indice en la lista
    if(not validar_indice_en_lista_tareas(lista_tareas, indice_input)):
        return False

    #Si la tarea con ese indice esta en la lista, es una tarea borrable
    return True


#Imprimir por pantalla
def imprimir_tarea(tarea):
    '''Dada una tarea, imprime su contenido por pantalla'''
    print(" ".join(tarea.values()))

def imprimir_lista_tareas(lista_tareas):
    '''Dada una lista de tareas, imprime su contenido por pantalla'''
    [imprimir_tarea(tarea) for tarea in lista_tareas]

def imprimir_menu():
    '''Imprime el menu'''
    print("\n1. Añadir tarea")
    print("2. Listar tareas")
    print("3. Marcar tarea como completada")
    print("4. Eliminar tarea")
    print("5. Salir \n")
    

#Importar y exportar CSV
def importar_csv(ruta, lista_tareas):
    '''Dada una lista de tareas y la ruta de un fichero csv, devuelve una copia de la lista de tareas con la informacion del fichero cargada'''
    lista_tareas_output = lista_tareas
    try:
        with open("lista_tareas.csv", "r") as fichero:
            lector = csv.DictReader(fichero)
            for linea in lector:
                lista_tareas_output = insertar_tarea(lista_tareas, importar_tarea(linea["indice"], linea["nombre"], linea["prioridad"], linea["fecha"], linea["esta_completada"]))
    except FileNotFoundError:
        print("Fichero no encontrado")
    except IOError:
        print("Error de escritura/lectura")
        
    return lista_tareas_output

def exportar_csv(ruta, lista_tareas):
    '''Dada una lista de tareas y la ruta de un fichero csv, exporta la información al fichero'''
    try:
        with open("lista_tareas.csv", "w") as fichero:
            escritor = csv.DictWriter(fichero, fieldnames = CAMPOS_TAREA)
            escritor.writeheader()
            [escritor.writerow(tarea) for tarea in lista_tareas]
    except FileNotFoundError:
        print("Fichero no encontrado")
    except IOError:
        print("Error de escritura/lectura")



#Vencimiento de tareas
def es_tarea_vencida(tarea):
    '''Dada una tarea, devuelve verdadero si ya ha pasado la fecha limite de vencimiento.'''
    componentes_fecha = tarea["fecha"].split("/")
    dia = int(componentes_fecha[0])
    mes = int(componentes_fecha[1])
    anyo = int(componentes_fecha[2])
    
    return datetime.date(anyo, mes, dia) < datetime.date.today()
    
def tareas_vencidas(lista_tareas):
    '''Dada una lista de tareas, devuelve una lista con todas aquellas tareas que hayan vencido'''
    lista_tareas_vencidas = list(filter(es_tarea_vencida, lista_tareas))
    return lista_tareas_vencidas



#Manejadores de opciones de menu
def manejar_opcion(opcion):
    '''Dada una opcion, ejecuta el manejador asociado a esa opcion'''
    if(opcion == "1"):
            manejar_opcion_1()
    elif(opcion == "2"):
            manejar_opcion_2()
    elif(opcion == "3"):
            manejar_opcion_3()
    elif(opcion == "4"):
            manejar_opcion_4()

def manejar_opcion_1():
    '''Maneja la creacion e insercion de una nueva tarea'''
    print("Introduce el nombre, prioridad y fecha de la tarea, separadas por una coma")
    print("El nombre no puede estar en blanco")
    print("La prioridad debe tomar un valor ALTA, MEDIA o BAJA")
    print("La fecha tiene que seguir el formato dia/mes/año")
    print("Ejemplo: Comprar el pan,ALTA,3/5/2020 \n");

    global lista_tareas
    
    input_usuario = input("Introduce nombre,prioridad,fecha")
    
    
    #Si no se ha introducido como nombre,prioridad,fecha se termina la ejecucion
    if(not validar_input_creacion_tarea(input_usuario)):
        print("Formato de entrada incorrecto")
        return

    #Si se han introducido valores ilegales se termina la ejecucion
    input_usuario = input_usuario.split(",")
    if(not validar_creacion_tarea(input_usuario[0], input_usuario[1], input_usuario[2])):
        print("Datos introducidos no validos")
        return

    #Si ha pasado todos los tests, se crea la tarea y se añade a la lista
    lista_tareas = insertar_tarea(lista_tareas, generar_tarea(input_usuario[0], input_usuario[1], input_usuario[2]))


def manejar_opcion_2():
    '''Maneja la impresion del listado de tareas existentes'''
    imprimir_lista_tareas(lista_tareas)

def manejar_opcion_3():
    '''Maneja el marcar una tarea como completada a partir de su indice'''
    global lista_tareas
    
    print("Introduce el numero de la tarea que quieras marcar como completada")
    indice_input = input()
    
    if(not validar_indice_en_lista_tareas(lista_tareas, indice_input)):
        return
        
    
    lista_tareas = completar_tarea_en_lista(lista_tareas, indice_input)

def manejar_opcion_4():
    '''Maneja el eliminar una tarea de la lista a partir de su indice'''
    global lista_tareas
    
    print("Introduce el numero de la tarea que quieras eliminar.")
    indice_eliminar = input("Introduce el indice")
    
    
    #Si es un valor ilegal se termina la ejecucion
    if(not(validar_input_borrar_tarea(lista_tareas, indice_eliminar))):
        return
        
    #Si ha pasado los tests, se borra
    lista_tareas = borrar_tarea(lista_tareas, indice_eliminar)

#Bucle principal
def menu():
    '''Bucle infinito que recibe la seleccion de opciones del usuario hasta que se le elija la opcion de cerrar el programa'''
    global lista_tareas
    lista_tareas = importar_csv("", lista_tareas)
    
    eleccion_usuario = "-1"
    while(eleccion_usuario != "5"):
        imprimir_menu()
        eleccion_usuario = input("Introduce un numero para elegir la opcion")
        manejar_opcion(eleccion_usuario)

    exportar_csv("", lista_tareas)
menu()
```



---
[Volver al indice](../)
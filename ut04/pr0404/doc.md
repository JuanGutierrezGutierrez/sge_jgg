# UT04 -El lenguaje de programación Python
## PR0404 - Ejercicios con diccionarios


## 1. Buscar valor en un diccionario
```python
def consultar_fruta(nombre_fruta): 

    frutas = { 

        "Fresa" : 15, 

        "Naranja" : 27, 

        "Limon" : 3, 

        "Kiwi" : 10, 

    } 

     

    return str(frutas.get(nombre_fruta, "Fruta no disponible")) 

     

print(consultar_fruta(input())) 
```




## 2. Contar elementos en un diccionario
```python
def consultar_categorias(): 

    productos = { 

    "Electrónica": ["Smartphone", "Laptop", "Tablet", "Auriculares", "Smartwatch"], 

    "Hogar": ["Aspiradora", "Microondas", "Lámpara", "Sofá", "Cafetera"], 

    "Ropa": ["Camisa", "Pantalones", "Chaqueta", "Zapatos", "Bufanda"], 

    "Deportes": ["Pelota de fútbol", "Raqueta de tenis", "Bicicleta", "Pesas", "Cuerda de saltar"], 

    "Juguetes": ["Muñeca", "Bloques de construcción", "Peluche", "Rompecabezas", "Coche de juguete"], 

    } 

     

    productosTotales = 0 

     

    print("Hay un total de " + str(len(productos)) + " categorias") 

    for categoria in productos: 

        print(categoria + ": " + str(len(productos.get(categoria))) + " productos.") 

        productosTotales += len(productos.get(categoria)) 

    print("Hay un total de " + str(productosTotales) + " productos") 

         

consultar_categorias() 

```



## 3. Contador de frecuencias de palabras
```python
def contar_frecuencia(frase): 

    diccionario = {} 

    for palabra in frase.split(" "): 

        cuenta = 1 if diccionario.get(palabra) is None else diccionario.get(palabra)+1 

        diccionario[palabra] = cuenta 

    print(diccionario) 

  

contar_frecuencia("Esto es es es una frase frase") 
```



## 4. Diccionario de listas
```python
asignaturas = { 

    "Matemáticas": ["Ana", "Carlos", "Luis", "María", "Jorge"], 

    "Física": ["Elena", "Luis", "Juan", "Sofía"], 

    "Programación": ["Ana", "Carlos", "Sofía", "Jorge", "Pedro"], 

    "Historia": ["María", "Juan", "Elena", "Ana"], 

    "Inglés": ["Carlos", "Sofía", "Jorge", "María"], 

} 

  

def diccionario_asignaturas(): 

    while True: 

        mostrar_menu() 

        seleccion = int(input()) 

        match seleccion: 

            case 1: 

                asignatura = input("Introduzca una asignatura") 

                print(asignaturas[asignatura]) 

            case 2: 

                asignatura = input("Introduzca una asignatura") 

                alumno = input("Introduzca un alumno") 

                listaAsignaturas = asignaturas[asignatura] 

                listaAsignaturas.append(alumno) 

                asignaturas[asignatura] = listaAsignaturas 

            case 3: 

                asignatura = input("Introduzca una asignatura") 

                alumno = input("Introduzca un alumno") 

                listaAsignaturas = asignaturas[asignatura] 

                listaAsignaturas.remove(alumno) 

                asignaturas[asignatura] = listaAsignaturas 

     

def mostrar_menu(): 

    print("Elige una opcion\n 1. Listar estudiantes en una asignatura \n 2. Matricular un estudiante en una asignatura \n 3. Dar de baja a un estudiante en una asignatura.\n") 

  

diccionario_asignaturas() 
```




## 5. Diccionario invertido
```python
def diccionario_invertido(): 

    diccionario = { 

        "claveUno" : "valorUno", 

        "claveDos" : "valorDos", 

        "claveTres" : "valorTres", 

    } 

  

    diccionario_invertido = {} 

    for claveAntigua in diccionario.keys(): 

        diccionario_invertido[diccionario[claveAntigua]] = claveAntigua 

  

    print(diccionario_invertido) 

  

diccionario_invertido() 
```




## 6. Combinar dos diccionarios
```python
primerDiccionario = { 

    "ordenador" : 500, 

    "raton" : 10, 

    "alfombrilla" : 5, 

} 

  

  

segundoDiccionario = { 

    "raton" : 10, 

    "pantalla" : 50, 

    "alfombrilla" : 5, 

} 

  

def combinar_diccionarios(diccionarioUno, diccionarioDos): 

    for llave in diccionarioDos: 

        if llave in diccionarioUno: 

            diccionarioUno[llave] += diccionarioDos[llave] 

        else: 

            diccionarioUno[llave] = diccionarioDos[llave] 

     

    return diccionarioUno 

     

print(combinar_diccionarios(primerDiccionario, segundoDiccionario)) 

```


---
[Volver a la Unidad 4](../)
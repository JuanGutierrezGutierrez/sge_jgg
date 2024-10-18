# UT04 -El lenguaje de programación Python
## PR0401 - Ejercicios básicos


## 1. Lectura de un número válido
```python
n = input()
while not n.isdigit():
    print("Introduce un numero")
    n = input()
```




## 2. Tabla de multiplicar
```python
n = input()
k = input()

if n.isdigit() and k.isdigit():
    for iterador in range(1, int(k)+1):
        print(n + " * " + str(iterador) + " = " +  str(int(n)*iterador))
else:
    print("Has introducido un valor incorrecto")

```



## 3. Triangulo de asteriscos
```python
n = int(input())
for iterador in range(1, n+1):
    linea = ""
    for numeroAsteriscos in range(1, iterador+1):
        linea = linea + "*"
    print(linea)
```



## 4. Piramide de asteriscos
```python
n = int(input())

for iterador in range(1, n+1, 2):
    linea = ""
    numeroEspacios = int((n - iterador)/2)
    for segundoIterador in range(1, numeroEspacios+1):
        linea = linea + " "
    for numeroAstericos in range(1, iterador+1):
        linea = linea + "*"
    print(linea)
```




## 5. Numero mayor y menor
```python
numeroMasBajo = int(input())
numeroMasAlto = numeroMasBajo
for iteracion in range(4):
    numeroEstaIteracion = int(input())
    if numeroEstaIteracion < numeroMasBajo:
        numeroMasBajo = numeroEstaIteracion
    if numeroEstaIteracion > numeroMasAlto:
        numeroMasAlto = numeroEstaIteracion
print("El numero mayor es " + str(numeroMasAlto) + " y el menor es " + str(numeroMasBajo))
```




## 6. Conversion de unidades
```python
cantidadInicial = int(input("cantidadInicial"))
unidadInicial = input("unidadInicial")
unidadFinal = input("unidadFinal")
cantidadFinal = 0

match unidadInicial:
    case "milimetros":
        match unidadFinal:
            case "centimetros":
                cantidadFinal = cantidadInicial * 0.1
            case "metros":
                cantidadFinal = cantidadInicial * 10**-3
            case "kilometros":
                cantidadFinal = cantidadInicial * 10**-6
    case "centimetros":
        match unidadFinal:
            case "milimetros":
                cantidadFinal = cantidadInicial * 10
            case "metros":
                cantidadFinal = cantidadInicial * 10**-2
            case "kilometros":
                cantidadFinal = cantidadInicial * 10**-5
    case "metros":
        match unidadFinal:
            case "milimetros":
                cantidadFinal = cantidadInicial * 10**3
            case "centimetros":
                cantidadFinal = cantidadInicial * 10**2
            case "kilometros":
                cantidadFinal = cantidadInicial * 10**-3
    case "kilometros":
        match unidadFinal:
            case "milimetros":
                cantidadFinal = cantidadInicial * 10**6
            case "centimetros":
                cantidadFinal = cantidadInicial * 10**5
            case "metros":
                cantidadFinal = cantidadInicial * 10**3

print(cantidadFinal)

```



## 7. Acierta el numero
```python
from random import *

numeroAleatorio = randint(0, 101)
numeroAdivinado = ""

while(not numeroAleatorio == numeroAdivinado):
    numeroAdivinado = int(input())
    if(numeroAdivinado > numeroAleatorio):
        print("El numero es mas bajo!")
    elif(numeroAdivinado < numeroAleatorio):
        print("El numero es mas alto!")
    else:
        print("Encontraste el numero!")
```



## 8. Piedra, papel o tijeras
```python
from random import *
puntuacionJugador = 0
puntuacionOrdenador = 0

while puntuacionJugador < 5 and puntuacionOrdenador < 5
    manoJugador = input("Introduce tu mano")
    manoOrdenador = ""
    numeroManoOrdenador = randint(0, 5)
    match numeroManoOrdenador:
        case 0:
            manoOrdenador = "piedra"
        case 1:
            manoOrdenador = "papel"
        case 2:
            manoOrdenador = "tijeras"
        case 3:
            manoOrdenador = "lagarto"
        case 4:
            manoOrdenador = "spock"

    match manoJugador:
        case "piedra":
            if manoOrdenador == "lagarto" or manoOrdenador == "tijeras":
                puntuacionJugador++
            else:
                puntuacionOrdenador++
        case "papel":
            if manoOrdenador == "piedra" or manoOrdenador == "spock":
                puntuacionJugador++
            else:
                puntuacionOrdenador++
        case "tijeras":
            if manoOrdenador == "papel" or manoOrdenador == "lagarto":
                puntuacionJugador++
            else:
                puntuacionOrdenador++
        case "lagarto":
            if manoOrdenador == "spock" or manoOrdenador == "papel":
                puntuacionJugador++
            else:
                puntuacionOrdenador++
        case "spock":
            if manoOrdenador == "piedra" or manoOrdenador == "tijeras":
                puntuacionJugador++
            else:
                puntuacionOrdenador++    
```



## 9. Secuencia de Fibonacci
```python
n = int(input())

def fibonacci(valor):
    if valor == 0 or valor == 1:
        return 1
    else:
        return (fibonacci(valor-1)) + (fibonacci(valor-2))

for i in range(n):
    print(fibonacci(i))
```

---
[Volver a la Unidad 4](../)
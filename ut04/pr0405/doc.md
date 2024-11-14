# UT04 -El lenguaje de programación Python
## PR0405 - Ejercicios de programación funcional


## 1. Filtrado de una lista de números
```python
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] 

  

def es_par(numero): 

    return numero % 2 == 0 

  

def filtrado_numeros(listaNumeros): 

    '''Obtener solo numeros pares''' 

    return list(filter(es_par, listaNumeros)) 

  

print(filtrado_numeros(numeros)) 
```




## 2. Mapeo de temperaturas
```python
temperaturas = [0, 20, 37, 100] 

  

def celsius_a_farenheit(temperatura): 

    return temperatura * 1.8 + 32 

  

def mapeo_temperaturas(listaTemperaturas): 

    '''Pasar de celsius a fahrenheit''' 

    return list(map(celsius_a_farenheit, listaTemperaturas)) 

  

print(mapeo_temperaturas(temperaturas)) 
```



## 3. Suma acumulativa
```python
from functools import reduce 

  

numeros = [1, 2, 3, 4, 5] 

  

def suma_acumulativa(listaValores): 

    return reduce(lambda suma, valor: suma + valor, listaValores, 0) 

  

print(suma_acumulativa(numeros)) 
```



## 4. Palabras con cierta longitud
```python
palabras = ["perro", "gato", "elefante", "oso", "jirafa"] 

  

def longitud_mayor_de(palabra): 

    return len(palabra) > 5 

  

def pasar_a_mayuscula(palabra): 

    return palabra.upper() 

  

def filtrar_y_mayusculas(listaPalabras): 

    listaFiltrada = list(filter(longitud_mayor_de, listaPalabras)) 

    return list(map(pasar_a_mayuscula, listaFiltrada)) 

  

print(filtrar_y_mayusculas(palabras)) 
```




## 5. Multiplicación de pares
```python
from functools import reduce 

  

numeros = [1, 2, 3, 4, 5, 6] 

  

def es_par(numero): 

    return numero % 2 == 0 

  

def multiplicar_pares(listaNumeros): 

    listaFiltrada = list(filter(es_par, listaNumeros)) 

    return reduce(lambda resultado, valor: resultado * valor, listaFiltrada, 1) 

  

print(multiplicar_pares(numeros)) 
```




## 6. Combinar operaciones en listas anidadas
```python
from functools import reduce 

  

numeros = [[-3, 2, 7], [10, -5, 3], [0, 8, -2]] 

  

def es_positivo(numero): 

    return numero > 0 

  

def comprimir_a_lista(listaDeListas): 

    listaComprimida = [] 

    for lista in listaDeListas: 

        listaComprimida += lista 

    return listaComprimida 

  

def suma_de_positivos(listaDeListas): 

    listaComprimida = comprimir_a_lista(listaDeListas) 

    listaFiltrada = list(filter(es_positivo, listaComprimida)) 

    return reduce(lambda resultado, valor: resultado + valor, listaFiltrada, 0) 

  

print(suma_de_positivos(numeros)) 

```


---
[Volver a la Unidad 4](../)
# UT04 -El lenguaje de programación Python
## PR0402 - Ejercicios de cadenas


## 1. Contar vocales y consonantes
```python
def contar_letras(palabra): 

    contadorVocales = 0 

    contadorConsonantes = 0 

     

    for letra in palabra.lower(): 

        if letra.isalpha(): 

            if letra == "a" or letra == "e" or letra == "i" or letra == "o" or letra == "u": 

                contadorVocales += 1 

            else: 

                contadorConsonantes += 1 

     

    print(contadorVocales) 

    print(contadorConsonantes) 

     

contar_letras("Prueba") 
```




## 2. Invertir una cadena
```python
def invertir_palabra(palabra): 

    print(palabra[::-1]) 

invertir_palabra("Hola") 
```



## 3. Verificar palíndromo
```python
def palindromo(palabra): 

    return palabra == palabra[::-1] 

  

print(palindromo("ala")) 
```



## 4. Contar palabras
```python
def contar_palabras(frase): 

    print(len(frase.split(" "))) 

     

contar_palabras("Hola mundo cruel") 
```




## 5. Eliminar caracteres repetidos
```python
def eliminar_duplicados(palabra): 

    palabraFinal = "" 

    contador = 0 

    for caracter in palabra: 

        if palabra.find(caracter) == contador: 

            palabraFinal += caracter 

        contador += 1 

         

    print(palabraFinal) 

     

eliminar_duplicados("HH  oo  ll  aa") 
```




## 6. Mayúsculas y minúsculas
```python
def invertir_mayusculas(palabra): 

    palabraInvertida = "" 

    for caracter in palabra: 

        if caracter.islower(): 

            palabraInvertida += caracter.upper() 

        else: 

            palabraInvertida += caracter.lower() 

     

    print(palabraInvertida) 

  

invertir_mayusculas("HoLa") 
```



## 7. Invertir palabras de una cadena
```python
def invertir_frase(frase): 

    print(" ".join(frase.split(" ")[::-1])) 

     

invertir_frase("Hola mundo cruel") 
```



## 8. Anagrama
```python
def comprobar_anagrama(palabraUno, palabraDos): 

    return sorted(palabraUno) == sorted(palabraDos) 

     

print(comprobar_anagrama("Hola","Hloa")) 
```



## 9. Frecuencia de caracteres
```python
def frecuencia_caracteres(palabra): 

    diccionario = {} 

    for caracter in palabra: 

        if caracter not in diccionario: 

            diccionario[caracter] = 0 

        diccionario[caracter] += 1 

         

    print(diccionario) 

     

frecuencia_caracteres("Hola mundo") 
```



## 10. Quitar caracteres alfanuméricos
```python
def mantener_alfanumericos(palabra): 

    palabra_filtrada = "" 

    for caracter in palabra: 

        if caracter.isalpha(): 

            palabra_filtrada += caracter 

    print(palabra_filtrada) 

     

mantener_alfanumericos("!!!!Hola!????") 
```



## 11. Transformar a camelCase
```python
def transformar_camelcase(palabra): 

    palabra = palabra.replace("-", " ") 

    listaPalabras = palabra.split(" ") 

    listaFinal = [] 

    for palabra in listaPalabras: 

        listaFinal += palabra.capitalize() 

    listaFinal[0] = listaFinal[0].lower() 

    return "".join(listaFinal) 

  

print(transformar_camelcase("Esto es-una-lista separada")) 
```



## 12. Codificacion RLE
```python
def codificar_RLE(cadena): 

    ultimoCaracterEncontrado = "" 

    numeroVecesEncontrado = 0 

    cadenaFinal = "" 

     

    for caracter in cadena: 

        if ultimoCaracterEncontrado == "" or ultimoCaracterEncontrado == caracter: 

            ultimoCaracterEncontrado = caracter 

            numeroVecesEncontrado += 1 

        else: 

            cadenaFinal += ultimoCaracterEncontrado + str(numeroVecesEncontrado) 

            ultimoCaracterEncontrado = caracter 

            numeroVecesEncontrado = 1 

     

    if(numeroVecesEncontrado > 0): 

        cadenaFinal += ultimoCaracterEncontrado + str(numeroVecesEncontrado) 

     

    return cadenaFinal 

     

print(codificar_RLE("hhhh  ooooo l aa")) 
```


---
[Volver a la Unidad 4](../)
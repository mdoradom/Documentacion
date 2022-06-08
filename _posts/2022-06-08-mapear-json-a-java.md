---
title: Mapear JSON a Java
author: mario
date: 2022-06-08 13:12:00 +0800
categories: [Java, json]
tags: [java, json]
math: true
mermaid: true
---

Mapear JSON a Java
==================

El formato de datos [JSON](https://www.json.org) se basa en dos estructuras:

*   **Objetos**: son una colección de pares clave/valor, y van entre llaves ``{ }``
    
*   **Arrays**: son una lista de valores, y van entre corchetes ``[ ]``

### Objetos

Para mapear un objeto JSON a Java, se debe definir una clase que contenga los campos equivalentes a las claves del objeto JSON, y el tipo de estos campos debe ser del mismo tipo que los valores del objeto JSON.

El nombre de la clase puede ser cualquiera (**a veces es difícil pensar qué nombre poner**), pero el nombre de los campos debe coincidir con las claves del objeto JSON.

```json
{
    "nombre": "Juan",
    "edad": 18,
    "nota": 7.75,
    "matriculado": true
}
```

```java
class Estudiante {
    String nombre;
    int edad;
    float nota;
    boolean matriculado;
}
```

También es posible que el valor de una clave JSON sea otro objeto. En este caso, se deberán crear una clase por cada tipo de objeto:

```json
{
    "nombre": "Juan",
    "ciclo" : {
        "titulo": "DAM",
        "curso": 2
    }
}
``` 

```java
class Estudiante {
    String nombre;
    CicloFormativo ciclo;
}

class CicloFormativo {
    String titulo;
    int curso;
}
```


### Arrays

Para mapear listas JSON a Java, se debe crear una List con el tipo adecuado a los elementos de la lista JSON:

```json
[ "SMX", "DAM", "ASIX", "DAW" ]
```

```java
List<String>
```

Con otro tipo de datos es igual:

```json
[ 4, 13, 22, 35 ]
```

```java
List<Integer>
```

El valor de una clave de un objeto JSON, puede ser una lista:

```json
{
    "notas": [ 6.5, 7.75, 4.1, 9.95 ]
}
```

```java
class Boletin {
    List<Integer> notas;
}
```

Los elementos de una lista pueden ser objetos (**todos tienen que ser de la misma clase**):

```json
{
    "estudiantes": [
        { "nombre": "juan" },
        { "nombre": "luis" },
        { "nombre": "paco" }
    ]
}
```

```java
class Curso {
    List<Estudiante> estudiantes;
}

class Estudiante {
    String nombre;
}
```
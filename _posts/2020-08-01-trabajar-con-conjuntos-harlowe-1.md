---
layout: post
title: "Twine Harlowe: La maravilla de los Arrays (1era Parte)"
---
Muchas veces, cuantro trabajamos con datos, nos damos cuenta que algunos de estos se relacionan de alguna forma entre sí. Si para cada dato creamos una variable, a lo largo del código puede ser tedioso lidiar con ellos. Pero, ¿y si agrupamos los datos que tienen cosas en común, y trabajamos directamente sobre esos conjuntos? Eso, mis queridos amigos, son los arrays.

Imaginemos que tenemos lo siguiente, cada vocal en una variable y al final las imprimimos todas en pantalla:

```
(set: $vocal1 to "A")
(set: $vocal2 to "E")
(set: $vocal3 to "I")
(set: $vocal4 to "O")
(set: $vocal5 to "U")

Las vocales son: $vocal1, $vocal2, $vocal3, $vocal4, $vocal5.

```

Este mismo ejemplo expresado en un conjunto sería:

```
(set: $vocales to (a: "A", "E", "I", "O", "U"))

Las vocales son: $vocales.

```

Así es. Para crear un conjunto usamos **(a:)** dentro de **(set:)**. Y para imprimir los valores que contiene separados por comas bastará con poner directamente el nombre de la variable en el texto. Pero, ¿qué tal si solo queremos imprimir un valor específico? Digamos que solo queremos imprimir "I".

```
La tercera vocal es: (print: $vocales's 3).

```

Tenemos que auxiliarnos de la macro **(print:)**, seguido del nombre de la variable, más **'s**, más la posición del valor dentro del array.

*NOTA:* Para los que tienen conocimiento de programación es muy importante este detalle: **el índice de los arrays empieza en 1, no en 0**.

Si queremos ver cuántos elementos tiene nuestro array:

```
La cantidad de vocales son: (print: $vocales's length).

```


Si queremos modificar un valor del array:

```
Las vocales son: $vocales.

(set: $vocales's 3 to "B")

Las vocales son: $vocales.
<!--Aparecerá la "B" en vez de "I"-->

```


Si queremos añadir un nuevo elemento al array:

```
Las vocales son: $vocales.

(set: $vocales to it + (a:"K"))

Las vocales son: $vocales.
<!--Aparecerán las 5 vocales y la letra "K"-->

```

Por si no se han dado cuenta, **it** representa lo mismo que $vocales, es lo mismo que escribir:

```
(set: $vocales to $vocales + (a:"K"))
```

Para sumar un nuevo valor a un array, este debe estar dentro de un array, es super importante tomarlo en cuenta, por eso K está encerrado en __(a:)__.

Ya para cerrar esta introducción básica a los arrays, si queremos saber si un array contiene cierto valor, podemos hacerlo así:

```
(if: $vocales contains "K")[
	Entre las vocales hay una K.
]
```

En el próximo post veremos cómo usar todo esto en la práctica y como tratar con arrays multidimensionales.
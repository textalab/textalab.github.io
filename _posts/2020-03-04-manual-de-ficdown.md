---
layout: post
title: "Manual de Ficdown en Español"
thumbnail: https://i.ibb.co/XWf9xKw/ficdown.png
---

### Tabla de Contenidos

- [**Preliminar**](#preliminar)
	+ [¿Qué es Ficdown?](#qué-es-ficdown)
	+ [¿Por qué Ficdown?](#por-qué-ficdown)
- [**Sintaxis**](#sintaxis)
	+ [Historia](#historia)
	+ [Escenas](#escenas)
	+ [Escenas condicionales](#escenas-condicionales)
	+ [First-Seen](#first-seen)
	+ [Acciones](#acciones)
	+ [Anclas](#anclas)
	+ [Objetivos](#objetivos)
	+ [Palancas](#palancas)
	+ [Condicionales](#condicionales)
	+ [Normalización](#normalización)
	+ [Estado del Jugador](#estado-del-jugador)

**ANTES DE LEER:** *En el texto que sigue se le dice **ancla** al texto encerrado entre corchetes (`[ancla]`) y **enlace** al texto encerrado entre paréntesis que sigue al ancla(`[ancla](enlace)`), también se le llama **ancla** a ambas cosas en conjunto. Cuando se habla de **descripción**, se refiere al contenido tanto de las escenas como de las acciones y el texto introductorio de la historia.*

# Preliminar

## ¿Qué es Ficdown?

[Ficdown](https://ficdown.com/) es un conjunto de estándares que utiliza la sintaxis del formato Markdown para crear ficción interactiva basada en elecciones . El formateo del texto se realiza utilizando Markdown estándar, con algunos elementos que adquieren propiedades adicionales (por ejemplo, se utilizan encabezados de nivel dos `##` para separar las escenas).

## ¿Por qué Ficdown?

Existen muchos sistemas para crear ficción interactiva basada en elecciones. Muchos de esos sistemas implican aprender un nuevo lenguaje de secuencias de comandos y requieren el uso de entornos de desarrollo especializados (ya sea en línea o instalados en su computadora). La mayoría de ellos producen juegos que requieren Javascript y sólo se pueden jugar en un navegador web.

El autor de Ficdown, [Rudis Muiznieks](https://rudism.com/), lo creó por tres razones:

* Poder escribir ficción interactiva en el editor de texto de su elección.
* Ejecutar ficción interactiva en dispositivos tan limitados como los ereaders, que no cuentan con soporte para Javascript o una conexión a Internet.
* Usar Markdown en lugar de aprender un nuevo lenguaje de secuencias de comandos.

Usando Ficdown, puedes concentrarte en la escritura. Ficdown solo usa la sintaxis Markdown existente para las definiciones del juego, para que puedas escribir en el editor que desees y no tengas que aprender ningún nuevo lenguaje de secuencias de comandos. Y aunque las historias de Ficdown se pueden reproducir de forma interactiva en un navegador web, también se pueden convertir a formato estándar de libros electrónicos o HTML estático para jugar sin conexión.

# Syntaxis

## Historia

Un bloque **Historia** se define mediante un encabezado de nivel uno (`#`). El título de la historia debe ser un [ancla](#anclas) que se vincule con la primera [escena](#escenas). El ancla no debe contener [condicionales](#condicionales) ni [conmutadores](#palancas).

El texto del bloque de Historia será lo primero que se mostrará al jugador y no debe contener ningún ancla en la descripción.

`# [Un asombroso juego](/primera-escena)`

`Bienvenido a un asombroso juego por *Algún Autor*.`

## Escenas

Las **escenas** están definidas por encabezados de nivel dos (`##`). Las descripciones de las escenas pueden contener bloques [first-seen](#first-seen) y [anclas](#anclas) para cambiar la descripción según el [estado del jugador](#estado-del-jugador) o algún enlace que apunte a otra escena.

Para mostrar un título distinto al nombre de la escena, puede envolver el nombre de la escena en un [ancla](#anclas) y especificar el texto que se mostrará en pantalla como el título del ancla.

```Markdown
## Primera escena

Este es el texto que se muestra al jugador cuando visita esta escena.

## [Segunda escena]("Título de la segunda escena")

Esta es la segunda escena con un título de visualización diferente al especificado.
```

## Escenas condicionales

Los nombres de las escenas pueden ser anclas que contengan condicionales que evalúan el [estado del jugador](#estado-del-jugador) las cuales deben cumplirse para ingresar a esa [escena](#escenas). El ancla no debe contener un [objetivo](#objetivos) ni un [conmutador](#palancas). Si se definen varias escenas con el mismo nombre, el jugador solo verá la escena para la cual su estado actual satisfaga todos las [condicionales](#condicionales) especificadas. Si el [estado del jugador](#estado-del-jugador) satisface las condicionales de más de una escena, solo verá el que tenga más variables activadas. Si dos escenas están satisfechas con el mismo número de variables activadas, el jugador verá la que se definió primero en el archivo de la historia.

Si solo se definen [escenas condicionales](#escenas-condicionales) y ninguna de ellas las satisface el [estado del jugador](#estado-del-jugador), esto se considera un error en el archivo de la historia. La forma más segura de evitar esto es tener siempre una versión no condicional de la escena a la que recurrir cuando ninguna de las versiones condicionales se satisfaga.

`## Mi escena`

`Esta es la escena predeterminada que el jugador verá si ninguna de las versiones condicionales se cumple.`

`## [Mi escena](?estado-uno)`

`El jugador verá esto si su variable "estado-uno" se ha activado, pero no si se ha activado "estado-dos" porque coincide con una escena condicional más específica a continuación.`

`## [Mi escena](?estado-uno&estado-dos "Título de escena alternativo")`

`El jugador verá esto si tanto el "estado-uno" como el "estado-dos" se han activado.`

## First Seen

**First Seen** o **visto por primera vez**. El texto descriptivo precedido por blockquote (`>`) solo se mostrará al jugador la primera vez que ingrese a la escena.

`## Mi escena`

`> Este texto solo se mostrará al jugador una vez.`

`Este texto se mostrará al jugador cada vez que ingrese a la escena.`

`> Este texto solo se mostrará al jugador una vez.`

## Acciones

Las **acciones** se definen mediante encabezados de nivel tres. Cuando se [alterna](#palancas) una [variable de estado](#estado-del-jugador) cuyo nombre coincide con una acción, la descripción de la acción se antepondrá a la descripción de la siguiente escena que ve el jugador. Los nombres de las acciones no deben ser [anclas](#anclas). Las anclas solo se pueden usar dentro de la descripción de la acción para cambiar el texto según el [estado del jugador](#estado-del-jugador), es decir, sólo se aceptan anclas con [condicionales](#condicionales).

Las acciones se corresponden con las variables de estado en función de su nombre [normalizado](#normalización).

Las acciones serán activadas al [conmutar](#palancas) la variable asociada, incluso si esa variable se ha activado anteriormente.

La primera vez que se activa una acción, la variable activada será falsa dentro de la acción misma. En activaciones posteriores, la variable será verdadera. Esto se puede usar para diferenciar la primera vez que se muestra una acción.

**NOTA:** *Si se conmutan múltiples variables al mismo tiempo asociadas a distintas acciones, las descripciones de cada una de las acciones se presentarán en el orden en que aparezcan en el archivo de la historia.*

`### Mi estado`

`Este texto se mostrará al jugador cada vez que active "mi-estado". Esta [no](?mi-estado) es la primera vez que se activó esta la acción.`

## Anclas

Las **anclas** se utilizan dentro de las descripciones de las [escenas](#escenas) para vincular a los jugadores con otras escenas de la historia y para cambiar el texto de la descripción según el [estado del jugador](#estado-del-jugador).

Las anclas con un [objetivo](#objetivos) o [conmutador](#palancas) se mostrarán al jugador como un enlace en el que se puede hacer clic. Las anclas que solo contienen [condicionales](#condicionales) se representarán como texto normal.

Las anclas pueden contener cualquier cantidad de [condicionales](#condicionales) y [conmutadores](#palancas), pero pueden contener como máximo un [objetivo](#objetivos).

El texto dentro del ancla no puede contener otras anclas. Para una modificación más compleja de las descripciones de escenas, en su lugar debe usar [escenas condicionales](#escenas-condicionales).

`[Texto a mostrar cuando se cumplen las condiciones|Texto a mostrar cuando no se cumplen las condiciones](/escena-objetivo?primera-condicional&segunda-condicional#palanca-uno+palanca-dos)`

## Objetivos

El **objetivo** dentro de un [ancla](#anclas) siempre debe ir al principio del enlace y estar precedido por un carácter de barra inclinada "/". El objetivo debe ser el nombre [normalizado](#normalización) de la [escena](#escenas) a la que se vincula. Las [anclas](#anclas) con un objetivo se mostrarán como un enlace en el que se puede hacer clic para el jugador. Cuando se hace clic en el enlace, se moverá a la [escena](#escenas) especificada por el objetivo.

Las anclas pueden tener como máximo un objetivo.

`[Ir a la escena dos](/escena-dos)`

## Palancas

Las **palancas** o **conmutadores** se utilizan en las [anclas](#anclas) para establecer las variables de [estado del jugador](#estado-del-jugador) en verdadero y para desencadenar [acciones](#acciones). Están precedidos en el enlace de un [ancla](#anclas) con un hash "#". Se pueden añadir múltiples conmutadores con un carácter más "+". Siempre deben añadirse al final del enlace, después del objetivo y las condicionales.

Las anclas con conmutadores siempre se mostrarán como enlaces en los que se puede hacer clic. Si el ancla no tiene un objetivo, al hacer clic en el enlace cambiará el estado de las variables y luego redirigirá al jugador a la escena actual.

`[Conmutar mis variables](#palanca-uno+palanca-dos)`

**NOTA:** *Lo dicho en el último párrafo corresponde si se quiere exportar a EPUB o HTML ESTÁTICO, pero para la versión WEB DINÁMICA, por lo menos en Ficdownjs 0.9.1, al añadir un ancla que solo contenga conmutadores se debe especificar también como mínimo la escena actual como objetivo.*

## Condicionales

Las **condicionales** se especifican en el enlace del ancla, entre el [objetivo](#objetivos) y los [conmutadores](#palancas). Están precedidos por un signo de interrogación "?" y delimitados por símbolos "&". Se utilizan para ocultar o cambiar el texto mostrado en función del [estado actual del jugador](#estado-del-jugador). Las condicionales se especifican con el nombre de la variable que se desea evaluar como verdadera, o el nombre de la variable precedido por un signo de exclamación "!" para evaluar si el valor es falso.

Si el texto dentro del ancla contiene un carácter de barra "\|" entonces el texto a la izquierda se mostrará si el estado actual del jugador cumple con todas las condicionales, y el texto a la derecha se mostrará si el estado actual del jugador no cumple con todas las condicionales. El texto del ancla que no contenga ninguna barra se ocultará por completo si el estado del jugador no cumple con las condicionales, y el texto del ancla sin nada a la izquierda de la tubería se ocultará si el estado del jugador cumple con los condicionales.

Las [anclas](#anclas) con solo [condicionales](#condicionales) y sin [objetivo](#objetivos) ni [conmutadores](#palancas) **no** se mostrarán como un enlace en el que se pueda hacer clic. Se pueden usar para cambiar fácilmente la descripción de una escena según el [estado del jugador](#estado-del-jugador).

`## Mi escena`

`La última palabra de este párrafo cambiará según el estado del jugador. La variable "mi-estado" es [verdadera|falsa] (?mi-estado).`

`Este es un texto que cambiará según el estado de varias variables: [Todo es verdadero|No todo es verdadero](?mi-estado&mi-estado-dos&mi-estado-tres).`

`Este es un texto que cambiará según reglas más complejas que verifican tanto los valores verdaderos como los falsos: [Satisfecho|No satisfecho](?mi-estado&!mi-estado-dos&!mi-estado-tres).`

`Los enlaces al final de esta descripción también cambiarán según el estado del jugador.`

`[Esta elección siempre aparecerá](/primera-eleccion)`

`[Esta elección solo aparece si mi-estado es verdadero](/segunda-eleccion?mi-estado)`

`[Esta elección solo aparece si otro estado es falso](/tercera-eleccion?!otro-estado)`

`[El texto de esta elección cambia según el valor de mi estado|Pero siempre aparecerá](/cuarta-eleccion?mi-estado)`

## Normalización

Los nombres de [escena](#escenas) y [acción](#acciones) se normalizan eliminando todos los caracteres no alfanuméricos, convirtiendo todo a minúsculas y reemplazando los espacios con guiones. Algunos ejemplos:

* "Escena" se convertiría en "escena"
* "Mi escena" se convertiría en "mi-escena"
* "Big Bob's House" se convertiría en "big-bobs-house"
* "Apartamento/Habitación 123" se convertiría en "apartamentohabitacin-123"

La **normalización** se aplica a los nombres de las escenas cuando se usan como objetivo en las anclas, y en los nombres de las acciones cuando se activan mediante conmutadores.

## Estado del jugador

El **estado del jugador** es una colección de variables booleanas que se asignan al jugador y se rastrean a lo largo del juego. Todas las variables de estado del jugador comienzan en falso. Las variables se establecen en verdadero cuando un jugador hace clic en un enlace que contiene esa variable como una [palanca](#palancas). Una vez activada, la variable de estado será verdadera durante el resto del juego (no se puede restablecer a falso).

No es necesario declarar variables previamente en ninguna parte de su historia. Solo debe ponerlas donde las vaya a usar en las anclas de su historia.

Las variables se utilizan en anclas condicionales para cambiar el texto que se muestra u ocultarlo por completo.

Las anclas condicionales envueltas alrededor de los nombres de las escenas se usan para definir [escenas condicionales](#escenas-condicionales).

**NOTA:** *Al compilar la historia en EPUB, puede pasar el indicador DEBUG que mostrará la información del estado del jugador en la parte inferior de cada página de su historia. Esto puede ser útil para rastrear enlaces o historias que no se comportan de la manera que se esperaba.*

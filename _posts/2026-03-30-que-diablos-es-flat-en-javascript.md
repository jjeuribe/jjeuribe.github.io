---
title: Qué diablos es flat en Javascript
image:
  path: /assets/2026-03-30-que-diablos-es-flat-en-javascript/0_KYnl1wsBy1stIdzY.webp
  alt: Photo by Ray Shrewsberry on Unsplash
date: 2026-03-30 05:00:00 -0600
categories: [Programming Languages, JavaScript]
tags: [javascript, javascript-fundamentals, functional-programming]     # TAG names should always be lowercase
---

En el mundo de la programación funcional existe un principio llamado **data flattening** cuyo significado vendría a ser algo como _aplanar_ o _aplastar_ una estructura de información.

Y aquí es donde tal vez te preguntes, ¿cómo que aplanar/aplastar información? 🧐, ¿a qué te refieres?, no es como si tuviéramos una lata llena de información y brincásemos sobre ella para aplastarla.

Bueno no precisamente, al igual que tú esto tampoco tenía sentido para mí, al menos al inicio.

Verás, este principio se encuentra muy relacionado al mundo de los arreglos, y es aquí cuando de alguna manera todo comienza a tener sentido.

Como bien sabemos un arreglo es una estructura de información que puede poseer varias dimensiones:

![Desktop View](/assets/2026-03-30-que-diablos-es-flat-en-javascript/Pasted_image_20241003171021.png)

Pues bien, bajo este principio nuestro trabajo es tomar alguna de estas estructuras con múltiples dimensiones y pasarla digamos por una “máquina aplanadora” para obtener una estructura uniforme de 1 sola dimensión.

Mmmmm algo así:

![Desktop View](/assets/2026-03-30-que-diablos-es-flat-en-javascript/Pasted_image_20241003171136.png)


Nuestra _"máquina aplanadora"_ es simplemente un método o función que se encarga de realizar el trabajo de _"aplanar"_ la estructura multidimensional que recibe como entrada.

En este sentido, ¿no sería conveniente tener algo como esto?:

![Desktop View](/assets/2026-03-30-que-diablos-es-flat-en-javascript/Pasted_image_20241003171555.png)

Apuesto a que así.

Vale, vale, y al final ¿para qué me serviría tener algo así? bueno, así como toda superficie irregular requiere cierto nivel de uniformidad para construir sobre ella, nuestra información requiere de cierta homogeneidad en su estructura para ser fácilmente manipulable o digerible.


Regresando al tema, si bien Javascript no posee un método `Array.flatten`, si posee un método `Array.flat`, introducido en las versiones modernas del lenguaje:

![Desktop View](/assets/2026-03-30-que-diablos-es-flat-en-javascript/Pasted_image_20241003171719.png)

Por defecto este método reduce un arreglo multidimensional en 1 nivel, sin embargo, podemos indicar el nivel al que queremos reducir una estructura multidimensional.

Por ejemplo, si nuestra intención es _"aplanar"_ un arreglo de 3 dimensiones para obtener un arreglo uniforme de 1 sola dimensión, simplemente debemos reducirlo en 2 niveles:

![Desktop View](/assets/2026-03-30-que-diablos-es-flat-en-javascript/Pasted_image_20241003171849.png)

Para obtener un arreglo de 1 dimensión a partir de un arreglo de 4 dimensiones tendríamos que reducirlo en 3 niveles:

![Desktop View](/assets/2026-03-30-que-diablos-es-flat-en-javascript/Pasted_image_20241003171926.png)

Y así sucesivamente. Solo tenemos que indicárselo al método `Array.flat`.

Además, te cuento que `Array.flat` va un poquito más allá, y es que este método puede trabajar con arreglos multidimensionales que posean una estructura heterogénea, ejemplo:

![Desktop View](/assets/2026-03-30-que-diablos-es-flat-en-javascript/Pasted_image_20241003172050.png)

¿Por qué le he indicado a `Array.flat` que lo reduzca en 2 niveles?.

Simplemente porque quiero obtener un arreglo de 1 sola dimensión con todos sus elementos, y para ello he tomado como referencia al elemento que se encuentra en la dimensión o nivel de anidación más profunda:

![Desktop View](/assets/2026-03-30-que-diablos-es-flat-en-javascript/image1.png)

Básicamente este elemento se encuentra físicamente ubicado en la 3ra dimensión de la estructura, a partir de aquí `Array.flat` comienza con su proceso de _"aplanado"_. El nivel de los demás elementos será respetado o reducido acorde.

## Eliminado agujeros.

Además de _"aplanar"_, nuestra pequeña maquinita también tiene la capacidad de eliminar cualquier _"agujero"_ o desperfecto de la superficie.

Por ejemplo:

![Desktop View](/assets/2026-03-30-que-diablos-es-flat-en-javascript/Pasted_image_20241003172521.png)

Y bien, esto es básicamente `Array.flat`. Así que si en algún punto necesitas que tu estructura tenga un cierto nivel de uniformidad ten en mente esta genial herramienta.


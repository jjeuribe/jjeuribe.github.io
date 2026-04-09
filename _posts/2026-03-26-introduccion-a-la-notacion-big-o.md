---
title: Introducción a la Notación Big O
image:
  path: /assets/2026-03-26-introduccion-a-la-notacion-big-o/1_YkimPxCWXrS3JZ0AtNkHGg.webp
  alt: Una dona deliciosa
date: 2026-03-26 14:00:00 -0600
categories: [Computer Science, Algoritmos]
tags: [algoritmos, big-o-notation, big-o, ]     # TAG names should always be lowercase
---

¿Cómo evaluar el desempeño de un algoritmo?, ¿Qué queremos decir cuando el desempeño de un algoritmo es mejor que otro?, ¿Qué es lo que buscamos de la implementación de nuestro algoritmo?. Al desarrollar un algoritmo surgen estas interrogantes y la ambigüedad se hace presente.

* Sea que busquemos rapidez en el **tiempo de ejecución**, ya sea en el orden de segundos e incluso en milisegundos.
* Minimizar el **uso intensivo de la memoria**, definiendo menos variables y con nombres más compactos.
* Maximizar la **legibilidad** de nuestro código para hacerlo más mantenible.

Cualquier decisión que tomemos siempre tendrá un _tradeoff_ o _costo de oportunidad_, sin embargo, lo importante es saber realmente lo qué queremos o buscamos de nuestro algoritmo.

¿Deberíamos escribir código más compacto con el riesgo de que sea menos legible y por lo tanto más _difícil de mantener_? o por el contrario, ¿Deberíamos escribir código más _claro y legible_ con el potencial de que ocupe mas espacio en memoria?.


Supongamos que nuestra intención es comparar la _velocidad_ de ejecución entre estas 2 funciones:


Implementación de `addUpToFirst()`:

```js
function addUpToFirst(n)
{
  let total = 0;
	
  for( i = 1; i <= n; i++) {
		
    total += i;
  }

  return total;
}
```

Implementación de `addUpToSecond()`:

```js
function addUpToSecond(n)
{
  return n * (n + 1) / 2;
}
```

Procedamos a evaluar el desempeño de ejecución de cada uno y comparemos.

Resultados de `addUpToFirst()`:

```js
let t1 = performance.now();
addUpToFirst(1000000000);
let t2 = performance.now();

console.log("Tiempo transcurrido: ${(t2 - t1) / 1000} segundos");
```

* Tiempo Transcurrido: **1.25750000000006985 segundos**
* Tiempo Transcurrido: **1.24369999999936985 segundos**

Resultados de `addUpToSecond()`:


```js
let t1 = performance.now();
addUpToSecond(1000000000);
let t2 = performance.now();

console.log("Tiempo transcurrido: ${(t2 - t1) / 1000} segundos");
```

* Tiempo Transcurrido: **0.00010000000004773 segundos**
* Tiempo Transcurrido: **0.00010000000004773 segundos**

Evidentemente la segunda versión es muchísimo mas rápida en comparación con la primera versión en términos de velocidad de ejecución.

Sin embargo, esta evaluación de alguna manera sigue siendo ambigua, y esto se debe principalmente a la herramienta que hemos utilizado para evaluar el desempeño en el tiempo de ejecución del algoritmo en cuestión: un **Temporizador**.

<!-- ad -->
{% include adsense-post-inline.html %}
<!-- /ad -->

Entonces, ¿Me estás diciendo que un _temporizador_ es un mecanismo poco confiable al evaluar el desempeño del tiempo de ejecución de un algoritmo?, en definitiva, y esto se debe principalmente al hardware:

* Diferentes máquinas registran diferentes tiempos dadas las especificaciones de hardware de cada máquina, lo que ocasiona que los resultados puedan variar.
* Una misma máquina puede registrar diferentes tiempos cada vez que ejecutamos nuestra rutina. La variación puede ser poco significativa, pero aún así puede existir una falta de precisión.
* La métrica de velocidad en el _tiempo de ejecución_ puede no ser un indicador preciso y confiable en algoritmos que son muy veloces. Por ejemplo, podemos tener 4 algoritmos superveloces, tan rápidos que los temporizadores no lleguen a registrar los _tiempos de ejecución_ de cada uno de forma precisa.

En lugar de intentar contabilizar el _número de segundos_ (el cual es variable) que tarda una rutina en finalizar su ejecución, es mucho mejor concentrarse en contabilizar el _número de operaciones_ que nuestra implementación debe realizar para llegar a un resultado.

El _tiempo de ejecución_ de nuestro algoritmo depende del _número de operaciones_ que realice, y este número es constante sin importar la máquina o hardware que utilicemos.

Tomemos como ejemplo la implementación de la función `addUpToFirst()`:

![Desktop View](/assets/2026-03-26-introduccion-a-la-notacion-big-o/1_FKZtRfS1aJS9LrQpnv2Z0Q.webp)


y comparemosla con la función `addUpToSecond()`:

![Desktop View](/assets/2026-03-26-introduccion-a-la-notacion-big-o/1_8dJFdSpMSvhrXk-B3OXunQ.webp)

¿Qué podemos concluir de esta comparativa?. La función `addUpToFirst()` ejecuta un número de operaciones que es proporcional al valor asignado a `n`, es decir, a medida que `n` aumenta, el número de operaciones que realiza este algoritmo crecerá en la misma proporción.

En contraste, la función `addUpToSecond()` lleva a cabo solamente 3 operaciones independientemente del valor que le asignemos a `n`, por lo que si `n` crece, el número de operaciones que realiza nuestro algoritmo se mantiene siempre igual.

En la práctica llevar un conteo de operaciones ejecutadas por un algoritmo puede parecer un enfoque poco intuitivo, sin embargo, existe una manera de llevar a cabo esto.

Debemos entender qué, lo importante no es llevar el conteo preciso y exacto en el número de operaciones ejecutadas, lo que realmente importa es observar el comportamiento general del número de operaciones ejecutadas relativas la variabilidad de `n`.


Esto básicamente significa que todo algoritmo posee un tendencia que puede ser plasmada en una gráfica, y es a través de esta gráfica que nos damos una idea general del desempeño de nuestro algoritmo, en este caso, relativa al tiempo o velocidad de ejecución.


* Tendencia general de `addUpToFirst()`: a medida que `n` crece, el tiempo de ejecución crece de forma proporcional o lineal.

![Desktop View](/assets/2026-03-26-introduccion-a-la-notacion-big-o/1_WCwSoXszJxt2WiBUmeya-Q.webp)


* Tendencia general de `addUpToSecond()`: a medida que `n` crece, el tiempo de ejecución permanece casi constante.

![Desktop View](/assets/2026-03-26-introduccion-a-la-notacion-big-o/1_pn_8lIlBBC0FnE9vEsVBWA.webp)

En esta breve introducción tenemos los fundamentos necesarios para comenzar a entender lo que es la notación Big O y su significado. En el siguiente artículo nos adentraremos en mayor profundidad en el tema, espero les haya gustado. Saludos y Happy Coding.

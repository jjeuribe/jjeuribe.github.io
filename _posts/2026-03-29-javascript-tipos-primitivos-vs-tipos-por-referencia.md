---
title: Javascript - Tipos Primitivos vs Tipos por Referencia
image:
  path: /assets/2026-03-29-javascript-tipos-primitivos-vs-tipos-por-referencia/1_UOSlVOFcmD7okV-45PQhPA.webp
date: 2026-03-29 14:00:00 -0600
categories: [Programming Languages, JavaScript]
tags: [javascript, javascript-fundamentals]     # TAG names should always be lowercase
---

Al hablar sobre manejo de memoria, existen 2 tipos de datos con los cuales vamos a estar trabajando la mayoría del tiempo en Javascript: **Tipos Primitivos** y **Tipos por Referencia**.

Los Tipos Primitivos son los bien conocidos: *Number*, *String*, *Boolean*, *Null* y *Undefined*, además de los nuevos integrantes recién llegados en las nuevas versiones de Javascript: *Symbol* y *BigInt*.

Los Tipos por Referencia básicamente son todo aquello que no es un Tipo Primitivo, ejemplo: *Funciones*, *Objetos*, *Arreglos*, etc.

Para entender mejor la diferencia entre ellos, analicemos su comportamiento desde el punto de vista del motor de Javascript.

![Desktop View](/assets/2026-03-29-javascript-tipos-primitivos-vs-tipos-por-referencia/1_59olKtfs8MoMau9ty8tMAA.webp)

De todo esto, lo único que debemos entender es que un valor de *Tipo Primitivo* es almacenado en el **Call Stack**, en específico, dentro el contexto de ejecución (función) donde es definido, mientras que un *Tipo por Referencia* es almacenado en el **Heap**.

## Tipos Primitivos

Al momento de declarar una variable y asignarle un valor de *tipo primitivo*, básicamente ocurre lo siguiente:

![Desktop View](/assets/2026-03-29-javascript-tipos-primitivos-vs-tipos-por-referencia/1__CefPg7q2qDT4qHMTCLzUA.webp)

1. Se crea un *identificador* con el nombre que se le asignó a la variable.
2. Se prepara una porción de memoria para almacenar el valor primitivo.
3. El valor es almacenado en memoria.

Es de notar que, el *identificador de la variable* apunta a la dirección de memoria donde se encuentra el valor, y no al valor en sí mismo.

Vamos a crear una segunda variable con el valor de la primera:

![Desktop View](/assets/2026-03-29-javascript-tipos-primitivos-vs-tipos-por-referencia/1_sidPJ010lNlCmWbeSSmDtw.webp)

En este caso, la variable `oldPreviousAge` apunta a la misma dirección de memoria que la variable `currentAge`, en este sentido, ambas variables hacen referencia al valor de tipo entero `25` a través de la dirección `0x001`.

Pero, ¿Y si modificamos el valor de la variable `currentAge`?, ¿Esto tendrá impacto en la variable `oldPreviousAge`?, veamos.

![Desktop View](/assets/2026-03-29-javascript-tipos-primitivos-vs-tipos-por-referencia/1_WaRsG2Enn5MBYC3pU3dteg.webp)

En primer lugar, tenemos que entender que un valor residente en una determinada dirección de memoria es **inmutable**, es decir, no la podemos cambiar una vez ocupada, por lo tanto, una nueva porción de memoria será creada y asignada para apuntar al nuevo valor.


Podemos afirmar que la modificación de `currentAge` no tiene impacto alguno en la variable `oldPreviousAge`, ya que ambas variables apuntan a direcciones de memoria diferentes.

## Tipos por Referencia

En este caso, las cosas funcionan un poco diferente. Cuando un objeto es creado, este es almacenado en el **Heap** del Motor de Javascript.

De igual manera, dentro del **Heap** habrá una dirección de memoria asignada apuntando al objeto en cuestión, sin embargo, la variable en cuestión no va a apuntar directamente a esta dirección de memoria, sino que lo hará a través de una nueva dirección de memoria ubicada en el **Call Stack**.

Para entender mejor esto, retomemos nuestro ejemplo anterior. Vamos a definir la variable `profile` y le vamos a asignar como valor un objeto literal.

![Desktop View](/assets/2026-03-29-javascript-tipos-primitivos-vs-tipos-por-referencia/1_WQbeW5jtVrraH_FiMTnjLQ.webp)

Como se puede apreciar, desde el **Call Stack** se hace referencia hacia el **Heap** para acceder a los objetos almacenados en memoria. El **Heap** es necesario, ya que los objetos pueden llegar a ser muy grandes, y por lo tanto, ocupar gran espacio en memoria.

Vamos a crear una nueva variable con el valor de la variable `profile`.

![Desktop View](/assets/2026-03-29-javascript-tipos-primitivos-vs-tipos-por-referencia/1_AS7nvR5fBbyKviUDo9uGfA.webp)

Ahora tanto la variable `account` como la variable `profile` apuntan a la misma dirección de memoria dentro del **Call Stack**, que a su vez, hace referencia a la dirección de memoria dentro del **Heap** donde se encuentra almacenado el objeto de nuestro interés.

En este caso podemos decir que tanto los *tipos primitivos* como los *tipos por referencia* comparten un comportamiento similar, sin embargo, lo verdaderamente interesante ocurre cuando modificamos alguna propiedad del objeto al que se está haciendo referencia, ya sea desde la variable `profile` o desde la variable `account`.

![Desktop View](/assets/2026-03-29-javascript-tipos-primitivos-vs-tipos-por-referencia/1_K3wvze5zuktk5qK9YR_7Cg.webp)

Al modificar el valor de la propiedad `age` desde `account`, el objeto en cuestión es encontrado dentro del **Heap** y su valor es actualizado, y dado que ambos identificadores apuntan a la misma referencia de memoria, implícitamente están apuntando al mismo objeto que acaba de ser actualizado.

Esta es la razón por la cual se les denomina *Tipos por Referencia*.

Sin más, espero les haya servido esta información, y nos leemos en el siguiente artículo. Saludos y Happy Coding.

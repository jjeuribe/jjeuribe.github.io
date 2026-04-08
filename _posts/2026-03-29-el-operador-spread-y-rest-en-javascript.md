---
title: El operador Spread y Rest en Javascript
image:
  path: /assets/2026-03-29-el-operador-spread-y-rest-en-javascript/0_qEyW2R9JkyTiIrwc.webp
  alt: Photo by Jemima Whyles on Unsplash
date: 2026-03-29 16:00:00 -0600
categories: [Programming Languages, JavaScript]
tags: [javascript, javascript-fundamentals]     # TAG names should always be lowercase
---

Tanto **Spread** como **Rest** son dos de las mejoras que han revolucionado la manera en como trabajamos con Javascript.

En este artículo veremos algunos de los usos más comunes de estos operadores.

## El operador Spread

Piensa en el operador Spread como un unidad militar que despliega a sus elementos sobre un objetivo de manera coordinada siguiendo un protocolo.

En este sentido, Spread toma una estructura de datos **Iterable** y despliega a cada uno de los elementos de forma coordinada para que ocupen un lugar dentro de una estructura de datos objetivo.

### Manipulación de arreglos.

Podemos crear un arreglo a partir de otros:

```js
const initialSequence = [ 1, 2, 3 ];
const finalSequence = [ 8, 9, 10 ];
const currentSequence = [ ...initialSequence, 4, 5, 6, 7, ...finalSequence];

// [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]
console.log(currentSequence);
```
Fusionar o concatenar 2 arreglos entre sí:

```js
const cold = [ 'autumn', 'winter' ];
const warm = [ 'spring', 'summer' ];
const seasons = [ ...warm, ...cold ];

// [ 'autumn', 'winter', 'spring', 'summer' ]
console.log(seasons);
```

Clonar un arreglo a partir de otro:

```js
const words = [ 'have', 'a', 'good', 'day' ];
const greetings = [ ...words ];

// [ 'have', 'a', 'good', 'day' ]
console.log(greetings);
```

### Manipulación de objetos.

Podemos clonar un objeto a partir de otro:

```js
const dollyTheSheep = { species: 'Finnish Dorset', sex: 'Female' };
const dollysClone = { ...dollyTheSheep };

// { species: 'Finnish Dorset', sex: 'Female' }
console.log(dollysClone);
```

Fusionar 2 objetos entre sí para crear uno nuevo:

```js
const user = { name: 'Javi' };
const roleAdmin = { role: 'admin' };
const adminUser = { ...user, ...roleAdmin };

// { name: 'Javi', role: 'admin' }
console.log(adminUser);
```

En otro artículo abordaremos las diferentes maneras en que podemos clonar un objeto en Javascript.

### Mejora la invocación de una función.

En este caso, Spread nos permite rellenar los argumentos de una función de forma automática al momento de su invocación:

```js
// Ejemplo 1: 
const numbers = [ 2, 1, 3, 100, 5, 4 ];
const minNumber = Math.min(...numbers);
const maxNumber = Math.max(...numbers);

// Ejemplo 2: 
const cold = [ 'autumn', 'winter' ];
const warm = [ 'spring', 'summer' ];
const seasons = cold.push(...warm);

// Ejemplo 3:
const firstBitcoinTransactionDate = new Date(...[2009, 0, 9]);
```

Definitivamente es una manera más rápida y limpia de pasar argumentos a una función.

## El operador Rest

La sintaxis del operador Rest es la misma que la del operador Spread, sin embargo, su significado es diferente debido al contexto donde se utiliza.

### Asignar el resto de un arreglo u objeto a una variable

Esto tiene que ver durante el proceso de desestructuración. Básicamente el operador Rest ordena el despliegue de solamente algunos de sus elementos sobre el objetivo, mientras que al resto los mantiene dentro de la unidad.

Esto se puede entender mejor mediante un ejemplo:

```js
const [ autumn, winter, ...restOfTheSeasons] = ['autumn', 'winter', 'spring', 'summer'];

// autumn
console.log(autumn);

// winter
console.log(winter);

// [ 'spring', 'summer' ]
console.log(restOfTheSeasons);
```

Esto funciona de igual manera sobre un objeto:

```js
const box = { color: '#2080A3', width: 500, height: 500 };
const { color, ...size } = box;

// #2080A3
console.log(color);

// { width: 500, height: 500 }
console.log(size);
```

### Asignar un número variable de argumentos a una función

Esto tiene que ver durante la fase de diseño de una función. En este caso podríamos decir que Rest ordena la retirada reuniendo a todos sus elementos en un simple arreglo.

Esto se puede entender mejor con un ejemplo:

```js
function countArguments(...args) {
  console.log(args);

  return args.length;
}

// [ 1, 2, 3, 4, 5 ]
countArguments(1, 2, 3, 4, 5);
```

Ten en cuenta que solo se permite una declaración de este operador por función.

Si nuestra intención es compartir este operador con otros argumentos en la definición de la función, asegurate que se encuentre ubicado hasta el final:

```js
function filter(type, ...items) {
  return items.filter(item => typeof item == type);
}

// [ true, false, true ]
filter('boolean', true, 0, 1, false, true);

// [ 3, 1, 43, 37 ]
filter('number', 3, '3', 1, 43, true, false, 37);
```

Tanto el operador Spread como Rest son artefactos que todo desarrollador Javascript debería tener en su cajita de herramientas.

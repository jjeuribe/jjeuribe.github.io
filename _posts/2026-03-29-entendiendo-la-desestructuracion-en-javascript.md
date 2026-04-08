---
title: Entendiendo la desestructuración en Javascript
image:
  path: /assets/2026-03-29-entendiendo-la-desestructuracion-en-javascript/0_DgmcP_4jNSIY6FPo.webp
  alt: Photo by S&B Vonlanthen on Unsplash
date: 2026-03-29 15:00:00 -0600
categories: [Programming Languages, JavaScript]
tags: [javascript, javascript-fundamentals]     # TAG names should always be lowercase
---

La **desestructuración** es simplemente tomar una estructura de información y _"romperla"_ de tal manera que podamos hacer uso de cada pieza de forma individual.

Si buscamos una analogía sencilla, la desestructuración es el equivalente a _"desempacar"_ ciertos artículos personales de nuestra mochila de viaje.

En este sentido, nuestra mochila toma la forma de un arreglo, objeto, cadena de caracteres o cualquier tipo de estructura que posea un comportamiento **Iterable** (en otra oportunidad hablaremos sobre *Iterators* e *Iterables* en Javascript).

Por otro lado, nuestros artículos personales son las piezas de información individual que vamos a consumir.

Teniendo esto en mente te preguntarás, ¿qué podemos hacer con estas pequeñas piezas de información individual?, pues podemos por ejemplo asignarlas a variables de una forma más eficiente:

```js
const numbers = [ 1, 2, 3 ];

// En lugar de realizar esto: 
let x = numbers[0];
let y = numbers[1];
let z = numbers[2];

// Simplemente podemos hacer esto: 
let [ x, y, z ] = numbers; 

console.log(x); // 1
console.log(y); // 2
console.log(z); // 3
```

Incluso durante el proceso de asignación podemos excluir aquellos valores que no son de nuestro interés:

```js
const numbers = [ 1, 2, 3 ];

// x = 1, z = 3
let [ x, , z ] = numbers; 

// y = 2, z = 3
let [ , y, z ] = numbers;

// x = 1, y = 2, z = 3, a = undefined
let [ x, y, z, a ] = numbers;
```

O crear nuevas variables:

```js
const numbers = [ 1, 2, 3 ];

// x = 1, y = 2, z = 3, a = 4
let [ x, y, z, a = 4 ] = numbers;
```

Sin mencionar que también seremos capaces de desempacar información anidada de nuestra *"mochila"*:

```js
const alphanumericSequence = [ 'a', 'b', 'c', [1, 2, 3] ];

// numericSequence = [ 1, 2, 3 ]
let [ , , , numericSequence ] = alphanumericSequence; 

// x = 1, y = 2, z = 3
let [ , , , [ x, y, z ] ] = alphanumericSequence;
```

Un uso interesante es poder realizar un intercambio (swap) de valores entre 2 variables sin la necesidad de crear una variable temporal:

```js
let a = 1;
let b = 2; 

// En lugar de hacer esto: 
let temp = a; 
a = b; 
b = temp; 

// Simplemente podemos hacer esto: 
[ a, b ] = [ b, a ];

console.log(a); // 2
console.log(b); // 1
```

Hasta ahora, nuestra mochila de viaje se ha comportado como un humilde arreglo, veamos qué ocurre cuando toma la forma de un objeto.

## Desestructuración sobre objetos.

En este caso, la manera en que vamos a desempacar nuestros artículos personales de nuestra mochila de viaje es ligeramente diferente.

La regla de oro a tener en mente es que, el nombre otorgado a las variables que van a recibir los valores producto de la desestructuración deben coincidir con el nombre de las propiedades del objeto que estamos manipulando.

Como siempre, un fragmento de código habla mejor que 1000 palabras:

```js
const employee = {
  name: 'Julia',
  age: 27,
  job: {},
  workSchedule: {}
};

// El nombre de las variables coincide con el nombre
// de las propiedades del objeto. 
let { name, age, job, workSchedule } = employee;
```

Claramente el nombre de cada variable definida coincide con el nombre de cada propiedad del objeto.

Si por el contrario, el nombre otorgado a nuestras variables no es de nuestro agrado, siempre podemos utilizar un **alias**:

```js
let {
  name:employeeName,
  age:employeeAge,
  job:employeeJob,
  workSchedule:employeeWorkSchedule
} = employee;
```

Incluso podemos inicializar nuestras variables durante la fase de desestructuración:

```js
let {
  name = 'john',
  age = 18,
  job = 'worker',
  workSchedule = null
} = employee;

// Incluso utilizar un alias durante este proceso:
let {
  name:employeeName = 'john',
  age:employeeAge = 18,
  job:employeeJob = 'worker',
  workSchedule:employeeWorkSchedule = null
} = employee;
```

Y por supuesto, no puede faltar la habilidad de trabajar con estructuras anidadas:

```js
const employee = {
  name: 'Julia',
  age: 27,
  job: {},
  workSchedule: {
    mon: {
      open: 12, 
      close: 22,
    },
    tue: {
      open: 11,
      close: 23,
    },
    wed: {
      open: 23,
      close: 5,
    }
  }
};

let { workSchedule: { mon, tue, wed } } = employee;

console.log(mon); // { open: 12, close: 22 }
console.log(tue); // { open: 11, close: 23 }
console.log(wed); // { open: 23, close: 5 }
```

Aunque con este último punto hay que ser conservadores. Desestructurar información a través de múltiples niveles de anidación reduce la legibilidad de nuestro código:

```js
const employee = {
  name: 'Julia',
  age: 27, 
  workSchedule: {
    mon: {
      open: {
        from: 12
      },
      close: {
        to: 22
      }
    }
  }
};

// Multiples niveles de anidación
let { workSchedule: { mon: { open: {from}, close: {to} } } } = employee;

console.log(from); // 12
console.log(to);   // 22
```

Un uso interesante de la desestructuración de objetos ocurre cuando trabajamos con funciones:

```js
const dispatchDelivery = (endpoint, { userId, time = '00:00', address, items = [] }) => {
  // Some action...
};

dispatchDelivery('api/dispatch-delivery', {
  userId: 3392,
  address: 'Villas del Sol',
  time: '22:30',
  items: [ 'Beers', 'Sodas', 'Doritos' ]
});

dispatchDelivery('api/dispatch-delivery', {
  userId: 5423,
  address: 'Villas del Sol'
});
```

El resultado es una función con una definición más rica, legible y expresiva, sin duda una herramienta útil en funciones que requieren de múltiples parámetros.

La desestructuración de información es sin duda un mecanismo poderoso y muy utilizado en el mundo de Javascript, pero lo es aún más si lo combinamos de la mano con el operador **Spread**, ciertamente se pueden crear patrones de desarrollo interesantes.

Por último, ten en cuenta que la desestructuración se encuentra disponible solamente en las versiones modernas de Javascript, siendo concretos, a partir de la versión 6 del lenguaje.

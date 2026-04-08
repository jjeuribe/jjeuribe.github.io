---
title: Mapeo inteligente con flatMap en Javascript
image:
  path: /assets/2026-03-30-mapeo-inteligente-con-flatmap-en-javascript/0_WvZqCXixrhMK7-3d.webp
  alt: Photo by Annie Spratt on Unsplash
date: 2026-03-30 06:00:00 -0600
categories: [Programming Languages, JavaScript]
tags: [javascript, javascript-fundamentals, functional-programming]     # TAG names should always be lowercase
---

Podemos ver a `Array.flatMap` como el hermano inteligente de `Array.flat`. Si no sabes lo que es `Array.flat`, por acá te dejo un [link](https://jjeuribe.github.io/posts/que-diablos-es-flat-en-javascript) donde lo explico a detalle:

El método `Array.flatMap` básicamente se encarga de llevar a cabo 2 simples tareas:

1. Transformar cada elemento de nuestro arreglo subyacente.
2. _"Aplanar"_ o reducir en máximo 1 nivel el resultado final.

En este sentido, es el equivalente a utilizar de forma combinada los métodos `Array.map` y `Array.flat`.

Esto se entiende mejor con un ejemplo:

```js
const posts = [
  { id: 1, title: 'When the Yogurt Took Over', tags: ['Sci-fi', 'Fantasy'], userId: 666 },
  { id: 2, title: 'The Mountain', tags: ['Romantic', 'Love Stories'], userId: 666 },
  { id: 3, title: 'Against all Odds', tags: ['Survival', 'War Stories'], userId: 666 },
];
```

Nuestro objetivo es obtener todos los tags de las publicaciones del usuario `666` con la siguiente estructura:

```js
[ 'Sci-fi', 'Fantasy', 'Romantic', 'Love Stories', 'Survival', 'War Stories' ]
```

Veamos, como primer paso se me ocurre echar mano de mi viejo conocido `Array.map` para extraer solamente la información que necesito:

```js
const tags = posts
              .map((post) => post.tags);

console.log(tags);
// [
//   ['Sci-fi', 'Fantasy'],
//   ['Romantic', 'Love Stories'],
//   ['Survival', 'War Stories']
// ]
```

Perfecto, con esto tengo todos los tags de las publicaciones, sin embargo, la estructura que obtengo no es la ideal.

Puedo apreciar que resultado es un arreglo de 2 dimensiones, ¿que tal si echamos mano de nuestro recién conocido amigo `Array.flat` para reducir esta estructura en 1 nivel? 🤔:

```js
const tags = posts
              .map((post) => post.tags)
              .flat();

console.log(tags);
// [
//  'Sci-fi',
//  'Fantasy',
//  'Romantic',
//  'Love Stories',
//  'Survival',
//  'War Stories'
// ]
```

Voalá, justo lo que quiero.

La pregunta ahora es ¿puedo llevar a cabo todo lo anterior en una sola acción? y la respuesta es `Array.flatMap`:

```js
const tags = posts
              .flatMap((post) => post.tags);

console.log(tags);
// [
//  'Sci-fi',
//  'Fantasy',
//  'Romantic',
//  'Love Stories',
//  'Survival',
//  'War Stories'
// ]
```

Pan comido ¡eh! 😎.

Venga, ya me mostraste al conejo 🐇 veamos hasta dónde llega su madriguera, ¡Muéstrame un ejemplo más práctico!

Con esta información:

```js
const availabilityZones = [
  { id: 1, code: 'a', region: 'us-west-1' }, 
  { id: 2, code: 'a', region: 'eu-central-1' },
  { id: 3, code: 'b', region: 'us-west-1' },
  { id: 4, code: 'a', region: 'sa-east-1' },
  { id: 5, code: 'a', region: 'us-east-1' },
  { id: 6, code: 'b', region: 'eu-central-1' },
  { id: 7, code: 'c', region: 'us-west-1' }
];

const datacenters = [
  { id: 100121, address: '1100 Harris Hill LN Tarboro', azId: 1 }, 
  { id: 100122, address: '3602 Maldon Way Hight Point', azId: 1 },
  { id: 100123, address: '1233 Rusell RD Dento', azId: 1 },
  { id: 133322, address: '6052 Turnerheim Sindlingen, 85 Farbenstraße', azId: 2 },
  { id: 100055, address: '6032 DFB-Campus, 274 Kennedyallee', azId: 2 }, 
  { id: 100023, address: '2800 Sam Wilson RD Charlotte' , azId: 3 },
  { id: 100024, address: '1145 Balfour Quarry RD Salisbury' , azId: 3 },
  { id: 112993, address: '1283 Rua Vigario João Álvares', azId: 4 }, 
  { id: 112992, address: '4330 Rua Ricardo Gumbleton Daunt', azId: 4 }, 
  { id: 100323, address: '9305 Merrifield AVE Fairfax Salem', azId: 5 }, 
  { id: 100343, address: '8012 Broad ST Portsmouth Indian Valley', azId: 5 },
  { id: 121049, address: '6043 Zum Kaagärtner Vereinshaus, 15 Am Ginnheimer', azId: 6 },
  { id: 130049, address: '6055 Symphatie, 9 Altenhainer Straße', azId: 6 },
  { id: 100432, address: '3917 Shelby AVE Kitty Hawk', azId: 7 }, 
  { id: 100423, address: '1704 Featherwood Court Charlotte', azId: 7 }
];
```
Nos piden crear un artefacto que tenga la capacidad de filtrar todos los datacenters de cualquier zona de disponibilidad de acuerdo a 1 o más regiones.

Ejemplo:

* Filtra todos los datacenters de la región `us-west-1`.
* Filtra todos los datacenters de la región `us-west-1` y `eu-central-1`.
* Etc.

Para resolver este problema se me ocurre crear una clase que se encargue de llevar a cabo las tareas de recibir los filtros y entregar los resultados.

Algo como esto:

```js
class DatacenterFilter {

  constructor(availabilityZones, datacenters) {
    this.filters = [];
    this.availabilityZones = availabilityZones;
    this.datacenters = datacenters;
  }

  static create(availabilityZones, datacenters) {
    return new DatacenterFilter(availabilityZones, datacenters);
  }

  add(filter) {
    this.filters.push(filter);
  }

  getMatchingResults() {

    const filterDatacentersByAZ = (az) =>
      this.datacenters.filter((dc) => az.id === dc.azId);

    const filterAZsByRegion = (region) =>
      this.availabilityZones.filter((az) => region === az.region);

    const filterDatacentersByRegion = (filter) =>
      filterAZsByRegion(filter).flatMap(filterDatacentersByAZ);

    return this.filters.flatMap(filterDatacentersByRegion);
  }
}
```

Por el momento no te abrumes con todos los detalles de implementación, pero presta atención al método `getMatchingResults()`, en especial a la partes donde hacemos uso del método `Array.flatMap`, allí es donde se esconde el señor conejo 🐇:

![Desktop View](/assets/2026-03-30-mapeo-inteligente-con-flatmap-en-javascript/1_rzxBQ4sYlyq5GM0QF6ep1A.webp)

La función `filterDatacentersByRegion()` básicamente funciona como un disparador de las funciones `filterAZsByRegion()` y `filterDatacentersByAZ()`:

![Desktop View](/assets/2026-03-30-mapeo-inteligente-con-flatmap-en-javascript/1_kTTZKzl1up6y9DtW52HI_w.webp)

Aislemos por un momento la ejecución de `filterAZsByRegion()` pasándole como argumento la región `us-west-1`:

![Desktop View](/assets/2026-03-30-mapeo-inteligente-con-flatmap-en-javascript/1_TTmjSsueF4QmVPYCXuSXtg.webp)

Con esta información podemos obtener los datacenters asociados a estas 3 zonas de disponibilidad, de paso también cambiemos ese `Array.flatMap` por `Array.map` solo para observar el resultado:

![Desktop View](/assets/2026-03-30-mapeo-inteligente-con-flatmap-en-javascript/1_0l8XP3z1DnD7rBBRrmfB6A.webp)

Mmmm 🤔 no es la estructura que busco, regresemos a nuestro viejo `Array.flatMap` a su lugar y observemos el resultado:

![Desktop View](/assets/2026-03-30-mapeo-inteligente-con-flatmap-en-javascript/1_c_hA55EPwXSH9kxIOq7KdA.webp)

Mucho mejor 👌.

Ahora que conocemos los detalles `filterDatacentersByRegion()`, sólo queda la pregunta ¿quién se encarga de ejecutarla? 🤨

Ese es trabajo es de esta pequeña línea:

![Desktop View](/assets/2026-03-30-mapeo-inteligente-con-flatmap-en-javascript/1_itPLjc4rXcCqAdhjXs1c0g.webp)

¿Puedes adivinar por qué?

Es correcto, la función `filterDatacentersByRegion()` es ejecutada por cada filtro que hayamos agregado. Básicamente algo así:

![Desktop View](/assets/2026-03-30-mapeo-inteligente-con-flatmap-en-javascript/1_LEHCQJnoFca1r7yQf1888A.webp)

Como ejercicio intenta responder a la siguiente pregunta ¿por qué hemos utilizado el método `Array.flatMap` en esta línea?, te daré una pista, cambia ese `Array.flatMap` por un `Array.map` y observa el resultado.

De cualquier manera, te dejaré por acá el código del ejemplo para que saques tus conclusiones:

```js
const availabilityZones = [
  { id: 1, code: 'a', region: 'us-west-1' }, 
  { id: 2, code: 'a', region: 'eu-central-1' },
  { id: 3, code: 'b', region: 'us-west-1' },
  { id: 4, code: 'a', region: 'sa-east-1' },
  { id: 5, code: 'a', region: 'us-east-1' },
  { id: 6, code: 'b', region: 'eu-central-1' },
  { id: 7, code: 'c', region: 'us-west-1' }
];

const datacenters = [
  { id: 100121, address: '1100 Harris Hill LN Tarboro', azId: 1 }, 
  { id: 100122, address: '3602 Maldon Way Hight Point', azId: 1 },
  { id: 100123, address: '1233 Rusell RD Dento', azId: 1 },
  { id: 133322, address: '6052 Turnerheim Sindlingen, 85 Farbenstraße', azId: 2 },
  { id: 100055, address: '6032 DFB-Campus, 274 Kennedyallee', azId: 2 }, 
  { id: 100023, address: '2800 Sam Wilson RD Charlotte' , azId: 3 },
  { id: 100024, address: '1145 Balfour Quarry RD Salisbury' , azId: 3 },
  { id: 112993, address: '1283 Rua Vigario João Álvares', azId: 4 }, 
  { id: 112992, address: '4330 Rua Ricardo Gumbleton Daunt', azId: 4 }, 
  { id: 100323, address: '9305 Merrifield AVE Fairfax Salem', azId: 5 }, 
  { id: 100343, address: '8012 Broad ST Portsmouth Indian Valley', azId: 5 },
  { id: 121049, address: '6043 Zum Kaagärtner Vereinshaus, 15 Am Ginnheimer', azId: 6 },
  { id: 130049, address: '6055 Symphatie, 9 Altenhainer Straße', azId: 6 },
  { id: 100432, address: '3917 Shelby AVE Kitty Hawk', azId: 7 }, 
  { id: 100423, address: '1704 Featherwood Court Charlotte', azId: 7 }
];

class DatacenterFilter {    
  constructor(availabilityZones, datacenters) {
    this.filters = [];
    this.availabilityZones = availabilityZones; 
    this.datacenters = datacenters; 
  }

  static create(availabilityZones, datacenters) {
    return new DatacenterFilter(availabilityZones, datacenters); 
  }

  add(filter) {
    this.filters.push(filter); 
  }

  getMatchingResults() {
    const filterDatacentersByAZ = (az) => this.datacenters.filter((dc) => az.id === dc.azId);
    const filterAZsByRegion = (region) => this.availabilityZones.filter((az) => region === az.region);
    const filterDatacentersByRegion = (filter) => filterAZsByRegion(filter).flatMap(filterDatacentersByAZ); 

    return this.filters.flatMap(filterDatacentersByRegion);
  }
}

const filter = DatacenterFilter.create(availabilityZones, datacenters); 

filter.add('us-west-1');
filter.add('sa-east-1'); 

const result = filter.getMatchingResults(); 

console.log(result);
```

Sin más agradezco tu tiempo y me despido, ¡Happy Coding! 🐇




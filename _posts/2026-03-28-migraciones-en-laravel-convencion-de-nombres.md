---
title: Migraciones en Laravel - Convención de Nombres
image:
  path: /assets/2026-03-28-migraciones-en-laravel-convencion-de-nombres/1_hbPIHiOXLFKSP7F4w-oMEQ.webp
date: 2026-03-28 14:00:00 -0600
categories: [Web Development, Laravel]
tags: [laravel, migraciones, laravel-migrations]     # TAG names should always be lowercase
---

Uno de los aspectos más difíciles en el mundo del desarrollo es elegir un nombre apropiado para los diferentes elementos que componen nuestro código: variables, clases, funciones, interfaces, etc, lo mismo aplica para aquellos elementos que contienen nuestro código: paquetes, carpetas, ficheros, etc. Al final, el objetivo es lograr una fuerte consistencia temática en la encapsulación y resolución de un problema.

En esta ocasión, hablaré sobre la *convención de nombres* que utilizo para nombrar mis archivos de migración en Laravel.

En primer lugar, considero que es importante entender que cada uno de estos archivos de migración debe de seguir una determinada *convención de nombre* que describa qué es lo que la migración va a realizar cuando sea ejecutada, y de forma inversa e intuitiva, inferir lo que va a ocurrir cuando sea revertida.

En este punto, debemos detenernos y tomar el tiempo necesario para pensar en un nombre que sea consistente y descriptivo, que minimice las ambigüedades y que defina la intención de lo que deseamos lograr con nuestro archivo de migración.

Si analizamos el archivo de todo proyecto en Laravel:

```
Illuminate/Database/Console/Migrations/TableGuesser.php
```

Nos daremos cuenta de que, por defecto existen 2 patrones encargados de inferir el comportamiento de la migración:

```php
class TableGuesser
{
  const CREATE_PATTERNS = [
    '/^create_(\w+)_table$/',
    '/^create_(\w+)$/',
  ];

  const CHANGE_PATTERNS = [
    '/_(to|from|in)_(\w+)_table$/',
    '/_(to|from|in)_(\w+)$/',
  ];
}
```

El patrón `CREATE_PATTERNS` indica que si el nombre de nuestro archivo de migración comienza con la palabra **create**, Laravel agrega el `Schema::create` al método `up()` y `Schema::dropIfExists` al método `down()` de la Clase.

El patrón `CHANGE_PATTERNS` indica que sí el nombre de nuestro archivo de migración contiene las palabras **to**, **form** o **in**, Laravel hace `uso de Schema::table` en los métodos `up()` y `down()` de la Clase.

Si el nombre de nuestro archivo de migración no cumple con alguno de los patrones de creación o modificación previamente mencionados, simplemente no agrega nada a los métodos `up()` y `down()` de la Clase, permitiéndonos de esta manera definir la lógica de migración de acuerdo a nuestras necesidades.

Al nombrar un archivo de migración, busco tomar alguno de estos 2 enfoques:

* Orientado a una **acción**.
* Orientado a una **feature**.

Para efectos de claridad, obviaré el uso del comando `php artisan make:migration` que antecede al nombre de cada archivo de migración.

## Enfoque orientado a una acción.

Aquí tengo la opción de ir por un camino simplista o perseguir una convención similar a una definición de datos (DDL).

El camino simplista implica tomar alguno de estos verbos y utilizarlo al inicio del nombre de mi fichero de migración: *add*, *create*, *remove*, *delete*, *set*, *update*, *modify*, *make*, *change*, *restore*, *drop*, *move* o *rename*.

Ejemplos:

```php
// Agregar una nueva columna.
add_featured_image_url_to_posts_table

// Eliminar una columna existente.
drop_featured_image_url_from_posts_table

// Cambiar el nombre a una columna existente
rename_featured_image_url_in_posts_table
  
// Cambiar el tipo de una columna existente.
change_status_type_in_posts_table

// Mover de posición una columna hacia delante en relación a otra columna.
move_featured_image_url_after_body_in_posts_table
  
// Mover de posición una columna hacia atrás en relación a otra columna.
move_featured_image_url_before_body_in_posts_table
  
// Añadir un índice a una columna existente.
set_name_to_index_on_users_table
  
// Añadir un índice único a una columna existente.
set_email_to_unique_on_users_table
  
// Añadir una llave foránea a una columna existente.
set_user_id_to_foreign_key_on_posts_table
  
// Agregar el comportamiento de Soft Deletes a una tabla existente.
restore_hard_deletes_on_posts_table
```

En algunos casos tenemos que adaptar el nombre de la tabla a la que hacemos referencia desde el archivo de migración.

Optar por la convención similar a la de una sentencia DDL implica simplemente hacer uso de la palabra **alter** y algunos agregados descriptivos demás.

Ejemplos:

```php
// Agregar una nueva columna.
alter_posts_table_add_featured_image_url
  
// Eliminar una columna existente.
alter_posts_table_drop_user_id

// Añadir un índice a una columna existente.
alter_users_table_add_index_on_name

// Añadir un índice único a una columna existente.
alter_users_table_make_name_unique
  
// Cambiar el tipo de una columna existente.
alter_users_table_change_status_type
  
// Mover de posición una columna hacia delante en relación a otra columna.
alter_users_table_modify_featured_image_url_after_body
```

Incluso, dentro de un mismo fichero de migración podemos llevar a cabo múltiples tareas dependiendo del caso:

```php
// Agregar múltiples columnas a nuestra tabla de publicaciones.
alter_posts_table_add_name_and_lastname_and_email

// Alterar una columna y agregar otras en la misma acción.
alter_posts_table_modify_email_and_add_name_and_lastname
```

Aunque en estos casos, podemos caer en la ambigüedad o en nombres de archivo excesivamente largos dependiendo de las acciones involucradas.

## Enfoque orientado a una feature.

Esta convención es con la que me siento más identificado. Pienso que el nombre es más amigable y entendible, puedo leer mis archivos de migración como si de historias se tratasen, además cada archivo de migración puede realizar 1 o varias tareas para alcanzar el objetivo deseado.

```php
implement_user_roles
  
organize_posts_under_categories
  
make_post_likeable

clear_today_user_logs

implement_post_iframes

make_user_verifiable

make_post_reviewable

set_post_visibility

make_post_editable_only_by_owner_and_admin
```

Por último, tal vez te estés preguntando ¿No deberíamos evitar realizar múltiples tareas en un solo archivo de migración y dividirlas mejor en múltiples archivos?.

La respuesta a esta pregunta es muy subjetiva y por lo tanto debatible. En mi opinión, va a depender de los requerimientos del problema; habrá ocasiones en que dividir la tarea en múltiples archivos de migración sea la solución correcta, y en otras ocasiones, llevar a cabo múltiples tareas en el mismo archivo de migración será lo más viable.

Si bien creo que todo archivo de migración debe ser lo más descriptivo posible, también creo que es un tema debatible y que no debemos ser tan restrictivos.

Me gustaría saber qué convención o patrón siguen ustedes para nombrar sus archivos de migración. Saludos y Happy Coding.

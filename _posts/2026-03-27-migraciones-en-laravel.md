---
title: Migraciones en Laravel
image:
  path: /assets/2026-03-27-migraciones-en-laravel/flying-birds.webp
date: 2026-03-27 14:00:00 -0600
categories: [Web Development, Laravel]
tags: [laravel, migraciones, laravel-migrations, base-de-datos, eloquent]     # TAG names should always be lowercase
---

Seguramente en algún punto dentro del desarrollo de nuestra base de datos hemos atravesado por alguna de estas fases: _Análisis de requerimientos_ donde definimos las reglas de negocio, _Diseño de la Solución_ donde modelamos la información (Modelo Conceptual) y la representamos de forma gráfica, y la _Implementación_ (Modelo Físico) donde básicamente llevamos a la realidad la manera en que se almacenará nuestra información, es decir, donde creamos la base de datos, las tablas, las columnas, generamos los índices, establecemos las restricciones, etc.

Es en esta fase de _implementación_ donde por lo regular hacemos uso de un gestor de base de datos como Mysql, Oracle, PostgreSQL, SQL Server, Mongodb, etc, para llevar a cabo las tareas previamente mencionadas, y es en esta fase donde haremos mayor énfasis.

Por lo común, las tareas dentro del _Modelo Físico_ se llevan a cabo de forma manual, por ejemplo, directamente a través de un cliente en consola o a través de una interfaz gráfica como Mysql Workbench o DBeaver. Otra manera de lidiar con esta tarea es a través de la codificación de un script que automatice esta tarea por nosotros. En cualquier caso, llegar a ejecutar los cambios de nuestra db de alguna de estas formas puede llegar a ser complicada y/o engorrosa.

Con Laravel, todas estas tareas pueden ser llevadas a cabo de una forma más sencilla y gestionable a través del uso de las **_migraciones_**.

Una **_migración_** nos permite crear y alterar el esquema de nuestras tablas, generar índices, establecer restricciones, etc, sin embargo, también actúa como una especie de control de versiones sobre nuestra base de datos, esto es, si eventualmente necesitamos consultar un cambio anterior o decidimos que la versión inicial o previa era la que se necesita, en lugar de reescribir todo, simplemente lanzando algunos comandos podemos volver hacia atrás en la línea de tiempo de nuestra db.

## Creando nuestra primera migración

Para crear una migración simplemente ejecutamos el siguiente comando:

```shell
> php artisan make:migration create_posts_table
```

Con esto se habrá generado un archivo de migración que tendrá por objetivo crear la tabla de nuestras publicaciones.

Por defecto, Laravel ubica los archivos de migración en `database/migrations`, además, podemos notar que el nombre del archivo de migración generado posee una nomenclatura que lo identifica:

![Desktop View](/assets/2026-03-27-migraciones-en-laravel/0_LuMhpJ7q2qHMby8f.webp)
_Estructura de nombre de un archivo de migración_

Sin embargo, lo realmente interesante se encuentra dentro de nuestro archivo de migración recién generado:

```php
class CreatePostsTable extends Migration
{
  /**
  * Run the migrations.
  *
  * @return void
  */
  public function up()
  {
    Schema::create('posts', function (Blueprint $table) {
      $table->id();
      $table->string('title');
      $table->string('url')->unique();
      $table->mediumText('excerpt')->nullable();
      $table->text('body')->nullable();
      $table->timestamp('published_at')->nullable();
      $table->timestamps();
    });
  }

  /**
  * Reverse the migrations.
  *
  * @return void
  */
  public function down()
  {
    Schema::dropIfExists('posts');
  }
}
```

Dentro del cuerpo de esta Clase podemos identificar 2 métodos:

* Dentro del método `up()` se encuentra aquel código que altera de alguna manera nuestra tabla, por ejemplo, agregar columnas, cambiar el tipo de dato de una columna, generar índices, establecer restricciones, etc. Este método es invocado en el momento que ejecutamos la migración.
* El método `down()` hace lo opuesto, esto es, ejecutar código que revierta lo realizado por el método `up()`. Este método es invocado en el momento en que decidimos revertir la migración.

## Ejecutando una migración

Existen múltiples maneras de correr una migración, sin embargo, la manera más sencilla de hacerlo es con el siguiente comando:

```shell
> php artisan migrate
```

Esto básicamente ejecuta el método `up()` de nuestra migración. En este punto, se ha creado la tabla `posts` dentro de nuestra base de datos.

## Revirtiendo una migración

Si nuestra intención es deshacer cualquier operación realizada por la ejecución de las migraciones previas, simplemente ejecutamos:

```shell
> php artisan migrate:rollback
```

Esto básicamente ejecuta el método `down()` de nuestra migración. En este punto, se ha removido la tabla `posts` dentro de nuestra base de datos.

## Migraciones e identificadores de Lote

Creo que esta parte es una de las más importantes y un poco engañosas de entender. Si inspeccionamos la base de datos de nuestro proyecto veremos que existe una tabla llamada *migrations* que es la encargada de llevar a cabo el control de nuestras migraciones.

![Desktop View](/assets/2026-03-27-migraciones-en-laravel/0_Pck566gNaPgkq2hE.webp)

Dentro de esta tabla podemos identificar una columna llamada *batch*. El valor de esta columna es utilizada por Laravel para determinar el orden de ejecución y/o reversión de nuestras migraciones, básicamente funciona como un **_identificador de lote_**.


![Desktop View](/assets/2026-03-27-migraciones-en-laravel/0_LwESTbtmABwezZyo.webp)

Las migraciones cuyo identificador de lote o batch es 1, son aquellas que ejecutamos por lo común al inicio de nuestro proyecto. La migración con identificador de lote 2, es la migración que recién ejecutamos para crear la tabla de publicaciones que pretendemos utilizar en nuestro proyecto.

Vamos a generar 2 nuevas migraciones: categorías y tags.

```shell
> php artisan make:migration create_categories_table
> php artisan make:migration create_tags_table
```

Ejecutamos nuestra suite de migración:

```shell
php artisan migrate
```

Y obtenemos el siguiente resultado:

![Desktop View](/assets/2026-03-27-migraciones-en-laravel/0_XJlk0reLvz2aREcI.webp)

Las nuevas migraciones han sido ejecutadas y ahora poseen el identificador de lote 3. Básicamente cada vez que creamos y corremos nuevas migraciones sobre nuestro proyecto, se genera un identificador de lote propio.

Podemos observar que este identificador de lote se va generando de forma incremental en el caso de nuevas migraciones, de igual manera, si revertimos una migración, el identificador de lote irá disminuyendo. Probemos ejecutando el comando de reversión de migraciones previamente mencionado:

```shell
> php artisan migrate:rollback
```

El resultado es el siguiente:

![Desktop View](/assets/2026-03-27-migraciones-en-laravel/0_iESKtttcPFLOqI3a.webp)

Podemos apreciar que no todas las migraciones fueron revertidas, solo las últimas, las que tenían el número de lote 3, básicamente hemos vuelto un paso hacia atrás. Si nuevamente ejecutamos el comando de *rollback*, el resultado será:

![Desktop View](/assets/2026-03-27-migraciones-en-laravel/0_eGrawyQDimCskq7d.webp)

Y si nuevamente revertimos nuestras migraciones con el comando anterior en este punto, solo nos quedaremos con:

![Desktop View](/assets/2026-03-27-migraciones-en-laravel/0_rDk971XU9meeskVH.webp)

absolutamente nada, nuestra base de datos no contiene tabla alguna a excepción de la tabla *migrations*.

Tal vez te preguntes ¿Por qué sucede esto?, nuevamente la respuesta se encuentra en el valor del campo *batch*.

El proceso de *rollback* solamente revierte un lote o conjunto de migraciones a la vez tomando como referencia el valor del campo *batch*, es por ello que tuvimos que ejecutar 3 veces el procedimiento de reversión a través del comando *rollback* en la consola de Laravel.

Si nuestra intención es revertir todas la migraciones sin importar el identificador de lote, simplemente lanzamos en la terminal:

```shell
> php artisan migrate:reset
```

Alternativamente, podemos ejecutar un *rollback* de forma selectiva:

```shell
> php artisan migrate:rollback --step=1
```

En este caso solo se eliminara la migración más reciente pero respetando el lote:

![Desktop View](/assets/2026-03-27-migraciones-en-laravel/0_DmBUP4jxjmw714cT.webp)

## Reversión y Migración en un sola acción

Cuando nos encontramos en modo desarrollo es muy común borrar todas las tablas haciendo un *rollback* y recrearlas nuevamente ejecutando la migración. Para poder realizar esta tarea en un solo paso simplemente:

```shell
> php artisan migrate:fresh
```

Sin embargo, dado que esta acción es destructiva se desaconseja su uso en producción.

Adicionalmente, podemos agregar una acción más a este comando, y esto es, correr nuestros *seeders*:

```shell
> php artisan migrate:fresh --seed
```

## Migrando sin perder la información

Probablemente te estes preguntando como alterar el esquema de una tabla sin perder la información que contiene.

Para poder alterar la estructura de una tabla sin perder la información que contiene simplemente creamos migraciones adicionales. Cada una de estas migraciones debe seguir una determinada convención que describe lo que hace.

Por ejemplo, digamos que necesitamos agregar una nueva columna a nuestra tabla de publicación para guardar la url de una imagen destacada, simplemente escribiríamos algo como esto:

```shell
> php artisan make:migration add_featured_image_url_to_posts_table
```
Si abrimos el archivo recién generado, podemos observar que el cuerpo de la Clase cambia ligeramente:

```php
class AddFeaturedImageUrlToPostsTable extends Migration
{
  /**
  * Run the migrations.
  *
  * @return void
  */
  public function up()
  {
    Schema::table('posts', function (Blueprint $table) {
      // Deseamos agregar esta nueva columna a nuestra tabla 'posts'
      $table->text('featured_image_url')->nullable();
    });
  }

  /**
  * Reverse the migrations.
  *
  * @return void
  */
  public function down()
  {
    Schema::table('posts', function (Blueprint $table) {     
      // Debemos es explícitos e indicar que queremos hacer cuando esta migración sea revertida.
      // En nuestro caso, deseamos eliminar la columna previamente creada.
      $table->dropColumn('featured_image_url');
    });
  }
}
```

En el método `up()` en lugar de crear una nueva tabla, hacemos referencia a una existente, en este caso, a la tabla *posts*. De igual manera, dentro del método `down()` indicamos que deseamos eliminar una columna, en lugar de la tabla completa.

Ya simplemente queda ejecutar la migración, y listo, tendremos una nueva columna en nuestra tabla de publicaciones.

Hablar sobre la convención de nombres que debemos utilizar para agregar nuevas columnas, índices, restricciones daría para otro articulo en el futuro. Espero esta información te haya servido para entender un poco más sobre las migraciones en Laravel.


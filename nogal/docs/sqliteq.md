# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# sqliteq
## nglDBSQLiteQuery *extends* nglStd [instanciable] [20160201]
Controla los resultados generados por consultas a la bases de datos SQLite.
  
## Variables
`private` $db = Objeto SQLite3Result  
`private` $cursor = Modos de INSERT y UPDATE  

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**column**|string|null|Nombre de una columna del grupo de resultados.
En el método **getAll**, cuando el nombre este presedido de un **#**, el valor de la columna será utilizado como indice asociativo.
Cuando el nombre este presedido de un **@**, el resultado será tratado con **nglCommon::arrayGroup** y el valor de la columna será utilizado como indice asociativo.|
|**get_group**|array|null|Estructura de agrupamiento de **nglCommon::arrayGroup** utilizada en el método getAll cuando column está precedido por una **@**|
|**get_mode**|string|assoc|tipo de modo GET. Valores admitidos:<ul><li>**assoc** =  devuelve un array indexado por el nombre de columna</li><li>**num** =  devuelve un array indexado por el número de columna, empezando por la columna 0</li><li>**both** =  devuelve un array indexado tanto por el nombre como por el número de columna empezando por la columna 0</li></ul>|
|**link**|resource|null|Puntero del driver de base de datos|
|**query**|object|null|Resultado de la última consulta ejecutada|
|**sentence**|string|null|Última consulta ejecutada|
|**query_time**|string|null|Tiempo que tomó la última consulta ejecutada|

## Atributos
|Atributo|Tipo|Descripción|
|---|---|---|
|**sql**|string|Sentencia SQL ejecutada|
|**time**|float|Tiempo que tomó la ejecución|
|**crud**|string|Tipo de sentencia CRUD (SELECT, INSERT, UPDATE, REPLACE or DELETE) o false en caso de otra sentencia|

  
&nbsp;

# Métodos
- [GetMode](#GetMode)
- [allrows](#allrows)
- [columns](#columns)
- [count](#count)
- [destroy](#destroy)
- [free](#free)
- [get](#get)
- [getall](#getall)
- [getobj](#getobj)
- [lastid](#lastid)
- [load](#load)
- [reset](#reset)
- [rows](#rows)
- [toArray](#toArray)

  
&nbsp;


## allrows
retorna el número de registros del conjunto de resultados ignorando los valores LIMIT  

**[int]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## columns
Retorna un array con los nombre de las columnas presentes en el resultado  

**[array]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## count
Devuelve el número de filas involucradas en la última consulta ejecutada.
Si la consulta es de los tipos **INSERT, UPDATE, REPLACE o DELETE** devolverá la cantidad de filas afectadas.
Si en cambio se trata de un conjunto de resultados, devolverá la cantidad de filas del mismo.  

**[int]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## destroy
Libera la memoria asociada con el identificador del resultado y destruye el objeto  

**[boolean]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## get
Obtiene una fila de resultados en forma de array y avanza el puntero.
Cuando se especifique **\$sColumn** y el valor sea el nombre una columna del grupo de resultados, se retornará unicamente el valor de dicha columna  

**[mixed]** =  *public* function ( *string* \$sColumn, *string* \$sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sColumn**|string|null|Nombre de una columna del grupo de resultados.
En el método **getAll**, cuando el nombre este presedido de un **#**, el valor de la columna será utilizado como indice asociativo.
Cuando el nombre este presedido de un **@**, el resultado será tratado con **nglCommon::arrayGroup** y el valor de la columna será utilizado como indice asociativo.|
|**\$sMode**|string|assoc|tipo de modo GET. Valores admitidos:<ul><li>**assoc** =  devuelve un array indexado por el nombre de columna</li><li>**num** =  devuelve un array indexado por el número de columna, empezando por la columna 0</li><li>**both** =  devuelve un array indexado tanto por el nombre como por el número de columna empezando por la columna 0</li></ul>|
### Ejemplos  
#### conexión  
```php
$foo = $ngl("sqlite.foobar");
$foo->base = "shop.sqlite";
$foo->connect();
$bar = $foo->query("SELECT * FROM `users`");
print_r($bar->get());
```

&nbsp;
___
&nbsp;

## getall
Obtiene todas las filas de resultados en forma de array bidimensional.
Cuando se especifique **\$sColumn** y el valor sea el nombre una columna del grupo de resultados, se retornará unicamente el valor de dicha columna;
excepto cuanto el nombre esté presedido de un **#**, en este caso se retornará un array multidimensional donde el valor de la columna será utilizado como indice asociativo del primer nivel. En un segundo nivel se agruparan los registros que tengan igual valor en el campo **\$sColumn**.
Este método reinicia el conjunto de resultados a la primera fila.  

**[mixed]** =  *public* function ( *string* \$sColumn, *string* \$sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sColumn**|string|null|Nombre de una columna del grupo de resultados.
En el método **getAll**, cuando el nombre este presedido de un **#**, el valor de la columna será utilizado como indice asociativo.
Cuando el nombre este presedido de un **@**, el resultado será tratado con **nglCommon::arrayGroup** y el valor de la columna será utilizado como indice asociativo.|
|**\$sMode**|string|assoc|tipo de modo GET. Valores admitidos:<ul><li>**assoc** =  devuelve un array indexado por el nombre de columna</li><li>**num** =  devuelve un array indexado por el número de columna, empezando por la columna 0</li><li>**both** =  devuelve un array indexado tanto por el nombre como por el número de columna empezando por la columna 0</li></ul>|
### Ejemplos  
#### conexión  
```php
$foo = $ngl("sqlite.foobar");
$foo->base = "shop.sqlite";
$foo->connect();
$bar = $foo->query("SELECT * FROM `users`");

[b]print_r($bar->getall());[/b]
Array (
    0 => Array(id => 1, name => Juan, age => 36),
    1 => Array(id => 2, name => Pedro, age => 28),
    2 => Array(id => 3, name => Manuel, age => 36)
);

[b]print_r($bar->getall(name));[/b]
Array (
    0 => Juan,
    1 => Pedro,
    2 => Manuel
);

[b]print_r($bar->getall(#age));[/b]
Array (
    36 => Array(
        0 => Array(id => 1, name => Juan, age => 36),
        1 => Array(id => 3, name => Manuel, age => 36)
    ),
    28 => Array(id => 2, name => Pedro, age => 28)
);
```

&nbsp;
___
&nbsp;

## GetMode
Selecciona el modo de salida para los métodos **get** y **getall**  

**[int]** =  *protected* function ( *string* \$sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sMode**|string|assoc|tipo de modo GET. Valores admitidos:<ul><li>**assoc** =  devuelve un array indexado por el nombre de columna</li><li>**num** =  devuelve un array indexado por el número de columna, empezando por la columna 0</li><li>**both** =  devuelve un array indexado tanto por el nombre como por el número de columna empezando por la columna 0</li></ul>|

&nbsp;
___
&nbsp;

## getobj
Obtiene una fila de resultados en forma de objeto stdClass y avanza el puntero.  

**[object]** =  *public* function ( );
  
### Ejemplos  
#### conexión  
```php
$foo = $ngl("sqlite.foobar");
$foo->base = "shop.sqlite";
$foo->connect();
$bar = $foo->query("SELECT * FROM `users`");
var_dump($bar->getobj());
```

&nbsp;
___
&nbsp;

## free
Libera la memoria asociada con el identificador del resultado  

**[boolean]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## lastid
Retorna el ID de la fila de la sentencia INSERT más reciente realizada en la base de datos  

**[int]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## load
Carga la ultima consulta ejecutada del driver.  

**[boolean]** =  *public* function ( *resource* \$link, *object* \$query );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$link**|resource|null|Puntero del driver de base de datos|
|**\$query**|object|||

&nbsp;
___
&nbsp;

## reset
Reinicia el conjunto de resultados a la primera fila.  

**[boolean]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## rows
Alias de **nglDBSQLiteQuery::count**  

**[int]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## toArray
Obtiene todas las filas de resultados en forma de array bidimensional utilizando **SQLite3Result::fetchArray** en modo asociativo.
Este método ignora los argumentos del objeto **nglDBSQLiteQuery** y al finalizar reinicia el conjunto de resultados a la primera fila.  

**[boolean]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

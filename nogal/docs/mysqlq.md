# mysqlq
## nglDBMySQLQuery *extends* nglStd *implements* iNglDBQuery [instanciable] [20160201]
Controla los resultados generados por consultas a la bases de datos MySQL.
  
## Variables
`private` $db = Objeto mysqli_result  
`private` $cursor = Modos de INSERT y UPDATE  

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**column**|string|null|Nombre de una columna del grupo de resultados o un comodin utilizado en [getall](#getall)|
|**get_mode**|string|assoc|Tipo de modo GET. Valores admitidos:<ul><li>**assoc** =  devuelve un array indexado por el nombre de columna</li><li>**num** =  devuelve un array indexado por el número de columna, empezando por la columna 0</li><li>**both** =  devuelve un array indexado tanto por el nombre como por el número de columna empezando por la columna 0</li></ul>|
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
|Método|Descripción|
|---|---|
|[allrows](#allrows)|Retorna el número de registros del conjunto de resultados ignorando los valores LIMIT|
|[columns](#columns)|Retorna un array con los nombre de las columnas presentes en el resultado|
|[count](#count)|Devuelve el número de filas involucradas en la última consulta ejecutada|
|[destroy](#destroy)|Libera la memoria asociada con el identificador del resultado y destruye el objeto|
|[free](#free)|Libera la memoria asociada con el identificador del resultado|
|[get](#get)|Obtiene una fila de resultados en forma de array y avanza el puntero.Cuando se e...|
|[getall](#getall)|Obtiene todas las filas de resultados en forma de array bidimensional.Cuando se ...|
|[getobj](#getobj)|Obtiene una fila de resultados en forma de objeto stdClass y avanza el puntero.|
|[lastid](#lastid)|Retorna el ID de la fila de la sentencia INSERT más reciente realizada en la bas...|
|[load](#load)|Carga la ultima consulta ejecutada del driver.|
|[reset](#reset)|Reinicia el conjunto de resultados a la primera fila.|
|[rows](#rows)|Alias de nglDBMySQLQuery::count|
|[toArray](#toArray)|Obtiene todas las filas de resultados en forma de array bidimensional utilizando...|
|[GetMode](#GetMode)|Selecciona el modo de salida para los métodos get y getall|

  
&nbsp;

## allrows
> Retorna el número de registros del conjunto de resultados ignorando los valores LIMIT  

**[int]** =  *public* function ( );

&nbsp;
___
&nbsp;

## columns
> Retorna un array con los nombre de las columnas presentes en el resultado  

**[array]** =  *public* function ( );
### Ejemplos  
#### conexión especificando parámetros (sin archivo .conf)
```php
$db = $ngl("mysql")->connect();
$query = $db->query("SELECT * FROM `tablename`");
print_r($query->columns());
```
&nbsp;
___
&nbsp;

## count
> Devuelve el número de filas involucradas en la última consulta ejecutada.
> Si la consulta es de los tipos **INSERT, UPDATE, REPLACE o DELETE** devolverá la cantidad de filas afectadas.
> Si en cambio se trata de un conjunto de resultados, devolverá la cantidad de filas del mismo.  

**[int]** =  *public* function ( );

&nbsp;
___
&nbsp;

## destroy
> Libera la memoria asociada con el identificador del resultado y destruye el objeto  

**[boolean]** =  *public* function ( );

&nbsp;
___
&nbsp;

## free
> Libera la memoria asociada con el identificador del resultado  

**[boolean]** =  *public* function ( );

&nbsp;
___
&nbsp;

## get
> Obtiene una fila de resultados en forma de array y avanza el puntero.
> Cuando se especifique **\$sColumn** y el valor sea el nombre una columna del grupo de resultados, se retornará unicamente el valor de dicha columna  

**[mixed]** =  *public* function ( *string* \$sColumn, *string* \$sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sColumn**|string|*arg::column*|Nombre de una columna del grupo de resultados|
|**\$sMode**|string|*arg::get_mode*|Tipo de modo GET. Valores admitidos:<ul><li>**assoc** =  devuelve un array indexado por el nombre de columna</li><li>**num** =  devuelve un array indexado por el número de columna, empezando por la columna 0</li><li>**both** =  devuelve un array indexado tanto por el nombre como por el número de columna empezando por la columna 0</li></ul>|
### Ejemplos  
#### todas las columnas  
```php
$users = $ngl("mysql")->connect()->query("SELECT * FROM `users`");
print_r($users->get());
```
#### sólo la columna username  
```php
$users = $ngl("mysql")->connect()->query("SELECT * FROM `users`");
echo $users->get("username");
```

&nbsp;
___
&nbsp;

## getall
> Obtiene todas las filas de resultados en forma de array bidimensional o vector.
> Si se especifica un valor para $mColumn, el conjunto de resultados puede ser agrupado de 4 maneras diferentes:
> - **nombre_columna** en este caso se retornará un vector númerico donde el valor de cada índice será el valor de la columna.
> - **#nombre_columna** en este caso se retornará un array multidimensional donde el valor de la columna será utilizado como indice asociativo del primer nivel. En un segundo nivel se agruparan los registros que tengan igual valor en el campo **\$sColumn**.
> - **@nombre_columna** retornará un vector asociativo CLAVE-VALOR. Para ello se explotará el valor de column por una punto y coma (;), el primer indice será utilizado como indice del vector, y el segundo como valor. Si no se encontrase una coma se usará el valor de column para ambas partes. En el caso de que hubiera mas de un valor para un indice, sólo se tendrá en cuenta la primer aparción del mismo.
> - **Cuando column sea un Array** será utilizado como estructura de agrupamiento de [fn::arrayGroup](https://github.com/arielbottero/wiki/blob/master/nogal/docs/fn.md#arraygroup)
> Este método reinicia el conjunto de resultados a la primera fila.  

**[mixed]** =  *public* function ( *string* \$mColumn, *string* \$sMode, *array* \$aGroup );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mColumn**|string|*arg::column*|Nombre de una columna del grupo de resultados|
|**\$sMode**|string|*arg::get_mode*|Tipo de modo GET. Valores admitidos:<ul><li>**assoc** =  devuelve un array indexado por el nombre de columna</li><li>**num** =  devuelve un array indexado por el número de columna, empezando por la columna 0</li><li>**both** =  devuelve un array indexado tanto por el nombre como por el número de columna empezando por la columna 0</li></ul>|
### Ejemplos  
#### simple  
```php
$foo = $ngl("mysql")->connect();
$bar = $foo->query("SELECT * FROM `users`");

print_r($bar->getall());

# salida
Array (
    0 => Array(id => 1, name => Juan, age => 36),
    1 => Array(id => 2, name => Pedro, age => 28),
    2 => Array(id => 3, name => Manuel, age => 36)
);
```
#### valor de un campo específico  
```php
$foo = $ngl("mysql")->connect();
$bar = $foo->query("SELECT * FROM `users`");

print_r($bar->getall("name"));

# retorna solamente los valores del campo seleccionado
Array (
    0 => Juan,
    1 => Pedro,
    2 => Manuel
);
```
#### agrupado por campo  
```php
$foo = $ngl("mysql")->connect();
$bar = $foo->query("SELECT * FROM `users`");

print_r($bar->getall("#age"));

# agrupa todos los registros con igual valor en el campo #.. en varios arrays
Array (
    36 => Array(
        0 => Array(id => 1, name => Juan, age => 36),
        1 => Array(id => 3, name => Manuel, age => 36)
    ),
    
    28 => Array(id => 2, name => Pedro, age => 28)
);
```
#### vector de datos
```php
$foo = $ngl("mysql")->connect();
$bar = $foo->query("SELECT * FROM `users`");

print_r($bar->getall("@id;name"));

# retorna un array clave-valor
Array (
    36 => Juan
    28 => Pedro
);
```
#### agrupado especiales pasando como argumento un array
Ver [fn::arrayGroup](https://github.com/arielbottero/wiki/blob/master/nogal/docs/fn.md#arraygroup)

&nbsp;
___
&nbsp;

## getobj
> Obtiene una fila de resultados en forma de objeto stdClass y avanza el puntero.  

**[object]** =  *public* function ( );
  
### Ejemplos  
#### objeto de datos  
```php
$foo = $ngl("mysql")->connect();
$bar = $foo->query("SELECT * FROM `users`");
var_dump($bar->getobj());
```

&nbsp;
___
&nbsp;

## lastid
> Retorna el ID de la fila de la sentencia INSERT más reciente realizada en la base de datos  

**[int]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## load
> Carga la ultima consulta ejecutada del driver.  

**[boolean]** =  *public* function ( *resource* \$link, *object* \$query, *string* \$sQuery, *int* \$nQueryTime );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$link**|resource||*arg::link*|Puntero del driver de base de datos|
|**\$query**|object||*arg::query*|Resultado de la última consulta ejecutada|
|**\$sQuery**|string||*arg::sentence*|Última consulta ejecutada|
|**\$nQueryTime**|int||*arg::query_time*|Tiempo que tomó la última consulta ejecutada|

&nbsp;
___
&nbsp;

## reset
> Reinicia el conjunto de resultados a la primera fila.  

**[boolean]** =  *public* function ( );
  
&nbsp;
___
&nbsp;

## rows
Alias de [count](#count)

**[int]** =  *public* function ( );

&nbsp;
___
&nbsp;

## toArray
> Una versión simplificada de [getall](#getall)
> Obtiene todas las filas de resultados en forma de array bidimensional utilizando [mysqli_result::fetch_array](http://php.net/manual/es/mysqli-result.fetch-array.php) en modo asociativo.
> Este método ignora los argumentos del objeto **nglDBMySQLQuery** y al finalizar reinicia el conjunto de resultados a la primera fila.  

**[boolean]** =  *public* function ( );

&nbsp;
___
&nbsp;

# Privados
## GetMode
> Selecciona el modo de salida para los métodos [get](#get) y [getall](#getall)

**[int]** =  *protected* function ( *string* \$sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sMode**|string|assoc|Tipo de modo GET. Valores admitidos:<ul><li>**assoc** =  devuelve un array indexado por el nombre de columna</li><li>**num** =  devuelve un array indexado por el número de columna, empezando por la columna 0</li><li>**both** =  devuelve un array indexado tanto por el nombre como por el número de columna empezando por la columna 0</li></ul>|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
# mysql
Gestor de conexiones con bases de datos MySQL  
Las consultas SQL se ejecutan a través del método [query](#query), que en caso de exito retornarán un objeto [mysqlq](mysqlq.md)
___

## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|$link|private|Puntero MySQL|
|$vModes|private|Modos de INSERT y UPDATE|

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**autoconn**|boolean|false|Cuando es TRUE, ejecuta el método connect luego de crear el objeto. Sólo usar en TRUE cuando se utilicen archivos .conf|
|**base**|string|null|Nombre de la base de datos|
|**charset**|string|utf8|Juego de caracteres con el que se ejecutarán las consultas y con el que serán tratados los archivos **file**|
|**check_colnames**|boolean|true|Activa el chequeo de los nombre de las columnas de la tabla en los métodos [insert](#insert) y [update](#update)|
|**debug**|boolean|false|Cuando es TRUE los métodos retorna la sentencia SQL en lugar de ejecutarla|
|**do**|boolean|false|Cuando es TRUE el método query ejecuta la sentencia pero no retorna resultado|
|**engine**|string|MyISAM|Motor de base de datos utilizado para crear tablas en el método [import](#import)|
|**error_description**|boolean|false|Ante un error mostrará la descripción del mismo|
|**error_query**|boolean|false|Ante un error mostrará la consulta que le dió origen|
|**file**|string|null|Ruta del archivo utilizado en los métodos [import](#import) y [update](#export)|
|**file_eol**|string|\n|Ruta del archivo utilizado en los métodos [import](#import) y [export](#export)|
|**file_local**|boolean|true|Determina si debe utilizarse la palabra clave **LOCAL** en la sentencia ejecutada por [import](#import)|
|**file_separator**|string|\t|Delimitador de campos en los archivos **file**|
|**host**|string|127.0.0.1|Puede ser o un nombre de host o una dirección IP. Pasando el valor NULL o la cadena "localhost" se asumirá el host local|
|**insert_mode**|string|INSERT|tipo de modo INSERT. Valores admitidos:<ul><li>**INSERT** =  inserta nuevos registros</li><li>**REPLACE** =  si el nuevo registro duplica un valor PRIMARY KEY o UNIQUE, el antiguo registro es eliminado</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecución</li></ul>|
|**jsql**|mixed|null|Sentencia SQL en formato JSON o Array:<ul><li>columns</li><li>tables</li><li>where</li><li>group</li><li>having</li><li>order</li><li>offset</li><li>limit</li></ul>|
|**jsql_eol**|string||Salto de linea luego de cada parte de la sentencia|
|**pass**|string||Contraseña para acceder a la base de datos|
|**port**|int|null|Puerto donde escucha el servidor MySQL|
|**socket**|string|null|Especifica el socket o la tubería con nombre que debería usarse|
|**sql**|string|null|Sentencia SQL|
|**table**|string|null|Nombre de la tabla activa en los métodos [insert](#insert) y [update](#update)|
|**update_mode**|string|UPDATE|Tipo de modo UPDATE. Valores admitidos:<ul><li>**UPDATE** =  actualiza los registros especificados</li><li>**REPLACE** =  crea un nuevo registro en caso de no hallar el registro especificados</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecución</li></ul>|
|**user**|string|root|Nombre de usuario para acceder a la base de datos|
|**values**|string|null|Datos enviados a los métodos [insert](#insert) y [update](#update). Valores admitidos:<ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor será analizado utilizando [parse_str](http://php.net/parse_str)</li></ul>|
|**where**|string|null|Cadena que representa una condición SQL WHERE|

## Atributos
|Atributo|Tipo|Descripción|
|---|---|---|
|**last_query**|string|Ultima consulta SQL ejecutada|

&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[close](#close)|Finaliza la conexión con la base de datos|
|[connect](#connect)|Establece la conexión con la base de datos utilizando un objeto [mysqli](http://php.net/mysqli)|
|[destroy](#destroy)|Cierra la conexión y destruye el objeto|
|[escape](#escape)|Escapa un valor para ser incluído de manera segura en una sentencia SQL|
|[exec](#exec)|Ejecuta una sentencia SQL y retorna un objecto [mysqli_result](http://php.net/mysqli_result)|
|[export](#export)|Intenta exportar el contenido de la consulta SQL a un archivo, utilizando la sentencia **INTO OUTFILE**|
|[import](#import)|Intenta importar el contenido de un archivo en una tabla utilizando la sentencia **LOAD DATA LOCAL INFILE**|
|[insert](#insert)|Inserta un nuevo registro en una tabla|
|[jsqlParser](#jsqlParser)|Convierte una sentencia JSQL y retorna una sentencia SQL|
|[mexec](#mexec)|Ejecuta varias sentencias SQL separadas por ; y retorna un array de objectos [mysqli_result](http://php.net/mysqli_result)|
|[mquery](#mquery)|Ejecuta varias sentencias SQL separadas por ; y retorna un array de objectos[nglDBMySQLQuery](mysqlq.md) o TRUE cuando DO esta activo|
|[query](#query)|Ejecuta una sentencia SQL y retorna un objecto [nglDBMySQLQuery](mysqlq.md)|
|[update](#update)|Actualiza todos los registros que cumplan con la condición $sWhere|
|Internos||
|[Error](#Error)|Muestra el mensaje de Error generado por el fallo más reciente|
|[PrepareValues](#PrepareValues)|Auxiliar de los métodos [insert](#insert) y [update](#update). Prepara el array asociativo o la cadena ...|
  
&nbsp;

## close
> Finaliza la conexión con la base de datos  

**[boolean]** =  *public* function ( );
 
&nbsp;
___
&nbsp;

## connect
> Establece la conexión con la base de datos  

**[$this]** =  *public* function ( *string* $sHost, *string* $sUser, *string* $sPass, *int* $nPort, *string* $sSocket );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sHost**|string|*arg::host*|Puede ser o un nombre de host o una dirección IP. Pasando el valor NULL o la cadena "localhost" se asumirá el host local|
|**$sUser**|string|*arg::user*|Nombre de usuario para acceder a la base de datos|
|**$sPass**|string|*arg::pass*|Contraseña para acceder a la base de datos|
|**$nPort**|string|*arg::port*|Puerto donde escucha el servidor MySQL|
|**$sSocket**|string|*arg::socket*|Contraseña para acceder a la base de datos|

### Ejemplos  
#### conexión especificando parámetros (sin archivo .conf)
```php
$db = $ngl("mysql")
	->host("localhost")
	->user("root")
	->pass("asd123")
	->base("test")
	->connect()
;
```

&nbsp;
___
&nbsp;

## destroy
> Cierra la conexión y destruye el objeto  

**[boolean]** =  *public* function ( );

&nbsp;
___
&nbsp;

## escape
> Escapa un valor para ser incluído de manera segura en una sentencia SQL  

**[mixed]** =  *public* function ( *mixed* $mValues );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mValues**|string|*arg::values*|Datos enviados a los métodos [insert](#insert) y [update](#update). Valores admitidos:<ul><li>**array**</li><li>**number**</li><li>**string**</li></ul>|

&nbsp;
___
&nbsp;

## exec
> Ejecuta una sentencia SQL y retorna un objecto [mysqli_result](http://php.net/mysqli_result)  

**object** =  *public* function ( *string* $sQuery );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sQuery**|string|*arg::sql*|Sentencia SQL|

### Ejemplos  
#### ejecución 
```php
$query = $ngl("mysql")->exec("SELECT * FROM `tablename` WHERE `field` LIKE 'foo%'");
```

&nbsp;
___
&nbsp;

## export
> Intenta exportar el contenido de la consulta SQL SELECT a un archivo, utilizando la sentencia **INTO OUTFILE**
> El método retorna **false** en caso de error o la ruta del archivo exportado en caso de éxito
> A la hora de exportar son utilizados los siguientes parámetros:
> - separador de campos: *arg::file_separator*
> - separador de registros: *arg::file_eol*
> - juego de caracteres: *arg::charset*
> - datos encerrados entre: **"** *(comillas dobles)*
> - caracter de escape: **\\** *(barra invertida)*

Por seguridad la ruta del archivo **$sFilePath** está alcanzada por la directiva [SANDBOX](file.md)

**string** =  *public* function ( *string* $sQuery, *string* $sFilePath );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sQuery**|string|*arg::sql*|Sentencia SQL|
|**$sFilePath**|string|*arg::file*|Ruta del archivo de salida de datos. Si no se especifica se generará un archivo en **tmp**|

### Ejemplos  
#### todas las ventas de enero
```php
$foo = $ngl("mysql");
$foo->connect();
$foo->export("SELECT * FROM `ventas` WHERE MONTH(`date`) = '01'");
```

&nbsp;
___
&nbsp;

## import
> Intenta importar el contenido de un archivo en una tabla utilizando la sentencia **LOAD DATA LOCAL INFILE**
> A la hora de importar son utilizados los siguientes parámetros:
> - uso de LOCAL: *arg::file_local*
> - separador de campos: *arg::file_separator*
> - separador de registros: *arg::file_eol*
> - juego de caracteres: *arg::charset*
> - datos encerrados entre: **"** *(comillas dobles)*
> - caracter de escape: **\\** *(barra invertida)*

Por seguridad la ruta del archivo **$sFilePath** está alcanzada por la directiva [SANDBOX](file.md)

**boolean** =  *public* function ( *string* $sFilePath, *string* $sTable );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sFilePath**|string|*arg::file*|Ruta del archivo de datos a importar|
|**$sTable**|string|*arg::table*|Tabla en la que se importarán los datos|

### Ejemplos  
#### carga de ventas de febrero 
```php
$foo = $ngl("mysql");
$foo->connect();
$foo->import("/tmp/febrero.csv", "ventas");
```

&nbsp;
___
&nbsp;

## insert
> Inserta un nuevo registro en una tabla  

**[nglDBMySQLQuery object]** =  *public* function ( *string* $sTable, *string* $mValues, *string* $sMode, *boolean* $bCheckColumns, *boolean* $bDO );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTable**|string|*arg::table*|Nombre de la tabla activa en los métodos INSERT y UPDATE|
|**$mValues**|string|*arg::values*|Datos a insertar: <ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor será analizado utilizando **parse_str**</li></ul>|
|**$sMode**|string|*arg::insert_mode*|tipo de modo INSERT. Valores admitidos:<ul><li>**INSERT** =  inserta nuevos registros</li><li>**REPLACE** =  si el nuevo registro duplica un valor PRIMARY KEY o UNIQUE, el antiguo registro es eliminado</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecución</li></ul>|
|**$bCheckColumns**|boolean|*arg::check_colnames*|Activa el chequeo de los nombre de las columnas en la tabla activa|
|**$bDO**|boolean|*arg::do*|Cuando es TRUE el método query ejecuta la sentencia pero no retorna resultado|
### Ejemplos  
#### datos en array  
```php
$foo = $ngl("mysql");
$foo->base = "test";
$foo->connect();

$data = array("foo"=>"foovalue", "bar"=>"barvalue");
$foo->insert("tablename", $data);
```
#### datos como cadena de variables  
```php
$foo = $ngl("mysql");
$foo->base = "test";
$foo->connect();

$data="foobar&bar=barvalue"
$foo->insert("tablename", $data, "replace");
```

&nbsp;
___
&nbsp;

## jsqlParser
> Convierte una cadena JSQL una sentencia SQL utilizando el objeto [jsql](jsql.md)
> Este método es utilizado por lo general por otros objetos, como por ejemplo [owl](owl.md)

**[string]** =  *public* function ( *mixed* $mJSQL, *string* $sEOL );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mJSQL**|mixed|*arg::jsql*|Sentencia SQL en formato JSON o Array:<ul><li>columns</li><li>tables</li><li>where</li><li>group</li><li>having</li><li>order</li><li>offset</li><li>limit</li></ul>|
|**$sEOL**|string|*arg::jsql_eol*|Salto de linea luego de cada parte de la sentencia|

&nbsp;
___
&nbsp;

## mexec
> Ejecuta varias sentencias SQL separadas por ; y retorna un array de objectos **mysqli_result**  

**[array]** =  *public* function ( *string* $sQuery );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sQuery**|string|*arg::sql*|Ultima sentencia SQL ejecutada o próxima a ejecutarse|

&nbsp;
___
&nbsp;

## mquery
> Ejecuta varias sentencias SQL separadas por ; y retorna un array de objectos **nglDBMySQLQuery**, o TRUE cuando DO esta activo  

**[array OR true]** =  *public* function ( *string* $sQuery, *boolean* $bDO );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sQuery**|string|null|Ultima sentencia SQL ejecutada o próxima a ejecutarse|
|**$bDO**|boolean|false|Cuando es TRUE el método query ejecuta la sentencia pero no retorna resultado|
### Ejemplos  
#### datos en array  
```php
$foo = $ngl("mysql");
$foo->base = "test";
$foo->connect();

$foo->mquery("
DELETE FROM `contactos` WHERE `nombre` LIKE '%Castillo%';
INSERT INTO `contactos` (id, nombre, email) VALUES(null, 'Alberto Castillo', 'acastillo@foobar.com');
");
```

&nbsp;
___
&nbsp;

## query
> Ejecuta una sentencia SQL y retorna un objecto **nglDBMySQLQuery**  

**[nglDBMySQLQuery object]** =  *public* function ( *string* $sQuery, *boolean* $bDO );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sQuery**|string|*arg::sql*|Sentencia SQL próxima a ejecutarse|
|**$bDO**|boolean|*arg::do*|Cuando es TRUE el método query ejecuta la sentencia pero no retorna resultado|
### Ejemplos  
#### conexión  
```php
$foo = $ngl("mysql");
$foo->base = "test";
$foo->connect();
$bar = $foo->query("SELECT * FROM `users`");
```

&nbsp;
___
&nbsp;

## update
> Actualiza todos los registros que cumplan con la condición **$sWhere**  

**[nglDBMySQLQuery object]** =  *public* function ( *string* $sTable, *string* $mValues, *string* $sWhere, *string* $sMode, *boolean* $bCheckColumns, *boolean* $bDO );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTable**|string|*arg::table*|Nombre de la tabla activa|
|**$mValues**|string|*arg::values*|Datos a actualizar. Valores admitidos:<ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor será analizado utilizando **parse_str**</li></ul>|
|**$sWhere**|string|nu*arg::where*ll|Cadena que representa una condición SQL WHERE|
|**$sMode**|string|*arg::update_mode*|Tipo de modo UPDATE. Valores admitidos:<ul><li>**UPDATE** =  actualiza los registros especificados</li><li>**REPLACE** =  crea un nuevo registro en caso de no hallar el registro especificados</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecución</li></ul>|
|**$bCheckColumns**|boolean|*arg::check_colnames*|Activa el chequeo de los nombre de las columnas en la tabla activa|
|**$bDO**|boolean|*arg::do*|Cuando es TRUE el método query ejecuta la sentencia pero no retorna resultado|
### Ejemplos  
#### datos en array  
```php
$foo = $ngl("mysql");
$foo->base = "test";
$foo->connect();

$data = array("foo"=>"foovalue", "bar"=>"barvalue");
$foo->update("tablename", $data, "`id`='7'");
```
#### datos como cadena de variables  
```php
$foo = $ngl("mysql.foobar");
$foo->base = "test";
$foo->connect();

$data="foobar&bar=barvalue"
$foo->update("tablename", $data, "`id`='7'", "ignore");
```

&nbsp;
___
&nbsp;

# Internos
## Error
> Muestra el mensaje de Error generado por el fallo más reciente  

**[mixed]** =  *private* function ( );

&nbsp;
___
&nbsp;

## PrepareValues
> Auxiliar de los métodos [insert](#insert) y [update](#update).
> Prepara el **array asociativo** o la **cadena de variables** para ser utilizados en las sentencias.
> Cuando los valores sean pasados como una **cadena de variables** estos serán tratados con **escape** para garantizar la seguridad del comando SQL.  

**[mysqli_result object]** =  *private* function ( *string* $sType, *string* $sTable, *mixed* $mValues, *boolean* $bCheckColumns );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sType**|string||Tipo de operación, INSERT o UPDATE|
|**$sTable**|string||Nombre de la tabla|
|**$mValues**|mixed||Datos en forma de array asociativo o cadena de variables|
|**$bCheckColumns**|boolean|true|Activa el chequeo de columnas en la tabla|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2021 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br /> 
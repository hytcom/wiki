# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# sqlite
## nglDBSQLite *extends* nglStd [instanciable] [20160201]
Gestiona conexciones con bases de datos SQLite.
  
## Variables
`private` $link = Puntero  
`private` $vModes = Modos de INSERT y UPDATE  

## Argumentos
|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**autoconn**|boolean|false|Cuando es TRUE, ejecuta el m茅todo connect luego de crear el objeto. S贸lo usar en TRUE cuando se utilicen archivos .conf|
|**base**|string|null|Ruta del archivo de base de datos|
|**check_colnames**|boolean|true|Activa el chequeo de los nombre de las columnas en la tabla activa|
|**debug**|boolean|false|Cuando es TRUE 茅l m茅todo retorna la sentencia SQL en lugar de ejecutarla|
|**do**|boolean|false|Cuando es TRUE el m茅todo query ejecuta la sentencia pero no retorna resultado|
|**error_description**|boolean|false|Ante un error mostrar谩 la descripci贸n del mismo|
|**error_query**|boolean|false|Ante un error mostrar谩 la consulta que le di贸 origen|
|**flags**|string|SQLITE3_OPEN_READWRITE | SQLITE3_OPEN_CREATE|Banderas opcionales para determinar c贸mo abrir la base de datos SQLite|
|**insert_mode**|string|INSERT|Tipo de modo INSERT. Valores admitidos:<ul><li>**INSERT** =  inserta nuevos registros</li><li>**REPLACE** =  si el nuevo registro duplica un valor PRIMARY KEY o UNIQUE, el antiguo registro es eliminado</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecuci贸n</li></ul>|
|**jsql**|mixed|null|Sentencia SQL en formato JSON o Array:<ul><li>columns</li><li>tables</li><li>where</li><li>group</li><li>having</li><li>order</li><li>offset</li><li>limit</li></ul>|
|**jsql_eol**|string||Salto de linea luego de cada parte de la sentencia|
|**pass**|string|null|Clave de encriptaci贸n opcional usada cuando se encripta o desencripta una base de datos|
|**sql**|string|null|Ultima sentencia SQL ejecutada o pr贸xima a ejecutarse|
|**table**|string|null|Nombre de la tabla activa en los m茅todos INSERT y UPDATE|
|**update_mode**|string|UPDATE|Tipo de modo UPDATE. Valores admitidos:<ul><li>**UPDATE** =  actualiza los registros especificados</li><li>**REPLACE** =  crea un nuevo registro en caso de no hallar el registro especificados</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecuci贸n</li></ul>|
|**values**|string|null|Datos enviados a los m茅todos INSERT y UPDATE. Valores admitidos:<ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor ser谩 analizado utilizando **parse_str**</li></ul>|
|**where**|string|null|Cadena que representa una condici贸n SQL WHERE|

  
&nbsp;

# M茅todos
|M茅todo|Descripci贸n|
|---|---|
|[Error](#Error)|Muestra el mensaje de Error generado por el fallo m醩 reciente|
|[PrepareValues](#PrepareValues)|Auxiliar de los m閠odos insert y update.Prepara el array asociativo o la cadena de variables para ser utilizados en las sentencias.Cuando los valores...|
|[close](#close)|Finaliza la conexi髇 con la base de datos|
|[connect](#connect)|Establece la conexi髇 con la base de datos|
|[destroy](#destroy)|Cierra la conexi髇 y destruye el objeto|
|[escape](#escape)|Escapa un valor para ser inclu韉o de manera segura en una sentencia SQL|
|[exec](#exec)|Ejecuta una sentencia SQL y retorna un objecto SQLite3Result|
|[insert](#insert)|Inserta un nuevo registro en una tabla|
|[jsqlParser](#jsqlParser)|Convierte una sentencia JSQL y retorna una sentencia SQL|
|[mexec](#mexec)|Ejecuta varias sentencias SQL separadas por ; y retorna un array de objectos SQLite3Result|
|[mquery](#mquery)|Ejecuta varias sentencias SQL separadas por ; y retorna un array de objectos nglDBSQLiteQuery, o TRUE cuando DO esta activo|
|[query](#query)|Ejecuta una sentencia SQL y retorna un objecto nglDBSQLiteQuery|
|[update](#update)|Actualiza todos los registros que cumplan con la condici髇 $sWhere|

  
&nbsp;


## close
Finaliza la conexi贸n con la base de datos  

**[boolean]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## connect
Establece la conexi贸n con la base de datos  

**[$this]** =  *public* function ( *string* \$sBase, *string* \$sPass, *string* \$nFlags );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sBase**|string|null|Ruta del archivo de base de datos|
|**\$sPass**|string|null|Clave de encriptaci贸n opcional usada cuando se encripta o desencripta una base de datos|
|**\$nFlags**|string|SQLITE3_OPEN_READWRITE | SQLITE3_OPEN_CREATE|Banderas opcionales para determinar c贸mo abrir la base de datos SQLite|

&nbsp;
___
&nbsp;

## destroy
Cierra la conexi贸n y destruye el objeto  

**[boolean]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## Error
Muestra el mensaje de Error generado por el fallo m谩s reciente  

**[mixed]** =  *private* function ( );
  

&nbsp;
___
&nbsp;

## escape
Escapa un valor para ser inclu铆do de manera segura en una sentencia SQL  

**[mixed]** =  *public* function ( *string* \$mValues );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$mValues**|string|null|Datos enviados a los m茅todos INSERT y UPDATE. Valores admitidos:<ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor ser谩 analizado utilizando **parse_str**</li></ul>|

&nbsp;
___
&nbsp;

## exec
Ejecuta una sentencia SQL y retorna un objecto **SQLite3Result**  

**[SQLite3Result object]** =  *public* function ( *string* \$sQuery );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sQuery**|string|null|Ultima sentencia SQL ejecutada o pr贸xima a ejecutarse|

&nbsp;
___
&nbsp;

## jsqlParser
Convierte una sentencia JSQL y retorna una sentencia SQL  

**[string]** =  *public* function ( *mixed* \$mJSQL, *string* \$sEOL );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$mJSQL**|mixed|null|Sentencia SQL en formato JSON o Array:<ul><li>columns</li><li>tables</li><li>where</li><li>group</li><li>having</li><li>order</li><li>offset</li><li>limit</li></ul>|
|**\$sEOL**|string||Salto de linea luego de cada parte de la sentencia|

&nbsp;
___
&nbsp;

## mexec
Ejecuta varias sentencias SQL separadas por ; y retorna un array de objectos **SQLite3Result**  

**[array]** =  *public* function ( *string* \$sQuery );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sQuery**|string|null|Ultima sentencia SQL ejecutada o pr贸xima a ejecutarse|

&nbsp;
___
&nbsp;

## mquery
Ejecuta varias sentencias SQL separadas por ; y retorna un array de objectos **nglDBSQLiteQuery**, o TRUE cuando DO esta activo  

**[array]** =  *public* function ( *string* \$sQuery, *boolean* \$bDO );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sQuery**|string|null|Ultima sentencia SQL ejecutada o pr贸xima a ejecutarse|
|**\$bDO**|boolean|false|Cuando es TRUE el m茅todo query ejecuta la sentencia pero no retorna resultado|

&nbsp;
___
&nbsp;

## insert
Inserta un nuevo registro en una tabla  

**[SQLite3Result object]** =  *public* function ( *string* \$sTable, *string* \$mValues, *string* \$sMode );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sTable**|string|null|Nombre de la tabla activa en los m茅todos INSERT y UPDATE|
|**\$mValues**|string|null|Datos enviados a los m茅todos INSERT y UPDATE. Valores admitidos:<ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor ser谩 analizado utilizando **parse_str**</li></ul>|
|**\$sMode**|string|INSERT|Tipo de modo INSERT. Valores admitidos:<ul><li>**INSERT** =  inserta nuevos registros</li><li>**REPLACE** =  si el nuevo registro duplica un valor PRIMARY KEY o UNIQUE, el antiguo registro es eliminado</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecuci贸n</li></ul>|
### Ejemplos  
#### datos en array  
```php
$foo = $ngl("sqlite.foobar");
$foo->base = "shop.sqlite";
$foo->connect();

$data = array("foo"=>"foovalue", "bar"=>"barvalue");
$foo->insert("tablename", $data);
```
#### datos como cadena de variables  
```php
$foo = $ngl("sqlite.foobar");
$foo->base = "shop.sqlite";
$foo->connect();

$data="foobar&bar=barvalue"
$foo->insert("tablename", $data, "replace");
```

&nbsp;
___
&nbsp;

## PrepareValues
Auxiliar de los m茅todos **insert** y **update**.
Prepara el **array asociativo** o la **cadena de variables** para ser utilizados en las sentencias.
Cuando los valores sean pasados como una **cadena de variables** estos ser谩n tratados con **escape** para garantizar la seguridad del comando SQL.  

**[SQLite3Result object]** =  *private* function ( *string* \$sType, *string* \$sTable, *mixed* \$mValues, *boolean* \$bCheckColumns );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sType**|string||Tipo de operaci贸n, INSERT o UPDATE|
|**\$sTable**|string||Nombre de la tabla|
|**\$mValues**|mixed||Datos en forma de array asociativo o cadena de variables|
|**\$bCheckColumns**|boolean|true|Activa el chequeo de columnas en la tabla|

&nbsp;
___
&nbsp;

## query
Ejecuta una sentencia SQL y retorna un objecto **nglDBSQLiteQuery**  

**[nglDBSQLiteQuery object]** =  *public* function ( *string* \$sQuery, *boolean* \$bDO );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sQuery**|string|null|Ultima sentencia SQL ejecutada o pr贸xima a ejecutarse|
|**\$bDO**|boolean|false|Cuando es TRUE el m茅todo query ejecuta la sentencia pero no retorna resultado|
### Ejemplos  
#### conexi贸n  
```php
$foo = $ngl("sqlite.foobar");
$foo->base = "shop.sqlite";
$foo->connect();
$bar = $foo->query("SELECT * FROM `users`");
```

&nbsp;
___
&nbsp;

## update
Actualiza todos los registros que cumplan con la condici贸n **\$sWhere**  

**[SQLite3Result object]** =  *public* function ( *string* \$sTable, *string* \$mValues, *string* \$sWhere, *string* \$sMode );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sTable**|string|null|Nombre de la tabla activa en los m茅todos INSERT y UPDATE|
|**\$mValues**|string|null|Datos enviados a los m茅todos INSERT y UPDATE. Valores admitidos:<ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor ser谩 analizado utilizando **parse_str**</li></ul>|
|**\$sWhere**|string|null|Cadena que representa una condici贸n SQL WHERE|
|**\$sMode**|string|UPDATE|Tipo de modo UPDATE. Valores admitidos:<ul><li>**UPDATE** =  actualiza los registros especificados</li><li>**REPLACE** =  crea un nuevo registro en caso de no hallar el registro especificados</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecuci贸n</li></ul>|
### Ejemplos  
#### datos en array  
```php
$foo = $ngl("sqlite.foobar");
$foo->base = "shop.sqlite";
$foo->connect();

$data = array("foo"=>"foovalue", "bar"=>"barvalue");
$foo->update("tablename", $data, "`id`='7'");
```
#### datos como cadena de variables  
```php
$foo = $ngl("sqlite.foobar");
$foo->base = "shop.sqlite";
$foo->connect();

$data="foobar&bar=barvalue"
$foo->update("tablename", $data, "`id`='7'", "ignore");
```

&nbsp;
___
&nbsp;

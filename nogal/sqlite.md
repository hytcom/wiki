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
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**autoconn**|boolean|false|Cuando es TRUE, ejecuta el método connect luego de crear el objeto. Sólo usar en TRUE cuando se utilicen archivos .conf|
|**base**|string|null|Ruta del archivo de base de datos|
|**check_colnames**|boolean|true|Activa el chequeo de los nombre de las columnas en la tabla activa|
|**debug**|boolean|false|Cuando es TRUE él método retorna la sentencia SQL en lugar de ejecutarla|
|**do**|boolean|false|Cuando es TRUE el método query ejecuta la sentencia pero no retorna resultado|
|**error_description**|boolean|false|Ante un error mostrará la descripción del mismo|
|**error_query**|boolean|false|Ante un error mostrará la consulta que le dió origen|
|**flags**|string|SQLITE3_OPEN_READWRITE | SQLITE3_OPEN_CREATE|Banderas opcionales para determinar cómo abrir la base de datos SQLite|
|**insert_mode**|string|INSERT|Tipo de modo INSERT. Valores admitidos:<ul><li>**INSERT** =  inserta nuevos registros</li><li>**REPLACE** =  si el nuevo registro duplica un valor PRIMARY KEY o UNIQUE, el antiguo registro es eliminado</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecución</li></ul>|
|**jsql**|mixed|null|Sentencia SQL en formato JSON o Array:<ul><li>columns</li><li>tables</li><li>where</li><li>group</li><li>having</li><li>order</li><li>offset</li><li>limit</li></ul>|
|**jsql_eol**|string||Salto de linea luego de cada parte de la sentencia|
|**pass**|string|null|Clave de encriptación opcional usada cuando se encripta o desencripta una base de datos|
|**sql**|string|null|Ultima sentencia SQL ejecutada o próxima a ejecutarse|
|**table**|string|null|Nombre de la tabla activa en los métodos INSERT y UPDATE|
|**update_mode**|string|UPDATE|Tipo de modo UPDATE. Valores admitidos:<ul><li>**UPDATE** =  actualiza los registros especificados</li><li>**REPLACE** =  crea un nuevo registro en caso de no hallar el registro especificados</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecución</li></ul>|
|**values**|string|null|Datos enviados a los métodos INSERT y UPDATE. Valores admitidos:<ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor será analizado utilizando **parse_str**</li></ul>|
|**where**|string|null|Cadena que representa una condición SQL WHERE|

  
&nbsp;

# Métodos
- [Error](#Error)
- [PrepareValues](#PrepareValues)
- [close](#close)
- [connect](#connect)
- [destroy](#destroy)
- [escape](#escape)
- [exec](#exec)
- [insert](#insert)
- [jsqlParser](#jsqlParser)
- [mexec](#mexec)
- [mquery](#mquery)
- [query](#query)
- [update](#update)

  
&nbsp;


## close
Finaliza la conexión con la base de datos  

**[boolean]** =  *public* function ( );
  
&nbsp;
___
&nbsp;

## connect
Establece la conexión con la base de datos  

**[$this]** =  *public* function ( *string* \$sBase, *string* \$sPass, *string* \$nFlags );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sBase**|string|null|Ruta del archivo de base de datos|
|**\$sPass**|string|null|Clave de encriptación opcional usada cuando se encripta o desencripta una base de datos|
|**\$nFlags**|string|SQLITE3_OPEN_READWRITE | SQLITE3_OPEN_CREATE|Banderas opcionales para determinar cómo abrir la base de datos SQLite|
&nbsp;
___
&nbsp;

## destroy
Cierra la conexión y destruye el objeto  

**[boolean]** =  *public* function ( );
  
&nbsp;
___
&nbsp;

## Error
Muestra el mensaje de Error generado por el fallo más reciente  

**[mixed]** =  *private* function ( );
  
&nbsp;
___
&nbsp;

## escape
Escapa un valor para ser incluído de manera segura en una sentencia SQL  

**[mixed]** =  *public* function ( *string* \$mValues );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mValues**|string|null|Datos enviados a los métodos INSERT y UPDATE. Valores admitidos:<ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor será analizado utilizando **parse_str**</li></ul>|
&nbsp;
___
&nbsp;

## exec
Ejecuta una sentencia SQL y retorna un objecto **SQLite3Result**  

**[SQLite3Result object]** =  *public* function ( *string* \$sQuery );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sQuery**|string|null|Ultima sentencia SQL ejecutada o próxima a ejecutarse|
&nbsp;
___
&nbsp;

## jsqlParser
Convierte una sentencia JSQL y retorna una sentencia SQL  

**[string]** =  *public* function ( *mixed* \$mJSQL, *string* \$sEOL );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mJSQL**|mixed|null|Sentencia SQL en formato JSON o Array:<ul><li>columns</li><li>tables</li><li>where</li><li>group</li><li>having</li><li>order</li><li>offset</li><li>limit</li></ul>|
|**\$sEOL**|string||Salto de linea luego de cada parte de la sentencia|
&nbsp;
___
&nbsp;

## mexec
Ejecuta varias sentencias SQL separadas por ; y retorna un array de objectos **SQLite3Result**  

**[array]** =  *public* function ( *string* \$sQuery );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sQuery**|string|null|Ultima sentencia SQL ejecutada o próxima a ejecutarse|
&nbsp;
___
&nbsp;

## mquery
Ejecuta varias sentencias SQL separadas por ; y retorna un array de objectos **nglDBSQLiteQuery**, o TRUE cuando DO esta activo  

**[array]** =  *public* function ( *string* \$sQuery, *boolean* \$bDO );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sQuery**|string|null|Ultima sentencia SQL ejecutada o próxima a ejecutarse|
|**\$bDO**|boolean|false|Cuando es TRUE el método query ejecuta la sentencia pero no retorna resultado|
&nbsp;
___
&nbsp;

## insert
Inserta un nuevo registro en una tabla  

**[SQLite3Result object]** =  *public* function ( *string* \$sTable, *string* \$mValues, *string* \$sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sTable**|string|null|Nombre de la tabla activa en los métodos INSERT y UPDATE|
|**\$mValues**|string|null|Datos enviados a los métodos INSERT y UPDATE. Valores admitidos:<ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor será analizado utilizando **parse_str**</li></ul>|
|**\$sMode**|string|INSERT|Tipo de modo INSERT. Valores admitidos:<ul><li>**INSERT** =  inserta nuevos registros</li><li>**REPLACE** =  si el nuevo registro duplica un valor PRIMARY KEY o UNIQUE, el antiguo registro es eliminado</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecución</li></ul>|
&nbsp;
___
&nbsp;

## PrepareValues
Auxiliar de los métodos **insert** y **update**.
Prepara el **array asociativo** o la **cadena de variables** para ser utilizados en las sentencias.
Cuando los valores sean pasados como una **cadena de variables** estos serán tratados con **escape** para garantizar la seguridad del comando SQL.  

**[SQLite3Result object]** =  *private* function ( *string* \$sType, *string* \$sTable, *mixed* \$mValues, *boolean* \$bCheckColumns );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sType**|string||Tipo de operación, INSERT o UPDATE|
|**\$sTable**|string||Nombre de la tabla|
|**\$mValues**|mixed||Datos en forma de array asociativo o cadena de variables|
|**\$bCheckColumns**|boolean|true|Activa el chequeo de columnas en la tabla|
&nbsp;
___
&nbsp;

## query
Ejecuta una sentencia SQL y retorna un objecto **nglDBSQLiteQuery**  

**[nglDBSQLiteQuery object]** =  *public* function ( *string* \$sQuery, *boolean* \$bDO );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sQuery**|string|null|Ultima sentencia SQL ejecutada o próxima a ejecutarse|
|**\$bDO**|boolean|false|Cuando es TRUE el método query ejecuta la sentencia pero no retorna resultado|
&nbsp;
___
&nbsp;

## update
Actualiza todos los registros que cumplan con la condición **\$sWhere**  

**[SQLite3Result object]** =  *public* function ( *string* \$sTable, *string* \$mValues, *string* \$sWhere, *string* \$sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sTable**|string|null|Nombre de la tabla activa en los métodos INSERT y UPDATE|
|**\$mValues**|string|null|Datos enviados a los métodos INSERT y UPDATE. Valores admitidos:<ul><li>**array asociativo** =  donde cada clave es el nombre del campo en la tabla</li><li>**cadena de variables** =  con el mismo formato que las pasadas por medio de una URL. El valor será analizado utilizando **parse_str**</li></ul>|
|**\$sWhere**|string|null|Cadena que representa una condición SQL WHERE|
|**\$sMode**|string|UPDATE|Tipo de modo UPDATE. Valores admitidos:<ul><li>**UPDATE** =  actualiza los registros especificados</li><li>**REPLACE** =  crea un nuevo registro en caso de no hallar el registro especificados</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecución</li></ul>|
&nbsp;
___
&nbsp;

# pecker
Pecker es una herramienta
Este objeto está alcanzado por la directiva **SANDBOX**, que es el entorno dentro del cual se establecerá el dominio del objeto, es decir, establece el directorio dentro del cual estará contenido el accionar del método, cualquier intento de leer o escribir un archivo fuera de este entorno resultará en error. Por seguridad, **SANDBOX** toma el valor de la constante [NGL_SANDBOX](constants.md#opcionales) y en caso de que ella no estuviese definida asumirá el valor de [NGL_PATH_PROJECT](constants.md#rutas)
___

  
## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**analyse_datatype**|boolean|false|Analiza los datos de la tabla para determinar normalizaciones y tipos de datos|
|**col**|string|null|Cadena formada por un nombre de tabla y un nombre de campo separados por un punto, con el cual se vincula al ID de una tabla de destino, mediante el campo **\_\_pecked\_\_** de la tabla activa|
|**col**|int|null|Número de columna seleccionada|
|**cols**|array|null|Array con números de columnas. En el método hit, las claves del array representan las columnas de la tabla principal y los valores su contraparte en la tabla secundaria|
|**cols**|array|null|Array con las relaciones entres las columnas de la tabla activa y la tabla de destino a la hora de ejecutar [fill](#fill) ó  [complete](#complete). Donde la clave es el número de columna en la tabla activa y el valor el de la tabla de destino|
|**datafile**|string|pecker|Nombre del archivo en donde se guarda la información del set actual|
|**db**|mixed|null|Objeto controlador de base de datos. Si el valor es un string, se utilizará dicho objeto con la configuración por defecto|
|**exec**|boolean|false|Actua como confirmación en algunos métodos|
|**features**|array|null|Array con los datos de la tabla de 'features' usada por el método normalize:<ul><li>**0** = indice con el nombre de la tabla</li><li>**1** = nombre del campo id</li><li>**2** = nombre del campo contra el que se hará match</li></ul>|
|**file**|string|null|Path del archivo para LOAD|
|**file_eol**|string|\r\n|Fin de línea|
|**force**|boolean|false|Determina si debe forzarse un reanalisis|
|**grouper**|array|null|Array con los números de columnas que compondrán el agrupador|
|**hashappend**|boolean|false|Determina si el valor del hash debe incluir el hash actual|
|**hittest**|mixed|false|Establece el modo de colisión: <ul><li>**test** = retorna el número de colisiones encotradas</li><li>**show** = muestra un número especifico de colisiones</li><li>**true** = ejecuta la colisión</li></ul>|
|**id**|int|null|Id del registro a mostrar en el método *show*|
|**key**|string|null|Nombre del campo de la clave primaria de la tabla actual|
|**length**|int|32|Tamaño máximo de los valores guardados en los campos **min** y **max** en el analisis|
|**limit**|int|20|Limite de registros a mostrar|
|**markas**|int|1|Valor con el que se marcarán los registros en el método **mark**, 1 o null|
|**markon**|string|\_\_pecked\_\_|Campo que deberá marcarse, **\_\_pecked\_\_** o **\_\_wano\_\_**|
|**newnames**|array|null|Array con los nuevos nombres de las columnas|
|**output**|string|print|Tipo de salida de datos:<ul><li>**print** = imprime los datos en formato ttable</li><li>**table** = retorna los datos en formato ttable</li><li>**data** = array de datos</li></ul>|
|**overwrite**|boolean|true|Determina si el método **complete** debe sobreescribir los datos|
|**policy**|array|null|Array con los métodos de sanitización que deben aplicarse sobre la columna activa:<ul><li>**digits** = elimina todo menos los digitos (0-9)</li><li>**consonants** = elimina todo menos las consonantes</li><li>**email** = elimina cualquier caracter que no sea válido en un email</li><li>**lcase** = convierte el valor a minúsculas</li><li>**left:LENGTH** = toma los LENGTH caracteres desde la izquierda</li><li>**letters** = elimina todo menos las letras (a-zA-Z)</li><li>**nospaces** = elimina todos los caracteres de espacio, tabulaciones y saltos de línea</li><li>**numbers** = elimina todo menos los números, sin importar signo ni signos de puntuación</li><li>**right:LENGTH** = toma los LENGTH caracteres desde la derecha</li><li>**trim** = elimina espacios en blanco a ambos lados</li><li>**trim:STRING** = elimina STRING a ambos lados del valor</li><li>**ucase** = convierte el valor a mayúsculas</li><li>**ucfirst** = convierte la primer letra de la cadena a mayúscula</li><li>**ucwords** = convierte la primer letra de cada palabra a mayúscula</li><li>**words** = elimina todo menos letters + digits</li></ul>|
|**rules**|array|null|Array con las reglas para la unificación de datos: <ul><li>**any** = cualquier valor que no sea vacio</li><li>**false** = no hace nada</li><li>**join** = une los valores si son diferentes</li><li>**longer** = valor de mayor longuitud</li><li>**longerjoin** = valor de mayor longuitud. Si hay 2 o más de igual longuitud, hace join</li><li>**noempty** = cualquier valor que no sea vacio ni 0</li></ul>|
|**skip**|boolean|true|Determina si el método hash debe rehashear todo|
|**splitter**|string|\\t|Separador de campos|;
|**table**|string|null|Nombre de la tabla activa|
|**tables**|array|null|Array con nombres de tablas para operaciones en bloque|
|**truncate**|boolean|false|Determina si debe vaciarse la tabla antes de hacer **loadfile**|;
|**where**|string|null|Condición SQL utilizada en varios métodos|
|**xtable**|string|null|Nombre de la tabla secundaria contra la que se realizan las operaciones|

## Atributos
|Atributo|Tipo|Descripción|
|---|---|---|
|**analysis**|array|análisis de la tabla activa|
|**colstr**|string|nombres concatenados de las columnas seleccionadas|
|**grouperstr**|string|nombres concatenados de las columnas del agrupador|
|**features_schema**|array|estructura de la tabla de características|
  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[analyse](#analyse)|Analiza la tabla activa|
|[analyseAll](#analyseAll)|Analiza un conjunto de tablas|
|[backup](#backup)|Hace un backup de la tabla activa|
|[backupAll](#backupAll)|Hace backups de un conjunto de tablas|
|[clear](#clear)|xxxxxxxxxx|
|[clearAll](#clearAll)|xxxxxxxxxx|
|[clearWano](#clearWano)|xxxxxxxxxx|
|[colsid](#colsid)|xxxxxxxxxx|
|[complete](#complete)|Completa uno o más columnas de la tabla destino, con datos de la tabla activa. Este método es complementario a [fill](#fill)|
|[concat](#concat)|Concatena una o mas columnas dentro de otra, utilizando el separador **splitter**|
|[drop](#drop)|xxxxxxxxxx|
|[duplicates](#duplicates)|Resumen de registros duplicados basado en el hash **\_\_pecker\_\_**|
|[equate](#equate)|Establece en el campo **\_\_pecker\_\_** el valor del campo **\_\_pecked\_\_** de otro registro con el mismo **\_\_pecker\_\_**|
|[fill](#fill)|Inserta en la tabla de destino los registros de la tabla activa donde **\_\_pecked\_\_** no sea nulo|
|[filltest](#filltest)|Testea la insersión de [fill](#fill) antes de ejecutarla|
|[filter](#filter)|xxxxxxxxxx|
|[getfeatures](#getfeatures)|xxxxxxxxxx|
|[getgrouper](#getgrouper)|xxxxxxxxxx|
|[hash](#hash)|Crea o alimenta el campo hash basado en el valor de una o varias columnas|
|[hit](#hit)|xxxxxxxxxx|
|[improve](#improve)|Cambia el tipo de columna en busca de un mejor rendimiento|
|[loadfile](#loadfile)|xxxxxxxxxx|
|[mark](#mark)|xxxxxxxxxx|
|[normalize](#normalize)|xxxxxxxxxx|
|[notpecked](#notpecked)|xxxxxxxxxx|
|[peck](#pecked)||
|[pecked](#pecked)|xxxxxxxxxx|
|[rename](#rename)|xxxxxxxxxx|
|[reset](#reset)|xxxxxxxxxx|
|[resetAll](#resetAll)|xxxxxxxxxx|
|[sanitize](#sanitize)|Sanitiza los datos de una o varias columnas, aplicando las politicas **policy**|
|[show](#show)|xxxxxxxxxx|
|[twins](#twins)|xxxxxxxxxx|
|[twinsAll](#twinsAll)|xxxxxxxxxx|
|[unhash](#unhash)|xxxxxxxxxx|
|[unify](#unify)|xxxxxxxxxx|
|[uniques](#uniques)|xxxxxxxxxx|
|Internos||
|[BuildAnalysis](#BuildAnalysis)|xxxxxxxxxx|
|[ChkAnalysis](#ChkAnalysis)|xxxxxxxxxx|
|[ChkFeatures](#ChkFeatures)|xxxxxxxxxx|
|[ChkHash](#ChkHash)|xxxxxxxxxx|
|[ChkKey](#ChkKey)|xxxxxxxxxx|
|[ChkSource](#ChkSource)|xxxxxxxxxx|
|[ClearAnalysis](#ClearAnalysis)|xxxxxxxxxx|
|[GetCols](#GetCols)|xxxxxxxxxx|
|[IsOwlTable](#IsOwlTable)|xxxxxxxxxx|
|[IfTableExist](#IfTableExist)|xxxxxxxxxx|
|[LoadData](#LoadData)|xxxxxxxxxx|
|[Mixer](#Mixer)|xxxxxxxxxx|
|[Output](#Output)|xxxxxxxxxx|
|[Sanitizer](#Sanitizer)|xxxxxxxxxx|
|[SaveData](#SaveData)|xxxxxxxxxx|
|[SecureName](#SecureName)|xxxxxxxxxx|
|[SetCols](#SetCols)|xxxxxxxxxx|
|[SetDataFile](#SetDataFile)|xxxxxxxxxx|
|[SetDb](#SetDb)|xxxxxxxxxx|
|[SetFeatures](#SetFeatures)|xxxxxxxxxx|
|[SetGrouper](#SetGrouper)|xxxxxxxxxx|
|[SetTable](#SetTable)|xxxxxxxxxx|
|[UpdateAnalysis](#UpdateAnalysis)|xxxxxxxxxx|
  
&nbsp;

## analyse
> Analiza la tabla activa, paso requerido para poder ejecutar cualquier otro método.
> Retorna un reporte donde se detalla:
> - **col** = número de columna
> - **field** = nombre de la columna
> - **type** = tipo de dato actual
> - **improve** = tipo de dacto sugerido para un mejor rendimiento
> - **length** = largo del máximo dato de la columna
> - **min** = mínimo valor de la columna
> - **max** = máximo valor de la columna
> - **empties** = cantidad de registros vacíos
> - **nulls** = cantidad de registros nulos
> - **normalizable** = cantidad de valores diferentes
> - **rows** = número total de registros
> 
> El valor **normalizable** unicamente está disponible si el argumento **analyse_datatype** es **TRUE**
> Este método retorna su valor por medio del método [Output](#Output)

**[mixed]** =  *public* function ( *string* $sTable, *boolean* $bForce );

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTable**|string|*arg::table*|Nombre de la tabla|
|**$bForce**|boolean|*arg::force*|Determina si debe forzarse un reanalisis|
  
### Ejemplos  
#### Análisis  
```php
$ngl("pecker")->analyse("sales2020");
```

&nbsp;
___
&nbsp;

## analyseAll
> Este método ejecuta el método [analyse](#analyse) para cada uno de las tablas pasadas en el argumento **\$aTables**

**[mixed]** =  *public* function ( *array* $aTables, *boolean* $bForce );

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aTables**|array|*arg::tables*|Array con los nombres de las tablas para analizar|
|**$bForce**|boolean|*arg::force*|Determina si debe forzarse un reanalisis|
  
### Ejemplos  
#### Análisis múltiple
```php
$ngl("pecker")->analyseAll(array("sales2019", "sales2020"));
```

&nbsp;
___
&nbsp;

## backup
> Hace un backup de la tabla activa, guardándolo con el mismo nombre de la tabla seguido de un guión bajo, seguido de la fecha y hora en formato **YmdHis**

**[mixed]** =  *public* function ( *string* $sTable, *boolean* $bForce );

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTable**|string|*arg::table*|Nombre de la tabla|
  
### Ejemplos  
#### Backup
```php
$ngl("pecker")->backup("sales2019");
```

&nbsp;
___
&nbsp;

## backupAll
> Este método ejecuta el método [backup](#backup) para cada uno de las tablas pasadas en el argumento **\$aTables**

**[mixed]** =  *public* function ( *array* $aTables );

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aTables**|array|*arg::tables*|Array con los nombres de las tablas para backupear|
  
### Ejemplos  
#### Backup múltiple  
```php
$ngl("pecker")->backupAll(array("sales2019", "sales2020"));
```

&nbsp;
___
&nbsp;

## duplicates
> Resumen de registros duplicados de la tabla activa basado en el hash **\_\_pecker\_\_**

**[mixed]** =  *public* function ();

### Ejemplos  
#### Backup múltiple  
```php
$ngl("pecker")->table("sales2019")->duplicates();
```

&nbsp;
___
&nbsp;

## hash
> Crea o alimenta el campo **\_\_pecker\_\_** con un hash creado sobre los valores de las columnas **$aCols**
> La primera vez que se ejecuta este método, crea en la tabla los campos **\_\_pecker\_\_**, **\_\_pecked\_\_** o **\_\_wano\_\_** para uso interno del objeto. Para eliminar estos campos debe ejecutarse [unhash](#unhash)

**[mixed]** =  *public* function ( *string* $sTable, *mixed* $mCols );

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTable**|string|*arg::table*|Nombre de la tabla|
|**$mCols**|mixed|*arg::cols*|Número de columna, Array con los números de columnas o cadena de rango (x-y)|
|**$aPolicy**|array|*arg::policy*|Array con los métodos de sanitización que deben aplicarse sobres el valor agrupado de las columnas, previo al hash:<ul><li>**digits** = elimina todo menos los digitos (0-9)</li><li>**consonants** = elimina todo menos las consonantes</li><li>**email** = elimina cualquier caracter que no sea válido en un email</li><li>**lcase** = convierte el valor a minúsculas</li><li>**left:LENGTH** = toma los LENGTH caracteres desde la izquierda</li><li>**letters** = elimina todo menos las letras (a-zA-Z)</li><li>**nospaces** = elimina todos los caracteres de espacio, tabulaciones y saltos de línea</li><li>**numbers** = elimina todo menos los números, sin importar signo ni signos de puntuación</li><li>**right:LENGTH** = toma los LENGTH caracteres desde la derecha</li><li>**trim** = elimina espacios en blanco a ambos lados</li><li>**trim:STRING** = elimina STRING a ambos lados del valor</li><li>**ucase** = convierte el valor a mayúsculas</li><li>**ucfirst** = convierte la primer letra de la cadena a mayúscula</li><li>**ucwords** = convierte la primer letra de cada palabra a mayúscula</li><li>**words** = elimina todo menos letters + digits</li></ul>|
  
### Ejemplos  
#### Grupo de varias columnas
```php
$ngl("pecker")->hash("sales2020", array("ucase", "trim"), array(5,6,7,10,12));
;
```

#### Aplicación por métodos
```php
$ngl("pecker")
	->table("sales2020")
	->policy(array("ucase", "trim"))
	->group(array(5,6))
	->hash()
;
```

&nbsp;
___
&nbsp;

## improve
> Cambia el tipo de columna al sugerido en el campo improve del método [analyse](#analyse) en busca de un mejor rendimiento.

**[mixed]** =  *public* function ( *mixed* $mCols );

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mCols**|mixed|*arg::cols*|Número de columna, Array con los números de columnas o cadena de rango (x-y)|
  
### Ejemplos  
#### Columna en particular
```php
$ngl("pecker")->table("sales2020")->improve(8);
```

#### Grupo de columnas
```php
$ngl("pecker")->table("sales2020")->improve(array(5,6,7,10,12));
```

#### Rango de columnas
```php
$ngl("pecker")->table("sales2020")->improve("0-10");
```

&nbsp;
___
&nbsp;

## sanitize
> Sanitiza los datos de una o varias columnas, aplicando las politicas **policy**

**[mixed]** =  *public* function ( *mixed* $mCols, *array* $aPolicy, *string* $sWhere);

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mCols**|mixed|*arg::cols*|Número de columna, Array con los números de columnas o cadena de rango (x-y)|
|**$aPolicy**|array|*arg::policy*|Array con los métodos de sanitización que deben aplicarse sobre la columna activa:<ul><li>**digits** = elimina todo menos los digitos (0-9)</li><li>**consonants** = elimina todo menos las consonantes</li><li>**email** = elimina cualquier caracter que no sea válido en un email</li><li>**lcase** = convierte el valor a minúsculas</li><li>**left:LENGTH** = toma los LENGTH caracteres desde la izquierda</li><li>**letters** = elimina todo menos las letras (a-zA-Z)</li><li>**nospaces** = elimina todos los caracteres de espacio, tabulaciones y saltos de línea</li><li>**numbers** = elimina todo menos los números, sin importar signo ni signos de puntuación</li><li>**right:LENGTH** = toma los LENGTH caracteres desde la derecha</li><li>**trim** = elimina espacios en blanco a ambos lados</li><li>**trim:STRING** = elimina STRING a ambos lados del valor</li><li>**ucase** = convierte el valor a mayúsculas</li><li>**ucfirst** = convierte la primer letra de la cadena a mayúscula</li><li>**ucwords** = convierte la primer letra de cada palabra a mayúscula</li><li>**words** = elimina todo menos letters + digits</li></ul>|
|**$sWhere**|string|*arg::where*|Condición SQL que acota el grupo de datos|
  
### Ejemplos  
#### Sanitizar un CUIT
```php
# suponiendo que la columna 3 es la columna CUIT
$ngl("pecker")->table("sales2020")->policy(array("digits", "left:11"))->sanitize(3);
```

#### Estandarizando nombre y apellido
```php
# suponiendo que las columna 4 y 5 tienen esos datos
$ngl("pecker")->table("sales2020")->policy(array("ucase", "trim"))->sanitize(array(8,9));
```

#### Sanitizando teléfonos
```php
# suponiendo que la columna 8 es un teléfono fijo y la 9 un mobil
$ngl("pecker")->table("sales2020")->policy(array("digits", "trim"))->sanitize(array(8,9));
```

&nbsp;
___
&nbsp;

## unify
> Cambia el tipo de columna al sugerido en el campo improve del método [analyse](#analyse) en busca de un mejor rendimiento.

**[mixed]** =  *public* function ( *array* $aRules, *string* $sWhere );

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aRules**|array|null|Array con las reglas para la unificación de datos: <ul><li>**any** = cualquier valor que no sea vacio</li><li>**false** = no hace nada</li><li>**join** = une los valores si son diferentes</li><li>**longer** = valor de mayor longuitud. Si tienen igual longuitud, hace join</li><li>**noempty** = cualquier valor que no sea vacio ni 0</li></ul>|
|**$sWhere**|string|*arg::where*|Condición SQL que acota el grupo de datos|

### Ejemplos  
#### Columna en particular
```php
$ngl("pecker")->table("sales2020")->unify(array(

));
```

#### Grupo de columnas
```php
$ngl("pecker")->table("sales2020")->improve(array(5,6,7,10,12));
```

#### Rango de columnas
```php
$ngl("pecker")->table("sales2020")->improve("0-10");
```

&nbsp;
___
&nbsp;




# Internos
## WriteContent
> Escribe contenido en un archivo. Este método es utilizado por [append](#append) y [write](#write)

**[$this]** =  *protected* function ( *string* $sFilePath, *string* $sContent, *boolean* $bReload, *string* $sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sFilePath**|string||Path del archivo de destino|
|**$sContent**|string||Contenido a escribir|
|**$bReload**|boolean|true|Determina si se aplicará el método nglFile::reload sobre el archivo para actualizar la información. Para una mejor performance se recomienda usar **false** cuando se realicen sucesivos [append](#append|
|**$sMode**|string|wb|Modo de escritura (según fopen)|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2020 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br /> 
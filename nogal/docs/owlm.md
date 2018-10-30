# owlm
Owl Manager es la herramienta por medio de la cual se crea y mantiene la estructura de base de datos (para MySQSL) del objeto [owl](https://github.com/arielbottero/wiki/blob/master/nogal/docs/owl.md)
___

## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|**$owl**|private|Objeto [owl](https://github.com/arielbottero/wiki/blob/master/nogal/docs/owl.md)|
|**$aTypes**|private|Tipos de campos predefinidos|
|**$bUpdate**|private|Indicador de modificación|
|**$aAdd**|private|Array de datos de agregado de la siguiente actualización|
|**$bAlterField**|private|Indicador de modificación de campo|
|**$aAlterTable**|private|Array de tablas que seran modificadas|
|**$aAlterField**|private|Array de campos que seran modificados|

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**about**|string|null|Tipo de dato solicitado al método [describe](#describe)|
|**after**|boolean|true|Nombre del campo después del cual se agregará el nuevo campo. Usar true para agregar al final|
|**core**|boolean|false|Indica si se deben agregar las sentencias para las creación de las tablas de control en el método [generate](#generate)|
|**db**|string|null|Objeto [mysql](https://github.com/arielbottero/wiki/blob/master/nogal/docs/mysql.md)|
|**structure**|string|null|Estructura owl en formato Array o una versión de la misma presente en la tabla **__ngl_sentences__**|
|**newname**|string|null|Nombre del nuevo objeto|
|**select**|string|null|Selecciona el objeto como activo|
|**preset**|string|null|Añade un objeto preseteado a la estructura|
|**type**|mixed|null|Define el tipo de campo que se quiere agregar|
|**field**|mixed|null|Nombre de un campo o array de nombres|
|**entity**|string|null|Nombre de un objeto que no es el actual. Es posible especificar un alias utilizando entidad:alias|
|**title**|string|null|Define el título de un objeto visible en la interfaz gráfica. Si no se especifica se utilizará como título el nombre del objeto|
|**fields**|string|null|Array de campos de un nuevo objeto o view|
|**run**|boolean|false|Indica si el método [generate](#generate) debe ejecutar las sentencias en la base|

## Definición de Campos
Cuando se agreguen o modifique campos, los mismos deberán estar definidos según:
- **type** = Tipo de campo
	- @NOMBRE_TABLA (para crear una unión PADRE-HIJO con otro objeto)
	- @NOMBRE_TABLA:ALIAS (para crear una unión con otro objeto)
	- BIGINT
	- BINARY
	- BIT
	- BLOB
	- BOOL
	- CHAR
	- DATE
	- DATETIME
	- DECIMAL
	- DOUBLE
	- ENUM
	- FLOAT
	- INT
	- LONGBLOB
	- LONGTEXT
	- MEDIUMBLOB
	- MEDIUMINT
	- MEDIUMTEXT
	- SET
	- SMALLINT
	- TEXT
	- TIME
	- TIMESTAMP
	- TINYBLOB
	- TINYINT
	- TINYTEXT
	- VARBINARY
	- VARCHAR
	- YEAR
- **length** = longuitud del campo ó set de datos entecomillados para los tipos SET y ENUM
- **default** = valor por defecto
	- valor
	- NONE
	- NULL
	- CURRENT_TIMESTAMP
- **attrs** = atributos especiales segun el tipo de campo
    - -- (vacio)
    - BINARY
	- UNSIGNED
	- UNSIGNED ZEROFILL
	- ON UPDATE CURRENT_TIMESTAMP
- **index** = indica que el campo es índice y de que tipo
	- -- (no es índice)
    - INDEX
	- UNIQUE
	- FULLTEXT
- **null** = indica si el campo acepta datos NULL
	- TRUE
	- FALSE
  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[add](#add)|Añade un campo al objeto actual|
|[alter](#alter)|Modifica un campo en el objeto actual|
|[check](#check)|Chequea si el valor es un Objeto o VIEW de la estructura|
|[chtitle](#chtitle)|Establece el titulo de un objeto (tabla) de la estructura. Los títulos son utilizados para referencia el objeto dentro del entorno del usuario final|
|[create](#create)|Crea un nuevo objeto dentro de la estructura|
|[describe](#describe)|Retorna los datos estructurales y de relación el objeto activo|
|[describeall](#describeall)|Retorna la estructura owl completa|
|[drop](#drop)|Elimina un objeto y todas sus referencias de la estructura|
|[duplicate](#duplicate)|Duplica un objeto existente|
|[generate](#generate)|Genera/ejecuta las sentencias SQL para generar impactar la estructura en la base de datos|
|[load](#load)|Carga en el objeto la estructura owl sobre la cual se necesita trabajar|
|[join](#join)|Se utiliza para realizar uniones directas entre el objeto actual y una VIEW|
|[presets](#presets)|Lista los objetos preseteados que pueden ser añadidos a la estructura|
|[rem](#rem)|Elimina uno o más campos del objeto activo|
|[rename](#rename)|Renombra el objeto actual|
|[save](#save)|Retorna la estructura owl con sus modificaciones actuales en formato JSON|
|[types](#types)|Lista los tipos de campos predefinidos que pueden ser utilizados en los objetos|
|[view](#view)|Registra una VIEW en la estructura|
|Internos||
|[CheckJoins](#checkjoins)|xxx|
|[CreateStructure](#createstructure)|xxx|
|[DefJoins](#defjoins)|xxx|
|[DescribeColumns](#describecolumns)|xxx|
|[FieldDef](#fielddef)|xxx|
|[MakePreset](#makepreset)|xxx|
|[SetObject](#setobject)|xxx|

&nbsp;

## add
> Añade un campo al objeto actual

**[$this]** =  *public* function ( *string* $sField, *mixed* $mType, *string* $sAfter );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sField**|string|*arg::field*|Nombre del campo. Puede asignarsele un alias separando el mismo por :|
|**$mType**|mixed|*arg::type*|Define el tipo de campo:  <ul><li>**array** = con los parámetros de la [definición del campo](#definicion-de-campos)</li><li>**string**<ul><li>**cadena** = tipo/alias de un campo [predefinido](#types)</li><li>**@NOMBRE_TABLA:ALIAS**<br />define el campo como *INT UNSIGNED*, crea un índice sobre él y genera una relación con el objeto **NOMBRE_TABLA** aplicando el alias **ALIAS**</li><li>**@TABLA-PADRE**<br />cuando el argumento **$sField** es **pid**, define el campo como INT UNSIGNED, crea un índice sobre él y genera una relación **CHILDREN** con el objeto **TABLA-PADRE**. En este caso el alias de la tabla será: **TABLA-PADRE_OBJETO-ACTUAL**</li></ul></li></ul>|
|**$sAfter**|string|*arg::after*|Nombre del campo después del cual se agregará el nuevo campo. Usar **true** para agregar al final|
### Ejemplos
#### agregando un campo predefinido
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm")->base($my);
$owlm->load("owl");
$owlm->select("contactos");
$owlm->add("direccion", "address");
```
#### agregando un campo tipo VARCHAR(32)
```php
$owlm->add("mascota", array("type"=>"varchar", "length"=>32));
```
#### agregando un campo vinculado a otro objeto
```php
$owlm->add("localidad", "@localidades:contactos_localidades");
```
#### agregando multiples campos
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm")->base($my);
$owlm->load("owl");
$owlm->select("clientes");
$owlm->add(array(
	array("fecha_alta", "date"),
	array("email_alternativo", "email"),
	array("responsable_compras", array("type"=>"varchar", "length"=>64))
));
```

&nbsp;
___
&nbsp;

## alter
> Modifica un campo en el objeto actual. La modificación puede o no incluir un cambio en el nombre del campo.

**[$this]** =  *public* function ( *string* $sField, *mixed* $mType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sField**|string|*arg::field*|Nombre del campo. Puede asignarsele un alias separando el mismo por :|
|**$mType**|mixed|*arg::type*|Define el tipo de campo: <ul><li>**array** = con los parámetros de la [definición del campo](#definicion-de-campos), completa o referenciando a un campo [predefinido](#types). Puede incluise el nuevo nombre en el índice **name**</li><li>**string**<ul><li>**cadena** = nuevo nombre del campo</li><li>**@NOMBRE_TABLA:ALIAS**<br />define el campo como *INT UNSIGNED*, crea un índice sobre él y genera una relación con el objeto **NOMBRE_TABLA** aplicando el alias **ALIAS**</li><li>**@TABLA-PADRE**<br />cuando el argumento **$sField** es **pid**, define el campo como INT UNSIGNED, crea un índice sobre él y genera una relación **CHILDREN** con el objeto **TABLA-PADRE**. En este caso el alias de la tabla será: **TABLA-PADRE_OBJETO-ACTUAL**</li></ul></li></ul>|
### Ejemplos
#### cambia el nombre de un campo
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm")->base($my);
$owlm->load("owl");
$owlm->select("contactos");
$owlm->alter("direccion", "direccion_comercial");
```
#### cambia el largo de un campo
```php
# supongamos que el campo nombre tiene 32 caracteres de largo
# y lo quereos extender a 64
$owlm->alter("nombre", array("length"=>64));
```
#### cambia un campo VARCHAR a un índice relacionado con otra tabla
```php
$owlm->alter("genero", "@generos:generos_contactos");
```
#### cambia el nombre y tipo de campo
```php
$owlm->alter("cp", array("name"=>"codigo_postal", "alias"=>"zipcode"));
```

&nbsp;
___
&nbsp;

## check
> Chequea si el valor es una tabla o VIEW de la estructura.

**[boolean]** =  *public* function ( *string* $sObject );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sObject**|string|*arg::entity*|Nombre de un objeto que no es el actual|

&nbsp;
___
&nbsp;

## chtitle
> Establece el titulo de un objeto (tabla) de la estructura. Los títulos son utilizados para referencia el objeto dentro del entorno del usuario final.

**[$this]** =  *public* function ( *string* $sTitle );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTitle**|string|*arg::title*|Nuevo título para el objeto|

&nbsp;
___
&nbsp;

## create
> Crea un nuevo objeto dentro de la estructura.

**[$this]** =  *public* function ( *string* $sTitle, *array* $aFields, *string* $sTitle );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sObject**|string|*arg::entity*|Nombre de un objeto que no es el actual|
|**$aFields**|array|*arg::fields*|Array con las definiciones de los campos del nuevo objeto|
|**$sTitle**|string|*arg::title*|Nuevo título para el objeto|
### Ejemplos
#### crea un nuevo objeto responsables, dependiente de eventos y vinculado a cargos
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm")->base($my);
$owlm->load("owl");
$owlm->create(
	"contactos",
	array(
		array("pid", "@eventos"),
		array("cargo", "@cargos:responsables_cargos"),
		array("nombre", "name"),
		array("correo", "email")
	)
	"Contactos"
);

```

&nbsp;
___
&nbsp;

## describe
> Por defecto, retorna un array con todos los datos estructurales y de relación el objeto activo.  
	- **relationship** = informe de relaciones
	- **view_code** = sentencia SQL para generar una VIEW
	- **structure** = definición de la estructura de campos
	- **foreignkeys** = relaciones de claves foraneas
	- **children** = dependencias directas
	- **joins** = relaciones con otros objetos
	- **validator** = estructura del validador de datos
> 
> Si se especifica un valor para **$sAbout** el método retornará ese único valor

**[mixed]** =  *public* function ( *string* $sAbout );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sAbout**|string|*arg::about*|Tipo de dato solicitado al método describe. NULL para todos|

&nbsp;
___
&nbsp;

## describeall
> Retorna la estructura owl completa

**[array]** =  *public* function ( );  

&nbsp;
___
&nbsp;

## drop
> Elimina un objeto y todas sus referencias de la estructura. A nivel base de datos, la tabla no es eliminada, es renombrada como:  
> **dropped_NOMBRE-OBJETO_FECHA-COMPLETA_ID-UNICO** por ejemplo: **dropped_comercios_20181030_Hdf23rT3** 

**[$this]** =  *public* function ( );  

&nbsp;
___
&nbsp;

## duplicate
> Duplica un objeto existente.

**[$this]** =  *public* function ( *string* $sClone, *string* $sTitle );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sClone**|string|*arg::newname*|Nombre del nuevo objeto|
|**$sTitle**|string|*arg::title*|Título para el objeto|
### Ejemplos
#### duplicando el objeto contactos
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm");
$owlm->load("owl", $my)
	->select("contactos")
	->duplicate("contactos_proveedores", "Contacto de Proveedores")
;
```

&nbsp;
___
&nbsp;

## generate
> Genera/ejecuta las sentencias SQL para generar impactar la estructura en la base de datos.

**[string]** =  *public* function ( *boolean* $bRun ); 
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$bRun**|boolean|*arg::run*|Indica si el método debe ejecutar las sentencias en la base o sólo retornarlas|

&nbsp;
___
&nbsp;

## load
> Carga en el objeto la estructura owl sobre la cual se necesita trabajar.  
> Si el argumento **$mStructure** es NULL, se iniciará una nueva estructura owl.

**[$this]** =  *public* function ( *mixed* $mStructure, *object* $db ); 
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mStructure**|mixed|*arg::structure*|Estructura owl en formato Array o una versión de la misma presente en la tabla **__ngl_sentences__**|
|**$db**|object|*arg::db*|Objeto [mysql](https://github.com/arielbottero/wiki/blob/master/nogal/docs/mysql.md)|
### Ejemplos
#### carga de una antigua versión de la estructura
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm");
$owlm->load("owl-20180101145231", $my);
```

&nbsp;
___
&nbsp;

## join
> Se utiliza para realizar uniones directas entre el objeto actual y una [VIEW](#view) previamente cargada en la estructura.  
> Las uniones se realizan siempre contra la columna **ID** de la VIEW, si la **VIEW** no cuenta con un campo **ID** ó si se necesitan más condiciones en el **ON**, se deberá crear una nueva **VIEW**

**[$this]** =  *public* function ( *mixed* $sWith, *object* $sField ); 
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sWith**|string|*arg::entity*|Nombre de un objeto que no es el actual. Es posible especificar un alias utilizando entidad:alias|
|**$sField**|string|*arg::field*|Nombre del campo por el cual se realizará la unión|
### Ejemplos
#### unión contra una VIEW
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm");
$owlm->load("owl", $my)
	->select("clientes")
	->join("view_reporte_ventas:ventas_por_clientes", "cuit")
;
```

&nbsp;
___
&nbsp;

## presets
> Lista los objetos preseteados que pueden ser añadidos a la estructura.

**[array]** =  *public* function ( );  

&nbsp;
___
&nbsp;

## rem
> Elimina uno o más campos del objeto activo.

**[$this]** =  *public* function ( *mixed* $mField ); 
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mField**|mixed|*arg::field*|Nombre o array de nombres de los campos a eliminar|
### Ejemplos
#### eliminando campos
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm");
$owlm->load("owl", $my)
	->select("contactos")
	->rem(array("MSN","ICQ"))
;
```

&nbsp;
___
&nbsp;

## rename
> Renombra el objeto actual.

**[$this]** =  *public* function ( *string* $sNewName, *string* $sTitle );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sNewName**|array|*arg::newname*|Nuevo nombre del objeto|
|**$sTitle**|string|*arg::title*|Nuevo título para el objeto|
### Ejemplos
#### renombrar
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm");
$owlm->load("owl", $my)
	->select("contactos")
	->rename("contactos_clientes", "Contactos de Clientes")
;
```

&nbsp;
___
&nbsp;

## save
> Retorna la estructura owl con sus modificaciones actuales en formato JSON.

**[string]** =  *public* function ( );  

&nbsp;
___
&nbsp;

## types
> Lista los tipos de campos predefinidos que pueden ser utilizados en los objetos.

**[array]** =  *public* function ( );  

&nbsp;
___
&nbsp;

## view
> Registra una VIEW en la estructura.

**[$this]** =  *public* function ( *string* $sObject, *string* $aFields, *string* $sTitle );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sNewName**|array|*arg::entity*|Nuevo nombre del objeto|
|**$sTitle**|string|*arg::fields*|Array con los nombres de los campos de la VIEW|
|**$sTitle**|string|*arg::title*|Título de la VIEW|
### Ejemplos
#### registro de una VIEW
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm");
$owlm->load("owl", $my)
	->view(
		"view_reporte_ventas",
		array("fecha","cuit","monto"),
		"Reporte de Ventas"
	)
;
```

&nbsp;
___
&nbsp;

# Internos
## MakeGroup
> Auxiliar de [grants](#grants). Crea los grupos y perfiles

**[array]** =  *public* function ( *array* $aData, *string* $sType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aData**|array||Array de permisos|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
# owlm
Owl Manager es la herramienta para crear y mantener la estructura de base de datos del objeto [owl](owl.md), en MySQSL.  
Para ver un ejemplo de uso completo ver la guía [owl paso a paso](owluso.md)
___

## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|**$owl**|private|Objeto [owl](owl.md)|
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
|**db**|string|null|Objeto [mysql](mysql.md)|
|**der**|string|null|Determina si el método [describeall](#describeall) debe retornar los datos perparados para generar un DER|
|**structure**|string|null|Estructura owl en formato Array o una versión de la misma presente en la tabla **__ngl_sentences__**|
|**newname**|string|null|Nombre del nuevo objeto|
|**select**|string|null|Selecciona el objeto como activo|
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
|[create](#create)|Crea un nuevo objeto dentro de la estructura y lo establese como objeto activo|
|[describe](#describe)|Retorna los datos estructurales y de relación el objeto activo|
|[describeall](#describeall)|Retorna la estructura owl completa|
|[drop](#drop)|Elimina un objeto y todas sus referencias de la estructura|
|[generate](#generate)|Genera/ejecuta las sentencias SQL para generar impactar la estructura en la base de datos|
|[load](#load)|Carga en el objeto la estructura owl sobre la cual se necesita trabajar|
|[join](#join)|Se utiliza para realizar uniones directas entre el objeto actual y una VIEW|
|[preset](#preset)|En caso de que exista, añade a la estructura el código de un objeto predefinido y lo establese como objeto activo|
|[presets](#presets)|Lista los objetos preseteados que pueden ser añadidos a la estructura|
|[rem](#rem)|Elimina uno o más campos del objeto activo|
|[rename](#rename)|Renombra el objeto actual|
|[save](#save)|Retorna la estructura owl con sus modificaciones actuales en formato JSON|
|[twin](#twin)|Duplica un objeto existente|
|[types](#types)|Lista los tipos de campos predefinidos que pueden ser utilizados en los objetos|
|[view](#view)|Registra una VIEW en la estructura|
|Internos||
|[CreateStructure](#createstructure)|Retorna las sentencias SQL necesarias para crear las tablas de control|
|[DefJoins](#defjoins)|Arma la estructura de relaciones para el objeto y campo dados|
|[DescribeColumns](#describecolumns)|Retorna los fragmentos de sentencias SQL SELECT para las columnas de una tabla ó view|
|[FieldDef](#fielddef)|Retorna el fragmento SQL utilizado por CREATE TABLE/ALTER TABLE que define a un campo, basado en su definición owl|
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
$owlm = $ngl("owlm")->db($my);
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
$owlm = $ngl("owlm")->db($my);
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
$owlm = $ngl("owlm")->db($my);
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
$owlm = $ngl("owlm");
$owlm->load("owl", $my);
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
> Retorna la estructura owl completa ó o los datos necesarios para generar un DER

**[array]** =  *public* function ( *boolean* $bDer );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$bDer**|boolean|*arg::der*|Determina si el método debe retornar los datos perparados para generar un DER|

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
> Si el argumento **$db** es NULL, se intenrará iniciar un objeto [mysql](mysql.md).

**[$this]** =  *public* function ( *mixed* $mStructure, *object* $db ); 
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mStructure**|mixed|*arg::structure*|Estructura owl en formato Array o una versión de la misma presente en la tabla **__ngl_sentences__**|
|**$db**|object|*arg::db*|Objeto [mysql](mysql.md)|
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

## preset
> En caso de que exista, añade a la estructura el código de un objeto predefinido.

**[string]** =  *public* function ( *string* $sEntity, *string* $sNewName, *string* $sTitle );  
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sEntity**|string|*arg::entity*|Nombre del objeto preseteado|
|**$sNewName**|string|*arg::newname*|Nombre del nuevo objeto|
|**$sTitle**|string|*arg::title*|Título para el nuevo objeto|
### Ejemplos
#### clásico objeto de lista desplegable
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm");
$owlm->load("owl", $my)
	->preset("users", "usuarios", "Usuarios")
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

## twin
> Duplica un objeto existente.

**[$this]** =  *public* function ( *string* $sTwin, *string* $sTitle );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTwin**|string|*arg::newname*|Nombre del nuevo objeto|
|**$sTitle**|string|*arg::title*|Título para el objeto|
### Ejemplos
#### duplicando el objeto contactos
```php
$my = $ngl("mysql")->connect();
$owlm = $ngl("owlm");
$owlm->load("owl", $my)
	->select("contactos")
	->clone("contactos_proveedores", "Contacto de Proveedores")
;
```

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
## CreateStructure
> Retorna las sentencias SQL necesarias para crear las tablas de control

**[string]** =  *private* function ( );  

&nbsp;
___
&nbsp;

## DefJoins
> Arma la estructura de relaciones para el objeto y campo dados.  
> Este método es utilizado como auxiliar por los métodos [add](#add) y [alter](#alter) 

**[void]** =  *private* function ( *string* $sObject, *string* $sField, *string* $sType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sObject**|string||Nombre del objeto|
|**$sField**|string||Nombre del campo|
|**$sType**|string|null|Tipo/definición del campo|

&nbsp;
___
&nbsp;

## FieldDef
> Retorna el fragmento SQL utilizado por CREATE TABLE/ALTER TABLE que define a un campo, basado en su definición owl

**[string]** =  *private* function ( *string* $sField, *array* $aField );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sField**|string||Nombre del campo|
|**$aField**|array||Definición del campo|
### Ejemplos
#### VARCHAR
```php
$this->FieldDef("comentario", array(
	"type"=>"VARCHAR", 
	"length"=>"255", 
	"default"=>"NONE", 
	"attrs"=>"--", 
	"index"=>"--", 
	"null"=>false)
);

# retornará
`comentario` VARCHAR (255) NOT NULL
```

&nbsp;
___
&nbsp;

## SetObject
> En caso de existir, setea a **$sObject** como el objeto activo.

**[string]** =  *private* function ( *string* $sObject );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sObject**|string||Nombre del objeto|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2019 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
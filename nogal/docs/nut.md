# nut
Gestor de los **nuts**.
___

## Definición de **nut**
Los **nuts** son clases php cuyo objetivo es agrupar procedimientos que pueden o no invocar a otros objetos del **nogal**. Son las *vistas* del modelo.<br />
A diferencia de los otros objetos, los **nuts** pueden ser utilizados dentro de las plantillas [rind](rind.md) como origen de datos.
Todos los **nuts** son heredados del objeto principal. La forma de invocar un **nut** es:
```php
$ngl("nut.NOMBRE_DEL_NUT")->run("NOMBRE_DE_METODO", $Array);
```

## Definidos por el Usuario
El sistema tiene algunos **nuts** genéricos, pero el usuario también puede definir los propios. Para aprender cómo, consultar la guía [Creando NUTS](nuts.md)
  
## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|**$sNut**|public|Nombre del nut actual|
|**$bSafemode**|private|Determina si el modo seguro está o no activo|
|**$ID**|protected|ID del nut|
|**$aSafeMethods**|protected|Listado de métodos seguros|

&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[arg](#arg)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[ifmethod](#ifmethod)|Verifica la existencia de un método dentro de un **nut**|
|[load](#load)|Carga y retorna un **nut** listo para ser usado|
|[ngl](#ngl)|Retorna un objeto del framework utilizando el método [root::call](root.md#call)|
|[run](#run)|Ejecuta el método $sMethod del nut|
|[safemode](#safemode)|Setea y/o retorna el valor de la variable **$bSafemode**|
|Internos||
|[SafeMethods](#safemethods)|Establece/retorna el valor de la variable **$aSafeMethods**|

&nbsp;

## arg
> Obtiene el valor del argumento **$sIndex** del array **$vArguments**, si no existiese retorna **$mDefault**  

**[mixed]** =  *protected* function ( *array* $vArguments, *mixed* $mDefault );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$vArguments**|array||Argumentos del nut"|
|**$sIndex**|string||Nombre del argumento buscado"|
|**$mDefault**|mixed|null|Valor por defecto"|

&nbsp;
___
&nbsp;

## ngl
> Retorna un objeto del framework utilizando el método [root::call](root.md#call)

**[object]** =  *public* function ( *string* $sObjectName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sObjectName**|string||Nombre del objeto|

&nbsp;
___
&nbsp;

## load
> Carga y retorna un nut listo para ser usado
> Al no ser este un objeto instanciable, si se pasa el argumento **$sNutID** el método intentará recuperar un **nut** previamente ejecutado

**[object]** =  *public* function ( *string* $sNutName, *string* $sNutID );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sNutName**|string||Nombre del nut|
|**$sNutID**|string||Id del nut|
### Ejemplos
Supongamos la existencia de un **nut* denomninado **foobar**
#### carga del nut foobar
```php
$my = $ngl("nut.foobar");
```

&nbsp;
___
&nbsp;

## ifmethod
> Verifica la existencia de un método en un **nut** 

**[boolean]** =  *public* function ( *string* $sFunction );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sFunction**|string||Nombre del método|

&nbsp;
___
&nbsp;

## run
> Ejecuta el método **$sMethod** del **nut** pasandole como único argumento **$aArguments**

**[mixed]** =  *public* function ( *string* $sMethod, *array* $aArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sMethod**|string||Nombre del método|
|**$aArguments**|array||Argumentos que se pasarán al método.<ul><li>Todos los índices alfanuméricos serán convertidos a minúsculas</li><li>Si el nombre de uno o más índices comienzan con "data-", serán reagrupados dentro de un único índice DATA y eliminados del array principal</li></ul>|
### Ejemplos
Supongamos que el método **lorem** imprime un texto aleatorio en función del idioma especificado
#### nut foobar método lorem
```php
$my = $ngl("nut.foobar")->run("lorem", array("lang"=>"es"));
```
#### ejemplo de re-agrupación de índices data
```php
Array (
	BASE => "test",
	tabla => "ventas",
	data-sucursal => "Central",
	data-monto => "1500",
	data-mes => "08"
)

# se re-agrupa como
Array (
	base => "test",
	tabla => "ventas",
	data => Array (
		sucursal => "Central",
		monto => "1500",
		mes => "08"
	)
)
```

&nbsp;
___
&nbsp;

## safemode
> Setea y/o retorna el valor de la variable **$bSafemode**
> Los **nuts** son accesesibles desde las plantillas [rind](rind.md). Cuando **$bSafemode** = **true** sólo podrán ejecutarse desde las plantillas aquellos métodos declarados en **$safemethods**
> Cuando el valor de **$bMode** sea **null** simplemente se retornará el valor actual de la variable.

**[boolean]** =  *public* function ( *boolean* $bMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$bMode**|boolean|null|True o False|

&nbsp;
___
&nbsp;

# Internos
## SafeMethods
> Establece/retorna el valor de la variable **$aSafeMethods**
> Cuando el valor de **$aSafeMethods** sea **null** simplemente se retornará el valor actual de la variable.

**[boolean]** =  *protected* function ( *boolean* $aSafeMethods );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aSafeMethods**|array|null|Array de métodos seguros|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2019 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
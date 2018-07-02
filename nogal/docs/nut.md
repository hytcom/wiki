# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# nut
## nglNut *extends* nglTrunk [main] [20150608]
Este objeto es el gestor de nuts del sistema, construye el objeto \$nut dentro del framework, el cual es accedido a través de: **\$ngl("nut.NOMBRE_DEL_NUT")->run("NOMBRE_DE_METODO", ...)**
  
## Variables
`public` $safemethods = Listado de método que pueden ser ejecutados con <b>nglNut::$bSafemode</b> en <b>true</b>  
`public` $sNut = Nombre del nut actual  
`private` $bSafemode = Determina si el modo seguro está o no activo  
`protected` $ID = ID del nut  
`protected` $storage = Array vacio para almacenar datos que luego pueden ser obtenidos mediante nutid  

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[arg](#arg)|Obtiene el valor del argumento $sIndex del array $vArguments, si no existiese retorna $mDefault|
|[ifmethod](#ifmethod)|Verifica la existencia de un m�todo en un nut|
|[load](#load)|Carga y retorna un nut listo para ser usado|
|[ngl](#ngl)|Retorna un objeto del framework utilizando el m�todo nglRoot::call|
|[run](#run)|Ejecuta el m�todo $sMethod del nut|
|[safemode](#safemode)|Setea y/o retorna el valor de la variable nglNut::$bSafemodeCuando el valor de $bMode sea null simplemente se retornar� el valor actual de la variabl...|

  
&nbsp;


## arg
Obtiene el valor del argumento **\$sIndex** del array **\$vArguments**, si no existiese retorna **\$mDefault**  

**[mixed]** =  *protected* function ( *array* \$vArguments, *mixed* \$mDefault );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array|||
|**\$mDefault**|mixed|null|Valor por defecto|

&nbsp;
___
&nbsp;

## ngl
Retorna un objeto del framework utilizando el método **nglRoot::call**  

**[object]** =  *public* function ( *string* \$sObjectName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sObjectName**|string||Nombre del objeto|

&nbsp;
___
&nbsp;

## load
Carga y retorna un nut listo para ser usado  

**[object]** =  *public* function ( *string* \$sNutName, *array* \$sNutID );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sNutName**|string||Nombre del nut|
|**\$sNutID**|array||Id del nut|

&nbsp;
___
&nbsp;

## ifmethod
Verifica la existencia de un método en un nut  

**[boolean]** =  *public* function ( *string* \$sFunction );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFunction**|string||Nombre del método|

&nbsp;
___
&nbsp;

## run
Ejecuta el método **\$sMethod** del nut  

**[mixed]** =  *public* function ( *string* \$sMethod, *mixed* \$mArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sMethod**|string||Nombre del método|
|**\$mArguments**|mixed||Argumentos que se pasarán al método|

&nbsp;
___
&nbsp;

## safemode
Setea y/o retorna el valor de la variable **nglNut::\$bSafemode**
Cuando el valor de **\$bMode** sea **null** simplemente se retornará el valor actual de la variable.  

**[boolean]** =  *public* function ( *boolean* \$bMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$bMode**|boolean|null|True o False|

&nbsp;
___
&nbsp;

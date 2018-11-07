# tutor
Los tutores son los encargados de realizar las tareas de *escritura* dentro del sistema, son los controladores del modelo MVC. En en ellos se declaran los procedimientos de escritura en base de datos, administración de archivos, envio de correos, etc.  
Este objeto no es en sí mismo un tutor, sino que es el padre del cual se desprenden los tutores.  
Los tutores pueden o no ser **bloqueables**, esto significa que sus métodos sólo funcionan cuando previamente se ha ejecutado el método **unlock**, evitando que puedan ser invocados desde la URL sin consentimiento. Luego, el tutor se vuelve a **bloquear** automáticamente.  
Dentro de los tutores de un proyecto es posible definir un tutor **master**, el cual realiza tareas comunes a más de un tutor, como por ejemplo, enviar e-mails.  
Cuando se declara un **tutor** se debe tener en cuenta que estos no aceptan métodos que no sean del tipo **protected** o **private**. Cualquier otro tipo de método será ignorado.
___

## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|**$aAllowedMethods**|private|Listado de métodos permitidos. Los métodos public serán ignorados|
|**$bDebug**|protected|Activa/desactiva el modo **debug**|
|**$bLockable**|private|Determina si el **tutor** es **bloqueable**|
|**$bLocked**|private|Indica si el **tutor** está bloqueado|
|**$master**|protected|Objeto que alberga al principal tutor del proyecto|
|**$sMethodName**|private|Nombre del método a ejecutarse|
|**$sTutorName**|private|Nombre del tutor activo|
|**$tutor**|private|Objeto que alberga al tutor en ejecución|

&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[debugging](#debugging)|Indica si el tutor se encuentra en modo **debug**|
|[load](#load)|Carga un tutor en el |
|[run](#run)|Chequea uno o más permisos y retorna un array con el resultado de las validaciones|
|Internos||
|[MakeGroup](#makegroup)|Auxiliar de [grants](#grants). Crea los grupos y perfiles|

&nbsp;

## analize
> Chequea uno o más permisos y retorna un array con el resultado de las validaciones

**[array]** =  *public* function ( *string* $sGrant, *string* $sToken );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sGrant**|string||[Cadena de permisos](https://github.com/arielbottero/wiki/blob/master/nogal/docs/alvinuso.md#cadenas-de-permisos) a chequear|
|**$sToken**|string|null|Token del usuario|
### Ejemplos
#### chequea los siguientes permisos sobre el usuario
```php
$chks = $alvin->analize("BUYING.DELETE,BUYING.ADD,BUYING.EDIT,USER.EDIT");
print_r($chks);

#retornará
Array (
	[BUYING.DELETE] => false
	[BUYING.ADD] => true
	[BUYING.EDIT] => true
	[USER.EDIT] => false
);
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
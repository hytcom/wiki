# object
descripcion
___

## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|**$aToken**|private|Datos con los que se generá el token|

&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[analize](#analize)|Chequea uno o más permisos y retorna un array con el resultado de las validaciones|
|Internos||
|[MakeGroup](#makegroup)|Auxiliar de [grants](#grants). Crea los grupos y perfiles|

&nbsp;

## analize
> Chequea uno o más permisos y retorna un array con el resultado de las validaciones

**[array]** =  *public* function ( *string* $sGrant, *string* $sToken );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sGrant**|string||[Cadena de permisos](alvinuso.md#cadenas-de-permisos) a chequear|
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
<sup>&copy; 2019 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
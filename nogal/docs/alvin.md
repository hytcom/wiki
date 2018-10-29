# alvin
Alvin es el sistema de seguridad de **nogal**, encargado de gestionar permisos, grupos y perfiles de usuario.  
Mas que un objeto es un concepto que atraviesa transversalmente todo el framework.  
Para aprender cómo aplicar Alvin, consultar la guía de [Aplicando Alvin](https://github.com/arielbottero/wiki/blob/master/nogal/docs/alvinuso.md)
___

## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|**$aToken**|private|Datos con los que se generá el token|
|**$sEncryptKey**|private|Clave RSA pública/privada utilizada para encriptar/desencritar el token. Por defecto la constante **NGL_ALVIN**|
|**$crypt**|private|Objecto [crypt](https://github.com/arielbottero/wiki/blob/master/nogal/docs/alvin.md)|
|**$vGrants**|public|Estructura de permisos de la cual se desprende el token|

&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[analize](#analize)|Chequea uno o más permisos y retorna un array con el resultado de las validaciones|
|[check](#check)|Chequea uno o más permisos y retorna **true** o **false** según el resultado de las validaciones|
|[grants](#grants)|xxx|
|[keys](#keys)|xxx|
|[load](#load)|xxx|
|[loaded](#loaded)|xxx|
|[password](#password)|xxx|
|[raw](#raw)|xxx|
|[setkey](#setkey)|xxx|
|[token](#token)|xxx|
|[unload](#unload)|xxx|
|[username](#username)|xxx|
|Internos||
|[CheckGrant](#checkgrant)|xxx|
|[MakeGroup](#makegroup)|xxx|

&nbsp;

## analize
> Chequea uno o más permisos y retorna un array con el resultado de las validaciones

**[array]** =  *public* function ( *string* $sGrant, *string* $sToken );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sGrant**|string||Cadena de permisos a chequear|
|**$sToken**|string|null|Token del usuario|
### Ejemplos
Chequea los siguientes permisos sobre el usuario
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

## check
> Chequea uno o más permisos y retorna **true** o **false** según el resultado de las validaciones

**[array]** =  *public* function ( *string* $sGrant, *string* $sToken );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sGrant**|string||Cadena de permisos a chequear|
|**$sToken**|string|null|Token del usuario|
### Ejemplos
Chequea los siguientes permisos sobre el usuario
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



&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
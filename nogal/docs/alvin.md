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
|[analize](#analize)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[check](#check)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[grants](#grants)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[keys](#keys)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[load](#load)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[loaded](#loaded)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[password](#password)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[raw](#raw)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[setkey](#setkey)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[token](#token)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[unload](#unload)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[username](#username)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|Internos||
|[CheckGrant](#checkgrant)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|
|[MakeGroup](#makegroup)|Obtiene el valor del obtiene el valor de uno de los índices del array de argumentos|


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



&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
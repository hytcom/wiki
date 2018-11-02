# alvin
Alvin es el sistema de seguridad de **nogal**, encargado de gestionar permisos, grupos y perfiles de usuario.  
Mas que un objeto es un concepto que atraviesa transversalmente todo el framework.  
Para aprender cómo aplicar Alvin, consultar la guía de [Aplicando Alvin](alvinuso.md)
___

## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|**$aToken**|private|Datos con los que se generá el token|
|**$sEncryptKey**|private|Clave RSA pública/privada utilizada para encriptar/desencritar el token. Por defecto la constante **NGL_ALVIN**|
|**$crypt**|private|Objecto [crypt](alvin.md)|
|**$vGrants**|public|Estructura de permisos de la cual se desprende el token|

&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[analize](#analize)|Chequea uno o más permisos y retorna un array con el resultado de las validaciones|
|[check](#check)|Chequea uno o más permisos y retorna **true** o **false** según el resultado de las validaciones|
|[grants](#grants)|Carga en el objeto la estructura de persmisos con la que se generarán los tokens|
|[keys](#keys)|Retorna el par de claves publica-privada o false, en caso de no existir el objeto [crypt](crypt.md)|
|[load](#load)|Carga el token del usuario en el objeto|
|[loaded](#loaded)|Verifica si el token del usuario se encuentra cargado correctamente|
|[password](#password)|Retorna un password SHA512 de 5000 vueltas, basado en **$sPassword** y utilizando como **salt** la clave pública|
|[raw](#raw)|Retorna el contenido de un [valor crudo](alvinuso.md#valores-crudos)|
|[setkey](#setkey)|Setea **$sKey** como clave activa, ya sea pública para leer o privada para generar el token|
|[token](#token)|Establece los permisos y valores RAW que se utilizarán para generar el token|
|[unload](#unload)|Desetea el token actual|
|[username](#username)|Retorna un nombre de usuario válido eliminando de **$sUsername** los caracteres prohibidos|
|Internos||
|[CheckGrant](#checkgrant)|Utilizada por los métodos [analize](#analize) y [check](#check) para validar los permisos|
|[MakeGroup](#makegroup)|Auxiliar de [grants](#grants). Crea los grupos y perfiles|

&nbsp;

# Nota
En los ejemplos supondremos la siguiente estructura de permisos
``` json
[
	{"type":"grant","name":"sales","grant":"SALES"},
	{"type":"grant","name":"sales.add","grant":"SALES>ADD"},
	{"type":"grant","name":"sales.delete","grant":"SALES>DELETE"},
	{"type":"grant","name":"sales.edit","grant":"SALES>EDIT"},
	{"type":"grant","name":"sales.view","grant":"SALES>VIEW"},

	{"type":"grant","name":"buying","grant":"BUYING"},
	{"type":"grant","name":"buying.add","grant":"BUYING>ADD"},
	{"type":"grant","name":"buying.delete","grant":"BUYING>DELETE"},
	{"type":"grant","name":"buying.edit","grant":"BUYING>EDIT"},
	{"type":"grant","name":"buying.view","grant":"BUYING>VIEW"},
	
	{"type":"grant","name":"users","grant":"USERS"},
	{"type":"grant","name":"users.view","grant":"USERS>VIEW"},

	{"type":"group","name":"rootsales","grant":["sales"]},
	{"type":"group","name":"rootbuying","grant":["buying"]},

	{"type":"group","name":"adminsales","grant":["sales", "-sales.delete"]},
	{"type":"group","name":"adminbuying","grant":["buying", "-buying.delete"]},
	
	{"type":"profile","name":"root","grant":["rootsales","rootbuying","users"]},
	{"type":"profile","name":"admin.boss","grant":["adminsales","adminbuying","users.view"]}
]
```

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

## check
> Chequea uno o más permisos y retorna **true** o **false** según el resultado de las validaciones

**[boolean]** =  *public* function ( *string* $sGrant, *string* $sToken );  

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
true o false
```

&nbsp;
___
&nbsp;

## grants
> Carga en el objeto la estructura de persmisos con la que se generarán los tokens

**[$this]** =  *public* function ( *array* $aDefinitions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aDefinitions**|array||[Estructura de permisos](alvinuso.md#definir-permisos)|

&nbsp;
___
&nbsp;

## keys
> Retorna el par de claves publica-privada o false, en caso de no existir el objeto [crypt](crypt.md)

**[array]** =  *public* function ( );  

### Ejemplos
#### par de claves pública/privada
```php
$keys = $ngl("alvin")->keys();

#salida
Array (
	[private] =>
		-----BEGIN RSA PRIVATE KEY-----
		MIGoAgEAAiEAuVxGKBvOd7UqRRTp7k9WSbsIKhZPHW+yufySCWdePJ8CAwEAAQIf
		aq1bCGT4bpcqZ0JMtNpJeIAUNToWFU/MNp0z3bn6sQIRANyc1D3hGqnqk/wCBCeH
		JxUCEQDXF9xDpacmIisUHIHlwYHjAhBV625tuyHbU1TXLSHZEzYRAhAhi6IZlsM7
		ykZnq46Cs6w7AhAUaAwFtzctOcvARCt/GWGb
		-----END RSA PRIVATE KEY-----

	[public] =>
		-----BEGIN PUBLIC KEY-----
		MDwwDQYJKoZIhvcNAQEBBQADKwAwKAIhALlcRigbzne1KkUU6e5PVkm7CCoWTx1v
		srn8kglnXjyfAgMBAAE=
		-----END PUBLIC KEY-----
)
```

&nbsp;
___
&nbsp;

## load
> Carga el token del usuario en el objeto  
> Antes de cargar el token es necesario haber definido la clave pública para que éste pueda ser leído

**[$this]** =  *public* function ( *string* $sToken );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sToken**|string|null|Token de permisos del usuario|
### Ejemplos
#### carga de clave pública y token
```php
$alvin->setkey(NGL_ALVIN);
$alvin->load($sToken);
```

&nbsp;
___
&nbsp;

## loaded
> Verifica si el token del usuario se encuentra cargado correctamente

**[boolean]** =  *public* function ( );  

&nbsp;
___
&nbsp;

## password
> Retorna un password SHA512 de 5000 vueltas, basado en **$sPassword** y utilizando como **salt** la clave pública 

**[string]** =  *public* function ( *string* $sPassword );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sPassword**|string||Password sin hashear|
### Ejemplos
#### carga de clave pública y hasheo de password
```php
$alvin->setkey(NGL_ALVIN);
$alvin->password("mipass4321");
```

&nbsp;
___
&nbsp;

## raw
> Retorna el contenido de un [valor crudo](alvinuso.md#valores-crudos)  
> En caso de no especificarse un valor para **$sIndex** retornará el array completo de valores **RAW**

**[mixed]** =  *public* function ( *string* $sIndex, *string* $sToken );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sIndex**|string|null|Indice **RAW** que se quiere recuperar|
|**$sToken**|string|null|Token del usuario|
### Ejemplos
#### retornará el fragmento de consulta SQL para limitar una sentencia
```php
$alvin->raw("sql")["place"];
```

&nbsp;
___
&nbsp;

## setkey
> Setea **$sKey** como clave activa, ya sea pública para leer o privada para generar el token.  
> Sino se especifíca una clave, el objeto conserva como activa la constante NGL_ALVIN

**[string]** =  *public* function ( *string* $sKey );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sKey**|string|NGL_ALVIN|Clave pública/privada|


&nbsp;
___
&nbsp;

## token
> Establece los permisos y valores RAW que se utilizarán para generar el token  
> En cuanto a los permisos, si los mismos están presedidos por un **-** eso indica que es un permiso de exclusión, [ver ejemplo](alvinuso.md#generar-token)

**[string]** =  *public* function ( *array* $aProfiles, *array* $aRaw );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aProfiles**|array|array()|Array con los permisos del usuario|
|**$aRaw**|array|array()|Clave pública/privada|
### Ejemplos
#### crea un token con acceso a todo compras y ventas, salvo a borrarlas
```php
echo $alvin->token(array("buying", "-buying.delete", "sales", "-sales.delete"));
```

Mismo token pero con algunos valores RAW
```php
$alvin->token(
	array("buying", "-buying.delete", "sales", "-sales.delete"),
	array(
		"owl"=>array(
			"sales" => array("insert"=>"USER-A", "select"=>"SALES-A,SALES-B", "update"=>"USER-A")
		),
		
		"sql"=>array(
			"place" => " (`place` = '1' OR `place` = '2') "
		)
	)
);
```

&nbsp;
___
&nbsp;

## unload
> Desetea el token actual

**[$this]** =  *public* function ( );  

&nbsp;
___
&nbsp;

## username
> Retorna un nombre de usuario válido eliminando de **$sUsername** los caracteres prohibidos  
> Los catacteres permitidos deberán matchear con &#91;^a-zA-Z0-9\_\-\.\@&#93;

**[string]** =  *public* function ( *string* $sUsername );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sUsername**|string||Cadena a validar|

&nbsp;
___
&nbsp;

# Internos
## CheckGrant
> Utilizada por los métodos [analize](#analize) y [check](#check) para validar los permisos

**[mixed]** =  *public* function ( *string* $sGrant, *string* $sToken, *string* $sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sGrant**|string||[Cadena de permisos](alvinuso.md#cadenas-de-permisos) a chequear|
|**$sToken**|string|null|Token del usuario|
|**$sMode**|string|single|Determina el tipo de chequeo que se efectuará:<br /><ul><li><b>analize</b> retorna un array con el análisis de los chequeos</li><li><b>any</b> retorna <b>true</b> cuando al menos 1 de los permisos sea positivo</li><li><b>all</b> retorna <b>true</b> cuando todos los permisos sean positivos</li><li><b>none</b> retorna <b>true</b> cuando todos los permisos sean negativos</li></ul>|

## MakeGroup
> Auxiliar de [grants](#grants). Crea los grupos y perfiles

**[array]** =  *public* function ( *array* $aData, *string* $sType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aData**|array||Array de permisos|
|**$sMode**|string||Determina el tipo si es **profile** ó **group**|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
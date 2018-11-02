# alvin
Tutorial de uso de los tokens [Alvin](alvin.md)
___

# Indice
- [Generar Claves](#generar-claves)
- [Guardar Claves](#guardar-claves)
- [Definir Permisos](#definir-permisos)
- [Generar Token](#generar-token)
- [Chequeos](#chequeos)
- [Valores Crudos](#valores-crudos)
  
&nbsp;

## Generar Claves
```php
$aKeys = $ngl("alvin")->keys();
```
> -----BEGIN RSA PRIVATE KEY-----
> MIIBOwIBAAJBAKlFrrwV34RkZzKmYJjj5ogFcDNvzJYiBsX6jsFWgEFjeH3lDYHn
> OANdVuQsrrTTiG5NlyLnKRP/l/J5b7B4O/ECAwEAAQJBAJRtuPHOkEesLt24DL4k
> IRKnCiLgZtpDDgNuWN1pt18dp/v+7+lQ6VaSGth70cRMWyOI/Ggq9mVXxQlTz9ff
> 5cECIQDTlEbjU7hLdnCuR/d6GvhYprHbtAqoAvUD1g0Q16o/iQIhAMzPiSXTtoTG
> f/c/Nl32cDVzecmRNcrSenUZ6VoGGNcpAiA3Kd69oHNZgXzpg6v7cxKzEmsm7C8n
> FPZK1ME9Ve12eQIhAI0FVTSjiufvWYsfjkqydd6H7VJ51qUZudHJjqA61H3JAiB6
> piCUFJMeUieykS0z8bcpOUBOES0JromWfhVffpJc4Q==
> -----END RSA PRIVATE KEY-----
> 
> -----BEGIN PUBLIC KEY-----
> MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAKlFrrwV34RkZzKmYJjj5ogFcDNvzJYi
> BsX6jsFWgEFjeH3lDYHnOANdVuQsrrTTiG5NlyLnKRP/l/J5b7B4O/ECAwEAAQ==
> -----END PUBLIC KEY-----
  
&nbsp;

## Guardar Claves
- guardar las claves en un lugar seguro
- cuando se utilice un login por ALVIN, almacenar la clave publica en la constante NGL_ALVIN esta debe estar en una sola linea y sin las marcas -----BEGIN PUBLIC KEY----- y -----END PUBLIC KEY-----
```php
define("NGL_ALVIN", "MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAKlFrrwV34RkZzKmYJjj5ogFcDNvzJYiBsX6jsFWgEFjeH3lDYHnOANdVuQsrrTTiG5NlyLnKRP/l/J5b7B4O/ECAwEAAQ==");
```
  
&nbsp;

## Definir Permisos
se debe definir la estructura de permisos en cualquier formato (json, txt, base de datos, array) pero debe ser pasada a ALVIN como array
para generar la estructura se debe tener en cuenta:

- cada componentes de la estructura cuenta de 3 valores ["type","name","grant"]
	- type: grant, group ó profile
	- name: nombre del permiso, grupo o perfil
	- grant: valor del componente
- los nombres de grants, groups y profiles no podran repetirse
- si un permiso, grupo, o perfil es presedido por un signo menos **-**, esto indica que ese permiso se excluirá del token
- el token final incluirá el nombre del perfil, los nombres de grupos en LCASE y los permisos UCASE
- los nombres de los GRANTS que lleven . jerarquizan, ej: "sales" va a generar un token que incluya
	- sales
	- sales.add
	- sales.delete
	- sales.edit
	- sales.view

ejemplo de estructura JSON:
```json
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

## Generar Token
cargamos la estructura de permisos (en formato array) en el objeto
```php
$alvin = $ngl("alvin")->grants(json_decode($sDefinitions,true));
$alvin->setkey($sPrivateKey);
```

generamos el token enviando los nombres de perfiles, grupos y permisos que queremos incluir en un array como primer argumento
```php
$alvin->token(array("admin.boss"));
```

Un token no es igual a un perfil, un token puede contener uno o mas perfiles, grupos o permisos individuales tambien se pueden restar permisos de un perfil utilizando el **-** delante el nombre del componente, por ejemplo: podriamos otorgar el perfin admin.boss sin acceso a users
```php
$alvin->token(array("admin.boss", "-users"));
```
  
&nbsp;

## Chequeos
primero seteamos la clave publica como clave activa y luego cargamos el token si no se especifica una clave publica, se utilizara el valor de la constante NGL_ALVIN
si al ejecutar el metodo load no se especifica un token, el sistema intentará cagar en su lugar la variable ```$_SESSION[NGL_SESSION_INDEX]["ALVIN"]["alvin"]```
```php
$alvin->setkey(NGL_ALVIN);
$alvin->load($sToken);
```
ó
```php
$alvin->load();
```

### Cadenas de Permisos
Los chequeos de permisos, ya sean por el método [analize](alvin.md#analize) ó [check](alvin.md#check) se realizan sobre las *cadenas de permisos*. Estas estan compuestas por los permisos a chequear separados por , (coma). Adicionalmente las cadenas pueden comenzar con los flags **!|** ó **?|**  

|Cadena|Descripción|
|---|---|
|permiso,permiso2,permiso3|Chequea que el usuario cuente con todos los permisos|
|!&#124;permiso,permiso2,permiso3|Chequea que el usuario **NO** cuente con todos los permisos|
|?&#124;permiso,permiso2,permiso3|Chequea que el usuario cuente con al menos uno de los permisos|


### Tipos de chequeos (para el perfil admin.boss del ejemplo):

- individual
```php
$chk = $alvin->check("BUYING>ADD");

#retornará
true
```

- todos los permisos consultados
```php
$chk = $alvin->check("USERS>VIEW,USERS>EDIT");

#retornará
false
```
- cualquiera de los permisos consultados
```php
$chk = $alvin->check("?|USERS>VIEW,USERS>EDIT");

#retornará
true
```

- ninguno de los permisos consultados
```php
$chk = $alvin->check("!|USERS>VIEW,USERS>EDIT");

#retornará
true
```

- analisis de permisos
```php
$chk = $alvin->analize("BUYING>DELETE,BUYING>ADD,BUYING>EDIT,USER>EDIT");

#retornará
Array (
	[BUYING>DELETE] => false
	[BUYING>ADD] => true
	[BUYING>EDIT] => true
	[USER>EDIT] => false
);
```
  
&nbsp;

## Valores Crudos
El token tambien puede albergar valores crudos para que puedan ser luego utilizados en distintos tipos de validaciones.
estos deben ser ingresados en formato array como segundo argumento del método nglAlvin::token, respetando una estructura **clave-valor** estos valores son simplemente almacenados en el token y serán retornados al consultar el método nglAlvin::raw
<br />
El objeto nglOWL cuenta con la característica de poder añadir a cualquier tabla tendria una columna en donde guardar el id de un grupo "owner"
En el token se pueden alamcenar todos los id de grupo con los que puede tener acceso un usuario a los registros de esa tabla (y asi armar un arbol de jerarquias)
Alvin-owl considera 3 tipos de permisos para cada objeto (insert, select y update) y la estructura del token es la siguiente:
```json
{
	"object_name": {
		"insert" : "valor unico",
		"select" : "*",
		"update" : "valores,separados,por,comas",
	},
	
	"object_name_2": {
		"insert" : "valor unico",
		"select" : "valores,separados,por,comas",
		"update" : "valores,separados,por,comas",
	}
}
```

Ej:
- "sales" es un objeto OWL (tabla) de nuestra base de datos donde se registran las ventas
- cada vendedor ve solo sus ventas
- cada supervisor ve las ventas de sus vendedores
- el gerente ve todas las ventas

- para el vendedor A se crea el grupo SALES-A
- para el vendedor B se crea el grupo SALES-B
- para el vendedor C se crea el grupo SALES-C
- para el vendedor C se crea el grupo SALES-D

- el supervidor A, con grupo SUPER-A, ve las ventas de SALES-A y SALES-B
- el supervidor B, con grupo SUPER-B,  ve las ventas de SALES-C y SALES-D
- el gerente, con grupo GERENTE, ve todas las ventas SALES-A, SALES-B, SALES-C y SALES-D

los tokens RAW para el objeto OWL serian:
``` js
vendedorA = {
	"sales": {
		"insert" : "SALES-A",
		"select" : "SALES-A",
		"update" : "SALES-A"
	}
};

supervidorA = {
	"sales": {
		"insert" : "SUPER-A",
		"select" : "SALES-A,SALES-B",
		"update" : "SUPER-A,SALES-A,SALES-B"
	}
};

gerente = {
	"sales": {
		"insert" : "GERENTE",
		"select" : "*",
		"update" : "*"
	}
};
```

La idea de utilizar valores RAW es poder guardar todo tipo de datos que puedan ser utilizados como filtro:
- sentencias SQL WHERE
- código PHP
- valores planos

luego de recuperar los datos mediante nglAlvin::raw, todo quedará después a criterio del desarrollador 


### Creacion y recuperacion de un token con valores crudos:
```php
$alvin->token(
	array("admin.boss", "-users"),
	array(
		"owl"=>array(
			"sales" => array("insert"=>"SUPER-A", "select"=>"SALES-A,SALES-B", "update"=>"SALES-A"),
			"inventory" => array("insert"=>"SUPER-A", "select"=>"INVENT-A", "update"=>"INVENT-A"),
		),
		
		"sql"=>array(
			"by_cash" => " `payment` = 'cash' ",
			"by_place" => " (`place` = '1' OR `place` = '2') ",
		),
		
		"403"=>array(
			"/mods/users/insert" => true,
			"/mods/users/update" => true,
			"/mods/users/delete" => true
		)
	)
);
```

### chequeos
```php
$alvin->raw("owl")["inventory"];
$alvin->raw("sql")["by_place"];
$alvin->raw("403")[$ngl("sysvar")->SELF["url"]];
```

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
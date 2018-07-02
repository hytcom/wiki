# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# url
## nglURL *extends* nglStd [instanciable] [20150306]
Parsea URLs en sus diferentes componentes, permite actualizarlos y volver conformar la dirección
  
## Variables
`private` $sURL = URL activa  

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**url**|string||URL|
|**argument**|string||Argumento que desea modificarse.|
|**value**|mixed||Nuevo valor para Argument.|

## Atributos
|Atributo|Tipo|Descripción|
|---|---|---|
|**basename**|string|nombre completo del archivo|
|**url**|string|URL completa|
|**dirname**|string|directorio del archivo|
|**extension**|string|extensión del archivo|
|**filename**|string|nombre sin extensión del archivo|
|**fragment**|string|Referencia interna en el documento|
|**host**|string|Dominio|
|**path**|string|Path completo del archivo apuntado|
|**params**|string|Request query parseado como array|
|**pass**|string|Contraseña (en caso de existir)|
|**port**|int|Puerto|
|**query**|string|Request query|
|**scheme**|string|Protocolo|
|**ssl**|boolean|Determina si el protocolo es un protocolo seguro|
|**user**|string|Nombre de usuario (en caso de existir)|

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[QueryString](#QueryString)||
|[get](#get)|Retorna la URL activa, ya sea la cargada por el aguamento URL o la última generada por el método **nglURL::unparse**|
|[parse](#parse)|Descompone una URL en los atributos del objeto y retorna los datos en un array asociativo|
|[parts](#parts)|Retorna todas las partes de la URL previamente generadas por **nglURL::parse** que estos pueden haber sufrido actualizaciones|
|[unparse](#unparse)|Compone una URL uniendo los atributos del objeto|
|[update](#update)|Permite actualizar las distintas partes de la URL basandose en las partes generadas **nglURL::parse**|

  
&nbsp;


## get
Retorna la URL activa, ya sea la cargada por el aguamento URL o la última generada por el método **nglURL::unparse**.  

**[string]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## parse
Descompone una URL en los atributos del objeto y retorna los datos en un array asociativo. Si **\$sURL** no es una cadena, retornará FALSE  

**[$this]** =  *public* function ( *string* \$sURL );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sURL**|string||URL|
### Ejemplos  
#### Ejemplo  
```php
$url = "http://example:pass@hytcom.net:80/nogal/help/class.php?q=nglURL::parse#privates";
$http = $ngl("url.foo")->parse($url)->parts();
print_r($http);

# resultado
Array (
    [basename] => "class.php"
    [dirname] => "/nogal/help"
    [extension] => "php"
    [filename] => "class"
    [fragment] => "privates"
    [host] => "hytcom.net"
    [pass] => "pass"
    [params] => Array (
        [ q ] => "nglURL::parse"
    )
    [path] => "/nogal/help/class.php"
    [port] => "80"
    [query] => "q=nglURL::parse"
    [scheme] => "http"
    [ssl] => false
    [user] => "example"

)
```

&nbsp;
___
&nbsp;

## parts
Retorna todas las partes de la URL previamente generadas por **nglURL::parse** que estos pueden haber sufrido actualizaciones.  

**[array]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## unparse
Compone una URL uniendo los atributos del objeto. Si el parámetro **query** es ignorado si existe **params**, ya que estos pueden haber sufrido actualizaciones.  

**[string]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## update
Permite actualizar las distintas partes de la URL basandose en las partes generadas **nglURL::parse**.
**nglURL::parse** se autoejecutará si aún no ha sido ejecutado y está seteado el argumento **url**.  

**[$this]** =  *public* function ( *string* \$sPart, *mixed* \$mValue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPart**|string||Argumento que desea modificarse.|
|**\$mValue**|mixed||Nuevo valor para Argument.|
### Ejemplos  
#### Ejemplo  
```php
$ngl("url.foo")->url = "http://www.hytcom.net/nogal/help/class.php?q=nglURL::parse#privates";
$ngl("url.foo")->update("host", "hytcom.net");
$ngl("url.foo")->update("fragment", "publics");
echo $ngl("url.foo")->unparse();

# salida
"http://hytcom.net/nogal/help/class.php?q=nglURL::parse#publics"
```

&nbsp;
___
&nbsp;

## QueryString
Convierte un Array en una cadena de variables válidad para ser enviada vía GET o POST  

**[string]** =  *private* function ( );
  

&nbsp;
___
&nbsp;

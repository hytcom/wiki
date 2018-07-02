# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# sysvar
## nglSystemVars *extends* nglTrunk [main] [20140202]
Establece y almacena las variables por sistema de NOGAL.<ul><li>**ERRORS** =  códigos de error</li><li>**NULL** =  valor NULL</li><li>**REGEX** =  expresiones regulares frecuentes</li><li>**SELF** =  datos del path del archivo actual</li><li>**UID** =  valor de la variable \$_SESSION[NGL_SESSION_INDEX]["UID"] o NULL</li><li>**VERSION** =  datos de la versión</li></ul>Tambien son capturadas variables \$_REQUEST de uso frecuente previas a la validación por Validate<ul><li>**ID** =  entero o cadena imya ( \$_REQUEST["id"] )</li><li>**Q** =  usualmente utilizada en búsquedas ( \$_REQUEST["q"] )</li></ul>nglSystemVars construye el objeto \$var dentro de NOGAL el cual es accedido a través de: **\$ngl("sysvar")->NOMBRE_DE_VARIABLE**
  
## Variables
`private` $VARS = Contenedor de variables  
`private` $SETTINGS = Valores por default de variables  

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[AccentedChars](#AccentedChars)|Retorna un array asociativo con los caracteres acentuados y su equivalente sin acento, donde la clave es el caracter acentuado y el valor el caracter ...|
|[SetErrors](#SetErrors)|Setea los mensajes de error del sistema en la variable ERRORSSetErrors leera los archivos de configuración de errores de __nogal__/data/errorse inten...|
|[SetIP](#SetIP)|En caso de existir setea el valor de la variable $_SERVER["REMOTE_ADDR"] en la variable IPde lo contrario la setea como NULL|
|[SetRegexs](#SetRegexs)|Almacena expresiones regulares de uso frecuente en la variable REGEX|
|[SetSelf](#SetSelf)|almacena los datos de la ruta del archivo actual en la variable SELF|
|[SetUID](#SetUID)|En caso de existir setea el valor de la variable $_SESSION[NGL_SESSION_INDEX]["UID"] en la variable UIDde lo contrario la setea como NULL|
|[SetVersion](#SetVersion)|Almacena los datos de la versión de NOGAL en la variable VERSION|
|[__get](#__get)|Método mágico encargado de retornar los valores de la variable $VARS cuando es invocada por medio de $ngl("sysvar")si no se especifica un nombre de ...|

  
&nbsp;


## __get
Método mágico encargado de retornar los valores de la variable \$VARS cuando es invocada por medio de **\$ngl("sysvar")**
si no se especifica un nombre de variable se retornarán todas las variables de sistema.  

**[mixed]** =  *public* function ( *string* \$sVarname );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sVarname**|string|null|Nombre de variable|
### Ejemplos  
#### llamada de variable  
```php
echo $ngl("sysvar")->foo;
```
#### llamada global  
```php
print_r($ngl("sysvar"));
```

&nbsp;
___
&nbsp;

## AccentedChars
Retorna un array asociativo con los caracteres acentuados y su equivalente sin acento, 
donde la clave es el caracter acentuado y el valor el caracter sin acentuar  

**[array]** =  *private* function ( );
  

&nbsp;
___
&nbsp;

## SetErrors
Setea los mensajes de error del sistema en la variable **ERRORS**
SetErrors leera los archivos de configuración de errores de **__nogal__/data/errors**
e intentará leer los archivos de la carpeta **ngl/conf** del proyecto  

**[array]** =  *private* function ( );
  

&nbsp;
___
&nbsp;

## SetIP
En caso de existir setea el valor de la variable \$_SERVER["REMOTE_ADDR"] en la variable **IP**
de lo contrario la setea como NULL  

**[string o null]** =  *private* function ( );
  

&nbsp;
___
&nbsp;

## SetSelf
almacena los datos de la ruta del archivo actual en la variable **SELF**  

**[array]** =  *private* function ( );
  
### Ejemplos  
#### /www/htdocs/foo/bar.php  
```php
[b]basename:[/b] bar.php
[b]dirname:[/b] foo
[b]extension:[/b] php
[b]filename:[/b] bar
[b]fullpath:[/b] /www/htdocs/foo/bar.php
[b]path:[/b] /www/htdocs/foo
```
#### http://localhost/foo/bar.php?val=demo  
```php
[b]basename:[/b] bar.php
[b]dirname:[/b] foo
[b]extension:[/b] php
[b]filename:[/b] bar
[b]fullpath:[/b] /www/htdocs/foo/bar.php
[b]path:[/b] /www/htdocs/foo
[b]query_string:[/b] val=demo
[b]url:[/b] foo/bar.php?val=demo
```

&nbsp;
___
&nbsp;

## SetRegexs
Almacena expresiones regulares de uso frecuente en la variable **REGEX**  

**[array]** =  *private* function ( );
  
### Ejemplos  
#### Expresiones  
```php
[b]base64:[/b] caracteres permitidos en una cadena base64
[b]color:[/b] color en formato hexadecimal #RGB, #RRGGBB ó #RRGGBBAA
[b]date:[/b] fecha en formato yyyy-mm-dd
[b]datetime:[/b] fecha y hora en formato yyyy-mm-dd hh-ii-ss
[b]email:[/b] dirección de correo
[b]filename:[/b] formato windows y Linux
[b]fulltag:[/b] etiqueta HTML completa: <div id="foo"...>Lorem ipsum dolor sit amet...</div>
[b]imya:[/b] cadena imya
[b]ip:[/b] dirección IPv4
[b]phpvar:[/b] nombre de variable PHP
[b]tag:[/b] etiqueta HTML, sólo apertura: <div id="foo"...>
[b]time:[/b] hora en formato hh:ii o hh:ii:ss
[b]url:[/b] dirección URL/FTP
```

&nbsp;
___
&nbsp;

## SetUID
En caso de existir setea el valor de la variable \$_SESSION[NGL_SESSION_INDEX]["UID"] en la variable **UID**
de lo contrario la setea como NULL  

**[int o null]** =  *private* function ( );
  

&nbsp;
___
&nbsp;

## SetVersion
Almacena los datos de la versión de NOGAL en la variable **VERSION**  

**[array]** =  *private* function ( );
  

&nbsp;
___
&nbsp;

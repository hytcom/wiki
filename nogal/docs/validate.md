# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# validate
## nglValidate *extends* nglTrunk [main] [20151027]
Validación de variables basada en reglas.

nglValidate construye el objeto \$validate dentro del framework, el cual es accedido a través de: \$**ngl("validate")->NOMBRE_DE_METODO(...)**

Cuenta con dos valores de configuración que impactan en el procesamiento de la variable **\$_REQUEST**:<ul><li>**proccess** =  Determina si se procesa o no la variable **\$_REQUEST**. Default: **1**</li><li>**from** =  Origen de la solicitud (HTTP_REFERER) LOCAL | ALL | HOST1,HOST2,HOSTn | IP1,IP2,IPn. Default: **LOCAL**</li></ul>Los tipos de validaciones soportadas por la clase son<<ul><li>**all** =  Cualquier tipo de contenido</li><li>**alpha** =  Cadena compuesta unicamente por letras y espacios</li><li>**color** =  Color en formato hexadecimal #RGB, #RRGGBB ó #RRGGBBAA</li><li>**date** =  Fecha en formato YYYY-mm-dd</li><li>**datetime** =  Fecha y hora en formato YYYY-mm-dd HH:ii:ss</li><li>**email** =  Dirección de correo</li><li>**filename** =  Nombre de archivo</li><li>**html** =  Cualquier tipo de contenido. El valor será tratado con HTMLENTITIES</li><li>**imya** =  IMYA</li><li>**int** =  Números enteros [^0-9]</li><li>**number** =  Números formateados [0-9\.\,\-]</li><li>**ipv4** =  Dirección IPV4</li><li>**ipv6** =  Dirección IPV6</li><li>**regex** =  Validación por expresiones regulares. La expresión regular es pasada por medio de la option **pattern**</li><li>**text** =  Cadena compuesta por letras, números, simbolos de uso frecuente y espacios</li><li>**time** =  Hora en formato YYYY-mm-dd HH:ii:ss</li><li>**url** =  URL http o ftp, segura o no</li><li>**string** =  Cadena compuesta por letras, números y espacios</li><li>**symbols** =  Solo símbolos y espacios</li></ul>
  
## Variables
`private` $bCheckError = Control de ocurrencia de errores  
`private` $mVariables = Control de ocurrencia de errores  
`private` $vVariables = Variables utilizadas dentro de las reglas  
`private` $vTypes = Tipos de validaciones soportadas  

  
&nbsp;

# Métodos
- [CheckValue](#CheckValue)
- [ClearCharacters](#ClearCharacters)
- [GetRulesFile](#GetRulesFile)
- [RequestFrom](#RequestFrom)
- [ValidateByType](#ValidateByType)
- [addvar](#addvar)
- [request](#request)
- [resetvars](#resetvars)
- [validate](#validate)

  
&nbsp;


## addvar
Almacena valores que luego pueden ser utilizados como variables dentro de las reglas. Esto es especialmente util dentro de los archivos **.json**.
Este método retorna el propio objeto, a fin de poder añadir varias variables con una sintaxis cómoda.  

**[object]** =  *public* function ( *string* \$sVarName, *string* \$mValue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sVarName**|string||Nombre que luego se utilizará para invocar el valor|
|**\$mValue**|string||Valor|
### Ejemplos  
#### Modo de empleo  
```php
[b][u]Variables PHP[/u][/b]
$var1 = "string";
$var2 = "milk,rice,sugar,tea";


[b][u]Añadiendo valores[/u][/b]
$ngl("validate")
	->addvar("type", $var1)
	->addvar("minlen", "3")
	->addvar("list", $var2)
;


[b][u]Archivo foobar.json[/u][/b]
{
	"type" : "{$type}",
	"minlength" : "{$minlen}",
	"in" : "{$list}"
}


[b][u]Execución[/u][/b]
echo $ngl("validate")->validate("rice", "foobar");
```

&nbsp;
___
&nbsp;

## CheckValue
Aplica las reglas **\$vRules** sobre la variable **\$mSource**. Este método es auxiliar de **validate**  

**[mixed]** =  *private* function ( *mixed* \$mSource, *array* \$vRules );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mSource**|mixed||Variable o Array a validar|
|**\$vRules**|array||Reglas de validación|

&nbsp;
___
&nbsp;

## ClearCharacters
Retorna una cadena despues de compararla contra **\$aToClean**  

**[string]** =  *private* function ( *string* \$sString, *array* \$aToClean, *boolean* \$bInvert );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Valor|
|**\$aToClean**|array||Array con los caracteres a conservar/eliminar|
|**\$bInvert**|boolean|false|Indica si deben retornarse los valores que se encuentran en **\$aToClean** o los que no se encuentran|

&nbsp;
___
&nbsp;

## GetRulesFile
Obtiene la configuración de un archivo **.json** y la retorna en como un Array  

**[array]** =  *private* function ( *string* \$sRulesFile );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sRulesFile**|string||Nombre del archivo **.json** ubicado en la carpeta **NGL_PATH_VALIDATE**|

&nbsp;
___
&nbsp;

## request
Valida y reemplaza los valores de la variable global **\$_REQUEST** en base al archivo **REQUEST.json**
Este método sobreescribe los valores de **\$_REQUEST**. Para obtener los valores originales se deberán consultar **\$_GET y \$_POST**.
La parametrización del arranque de esta propiedad se efectua desde el archivo **NGL_PATH_CONF/validate.conf**  

**[array]** =  *public* function ( *string* \$sFrom );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFrom**|string|LOCAL|Especifica el origen válido de la solicitud, a fin de impedir consultas no autorizadas.
En caso de que la solicitud provenga de un origen inválido se vaciará **\$_REQUEST**

**Origenes válidos**<ul><li>**ALL** =  Cualquier origen</li><li>**LOCAL** =  Solicitudes provenientes del propio servidor. Este es el comportamiento predeterminado</li><li>**Especificas** =  **Hostnames** y/o direcciones **IP** separados por comas (,)</li></ul>|

&nbsp;
___
&nbsp;

## RequestFrom
Analiza la cadena **\$sFrom** y retorna un array de origines para ser utilizados en **request**  

**[array]** =  *private* function ( *string* \$sFrom );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFrom**|string||Cadena de posibles origenes|

&nbsp;
___
&nbsp;

## resetvars
Desetea las variables seteadas con **ngl:Validate::addvar**  

**[void]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## validate
Valida la variable **\$mVariables** aplicando las reglas **\$mRules**. Si **\$mRules** no está definido, retornará **NULL**  

**[mixed]** =  *public* function ( *mixed* \$mVariables, *mixed* \$mRules, *boolean* \$bIgnoreDefault );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mVariables**|mixed||Variable o Array a validar|
|**\$mRules**|mixed||Conjunto de reglas que se aplicarán para la validación.
\$mRules acepta texto JSON, el nombre de un archivo .json o un Array de datos

**Sintaxsis** = <br />
{
<blockquote>
**nombre_campo**: {
<blockquote>
**type**: Es el único valor obligatorio. Ver tipos
**required** : 1 o 0
**default** : Valor utilizado cuando el valor no esté seteado o no cumpla con la validación
**minlength**: Longuitud mínima del valor
**maxlength**: Longuitud máxima del valor
**lessthan**: El valor debe ser menor a ... valor alfanumerico
**greaterthan**: El valor debe ser mayor a ... valor alfanumerico
**in**: Lista de valores separados por coma

**options**: [
<blockquote>
**addslashes** : Especifica si el valor debe ser tratado con **addslashes** después de ser validado
**allow** : Tipos o grupos de caracteres UTF-8 (nglUnicode::groups) mas el grupo especial **SPACES**, separados por coma, que aceptará el campo (tipo ALL)
**decode** : Especifica si el valor debe ser tratado con **urldecode** o **rawurldecode** antes de ser validado
**deny** : Tipos o grupos de caracteres UTF-8 (nglUnicode::groups) mas el grupo especial **SPACES**, separados por coma, que aceptará el campo (tipo ALL)
**encoding** : Juego de caracteres empleado durante la conversión htmlentities (tipo HTML)
**htmlentities** : Flags utilizados durante la conversión htmlentities (tipo HTML)
**multiple** : Caracter utilizado para separar el valor de la variable y convertirla en un array de valores. Todas la variables pueden ser multiples
**pattern** : Expresión regular (tipo REGEX)
**quotemeta** : Especifica si el valor debe ser tratado con **quotemeta** después de ser validado
**striptags** : Determina cuales son las etiquetas permitidas para aplicar **strip_tags** sobre el valor antes de ser validado (tipos: ALL, ALPHA, HTML, TEXT, STRING)
</blockquote>
]
</blockquote>
}
</blockquote>
}

Cuando las reglas son del tipo JSON, archivo o texto, se pueden invocar valores variables agregados por medio de **addvar**, utilizando la sintaxsis {\$nombre_variable}

Nota: el grupo especial **SPACES** contiene a los caracteres:<ul><li>tabulación (ord:9)</li><li>salto de línea (ord:10)</li><li>retorno de carro (ord:13)</li><li>espacio (ord:32)</li></ul>|
|**\$bIgnoreDefault**|boolean|false|Desactiva el uso de valores **default**|
### Ejemplos  
#### Reglas JSON en archivo .json  
```php
El archivo debe estar alojado en la carpeta <b>NGL_PATH_VALIDATE</b> del proyecto

$age = 22; 
$age = $ngl("validate")->validate($age, "rules_age");
```
#### Reglas JSON en línea  
```php
$rules = "{
	"type" : "regex",
	"options" : { "pattern" : "\\$[0-9]+" },
	"minlength" : 5
}";

$var = "$522"; 
$var = $ngl("validate")->validate($var, $rules);
```
#### Reglas en Array  
```php
$rules = array();
$rules["type"] = "html";
$rules["options"] = array(
	"htmlentities" => "ENT_COMPAT,ENT_HTML401,ENT_QUOTES",
	"striptags" => "<i>"
);

$var = "<b>prueba de 'validación' de datos <i>HTML</i></b>";
$var = $ngl("validate")->validate($var, $rules);
```
#### Validación multiple  
```php
$rules = "{
	"type" : "email",
	"options" : { "multiple" : ";" }
}";

$emails = "mail1@foobar.com;mail2@foobar.com;mail3@foobar.com";
print_r($ngl("validate")->validate($emails, $rules));
```
#### Validación de Arrays  
```php
$rules = "{
	"product" : {
		"type" : "string",
		"default" : "milk",
		"in" : "milk,rice,sugar,tea"
	},
	
	"quantity" : {
		"type" : "int",
		"greaterthan" : "1",
		"lessthan" : "20",
	},
	
	"price" : {
		"type" : "regex",
		"options" : { "pattern" : "\\$[0-9]+" },
		"minlength" : 5
	}
}";

print_r($ngl("validate")->validate($_POST, $rules));
```
#### Tipo all customizado  
```php
$rules = array();
$rules["type"] = "all";
$rules["options"] = array(
	"allow" => "LATIN_BASIC_LOWERCASE,LATIN_BASIC_NUMBERS",
	"striptags" => true
);

$var = "<b>ESTA ES 'la prueba' <i>1234</i></b>";
$var = $ngl("validate")->validate($var, $rules);

# retornara: laprueba1234
```

&nbsp;
___
&nbsp;

## ValidateByType
Validador de variables por tipo  

**[string]** =  *private* function ( *string* \$mValue, *string* \$sType, *array* \$vOptions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mValue**|string||Valor|
|**\$sType**|string||Tipo de validación|
|**\$vOptions**|array|array()|Parámetros auxiliares de la validación|

&nbsp;
___
&nbsp;

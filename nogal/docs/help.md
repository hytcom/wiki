# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# help
## nglHelp *extends* nglTrunk [main] [20160201]
Este objeto brinda ayuda sobre todos los métodos del framework.
nglHelp construye el objeto \$help dentro del framework, el cual es accedido a través de: **\$ngl("help")->NOMBRE_DE_METODO(...)**
  
## Variables
`private` $sLang = Idioma en el que se mostrará la ayuda  
`private` $sPath = Ruta de carpeta que contiene el contenido de la ayuda  
`private` $sStyles = Contenido de la hoja de estilos utilizada para formatear los datos  
`private` $vThesaurus = Diccionario de titulos y mensajes basado en el idioma  

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[ParamsBox](#ParamsBox)||
|[PrintClass](#PrintClass)||
|[PrintList](#PrintList)||
|[PrintMethod](#PrintMethod)||
|[Template](#Template)||
|[VarsBlocks](#VarsBlocks)||
|[VarsBlocksExplain](#VarsBlocksExplain)||
|[VarsExplain](#VarsExplain)||
|[about](#about)||
|[language](#language)||
|[varsFunctions](#varsFunctions)||

  
&nbsp;


## about
Retorna un documento HTML con la ayuda solicitada  

**[string]** =  *public* function ( *string* \$sWhat, *string* \$sBaseURL );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sWhat**|string|null|Clase, Método o Comando sobre del cual se desea ver el texto de ayuda.<ul><li>**HELP** =  Retorna el listado de Clases disponibles</li><li>**ClassName** =  para obtener ayuda acerca de una clase</li><li>**ClassName::MethodName** =  para obtener ayuda sobre el método **MethodName** de la clase **ClassName**</li></ul>Si existe la variable \$_SERVER["QUERY_STRING"] y es distinta de empty, su valor será asignado a **\$sWhat**|
|**\$sBaseURL**|string|NGL_URL/help/|Especifica la URL predeterminada para todos los enlaces.|
### Ejemplos  
#### Tipos de llamados  
```php
# Listado de Clases
echo $ngl("help")->about();

# Listado de Clases (2)
echo $ngl("help")->about("HELP");
# los links serán del tipo: http://dominio.com/help/

# Clases
echo $ngl("help")->about("nglDBMySQL");

# Método
echo $ngl("help")->about("nglDBMySQL::query");

# Cambio de baseURL
echo $ngl("help")->about("HELP", "/projectname/docs/");
# los links serán del tipo: http://dominio.com/projectname/docs/help/
```

&nbsp;
___
&nbsp;

## language
Setea el idioma en el cual se mostrará la ayuda  

**[void]** =  *public* function ( *string* \$sLang );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sLang**|string|ES|Sigla de 2 letras que representa al idioma|

&nbsp;
___
&nbsp;

## ParamsBox
Retorna la porción de código HTML que muestra los bloques de parámetros  

**[string]** =  *private* function ( *string* \$sTitle, *array* \$aData, *mixed* \$mSelected );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sTitle**|string||Título de la caja|
|**\$aData**|array||Información que se mostrará en la caja|
|**\$mSelected**|mixed|null|Array o cadena con los argumentos de entrada o salida|

&nbsp;
___
&nbsp;

## PrintClass
Retorna el código HTML que muestra la ayuda de la clase solicitada  

**[string]** =  *private* function ( *string* \$sClassName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sClassName**|string||Nombre de la clase|

&nbsp;
___
&nbsp;

## PrintList
Retorna el código HTML que muestra el listado de clases disponibles  

**[string]** =  *private* function ( );
  

&nbsp;
___
&nbsp;

## PrintMethod
Retorna el código HTML que muestra la ayuda del método solicitado  

**[string]** =  *private* function ( *string* \$sMethod );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sMethod**|string||Nombre del método anteseguido por :: y el nombre de la clase a la cual pertenece|

&nbsp;
___
&nbsp;

## Template
Utilizado para enmarcar el código HTML de la ayuda soliticitada dentro de la plantilla HTML que se retorna al usuario  

**[string]** =  *private* function ( *string* \$sContent );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sContent**|string||Contenido de la ayuda a enmarcar en la plantilla|

&nbsp;
___
&nbsp;

## VarsBlocks
Retorna el código HTML con la variables de un comando para ser mostradas en la descrición del mismo  

**[HTML]** =  *private* function ( *array* \$aData );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData**|array||Variables del comando|

&nbsp;
___
&nbsp;

## VarsBlocksExplain
Retorna el código HTML con la variables de un comando formateadas para ser mostradas en el detalle del mismo  

**[string]** =  *private* function ( *array* \$aData );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData**|array||Variables del comando|

&nbsp;
___
&nbsp;

## VarsExplain
Retorna el código HTML con la variables de un método para ser mostradas en la descrición de la misma  

**[HTML]** =  *private* function ( *array* \$aData, *boolean* \$bEncloseDiv );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData**|array||Variables del método|
|**\$bEncloseDiv**|boolean|false|Determina si el codigo devuelto debe estar contenido en un DIV|

&nbsp;
___
&nbsp;

## varsFunctions
Retorna el código HTML con la variables de un método formateadas para ser mostradas en el detalle de la misma  

**[string]** =  *private* function ( *array* \$aData );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData**|array||Variables del método|

&nbsp;
___
&nbsp;

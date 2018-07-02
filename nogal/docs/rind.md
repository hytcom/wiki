# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# rind
## nglRind *extends* nglStd [instanciable] [20150316]
Motor de plantillas.
  
## Variables
`private` RIND_UID = ID único del objeto.  
`private` RIND_ME = ID del objeto entrecomillado.  
`private` RIND_DOLLAR_SIGN = cadena reservada para el signo $ (variables).  
`private` RIND_QUOTE = cadena reservada para las comillas dentro del pseudo código.  
`private` RIND_HTML_QUOTE = cadena reservada para las comillas dobles utilizadas en las plantillas.  
`private` RIND_RESERVED = cadena reservada para la marca de palabras reservadas.  
`private` RIND_DC1 = cadena reservada para el dígito control 1.  
`private` RIND_DC2 = cadena reservada para el dígito control 2.  
`private` RIND_DC3 = cadena reservada para el dígito control 3.  
`private` RIND_LC_BRACKET = cadena reservada para la llave de apertura { .  
`private` RIND_RC_BRACKET = cadena reservada para la llave de cierre } .  
`private` RIND_FUN_OPEN = cadena reservada para la apertura de función.  
`private` RIND_FUN_CLOSE = cadena reservada para el cierre de función.  
`private` RIND_VAR_OPEN = cadena reservada para la apertura de variable.  
`private` RIND_VAR_CLOSE = cadena reservada para el cierre de variable.  
`private` RIND_PHP_OPEN = cadena reservada para la apertura PHP.  
`private` RIND_PHP_CLOSE = cadena reservada para el cierre PHP.  
`private` RIND_HDV_OPEN = cadena reservada para la apertura de variable en JSON.  
`private` RIND_HDV_CLOSE = cadena reservada para el cierre de variable en JSON.  
`private` bInitPHPFunctions = indicador de funciones PHP configuradas.  
`private` bInitVarsDenyAllow = indicador de variables PHP configuradas.  
`private` bInitConstants = indicador de constantes PHP configuradas.  
`private` aRindFunctions = nombre de las funciones RIND.  
`private` SET = array con las variables de sistema disponibles en las plantillas.  
`private` aReservedWords = array palabras reservadas en las plantillas.  
`private` vVarsDeny = array de variables reservadas en las plantillas.  
`private` vVarsAllow = array de variables permitidas en las plantillas.  
`private` aLoops = array contenedor de LOOPS de las plantillas.  

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**root**|string|getcwd()|Path principal de la aplicación.|
|**cache**|string|getcwd()|Path del directorio CACHE.|
|**gui**|string|getcwd()|Path del repositorio de plantillas.|
|**scheme**|string|http|Protocolo **http** o **https**.|
|**template**|string||Nombre del archivo plantilla.|
|**cache_file**|string||Nombre del archivo en el directorio **cache**.|
|**vars_deny**|string|NONE|Variables PHP denegadas en las plantillas.|
|**vars_allow**|string|ALL|Variables PHP permitidas en las plantillas.|
|**constants**|string||Listado separado por comas, de constantes PHP permitidas en las plantillas.|
|**php_code**|string||Sentencias PHP que serán incluidas al principio del archivo generado. Este código no será procesado, simplemente será incluído.|
|**php_functions**|string||Listado separado por comas de las funciones PHP permitidas en las plantillas.|
|**loops_limit**|int|1000|Limite para loops numéricos.|
|**var_needle**|string||Base para el nombre de una variable dinámica.|
|**set_index**|string||Nombre del indice a setear.|
|**set_value**|string||Valor a guardar en **set_index**.|
|**set_request_index**|string||Nombre del indice de la variable global **\$_REQUEST** utilizado en nglRind::setSET.|
|**cache_mode**|string|dev|Estable el modo en el que trabajará el cache<ul><li>**none** =  no se utiliza cache, todas las plantilas se leen y procesan cada vez que son llamadas</li><li>**dev** =  se leen y procesan las nuevas y se leen las ya generadas, pero solo se procesan en caso de detectarse cambios</li><li>**use** =  se leen y procesan sólo las nuevas, las ya generadas simplemente se invocan</li><li>**cache** =  sólo se invocan las ya generadas, no se procesan nuevas</li></ul>|
|**fill_urls**|boolean|true|Activa el autocompletar del fullpath de las url en los **href, src y background**|
|**http_support**|boolean|true|Admite urls en los paths de archivos.|
|**include_support**|boolean|true|Habilita el uso de **mergefile**.|
|**clear_utf8_bom**|boolean|true|Elimina la marca BOM de los archivos UTF-8.|

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[ClearCode](#ClearCode)|Realiza limpieza del código generado por la clase antes de devolverlo como plantilla.variables $_SET...|
|[ClearHyphenArguments](#ClearHyphenArguments)|Elimina de un array todos valores cuyo indice contenga un guión medio.|
|[CommentReservedConstants](#CommentReservedConstants)|Auxiliar de las nglRind::ReservedWords, comenta las constantes reservadas en las plantillas.|
|[CommentReservedFunctions](#CommentReservedFunctions)|Auxiliar de las nglRind::ReservedWords, comenta las funciones no permitidas.|
|[CommentReservedWords](#CommentReservedWords)|Auxiliar de las nglRind::ReservedWords, comenta las palabras reservadas en las plantillas.|
|[ConstantsAllowed](#ConstantsAllowed)|Parsea la cadena $sConstantsAllowed y setea las constantes PHP permitidas en las plantillas en la va...|
|[FillURL](#FillURL)|Reemplaza las URLs relativas por absolutas en el código de las plantilas.|
|[FillURLParser](#FillURLParser)|Auxiliar del método nglRind::FillURL encargado de efectuar los reemplazos.|
|[FixCode](#FixCode)|Analiza el código HTML/NGL y genera un código de etiquetas previo al código PHP final.|
|[GetCommand](#GetCommand)|Captura los comandos rind:cmd_ini: posición de inicio del comando en el código fuente.cmd_end: posic...|
|[IfcaseInline](#IfcaseInline)|Auxiliar del método nglRind::rindIfcase para los casos de if inline.|
|[InNotInArgument](#InNotInArgument)|Auxiliar del método nglRind::rindIfcase, retorna el código que verifica la existencia de un argument...|
|[IsTemplateFile](#IsTemplateFile)|Chequea si el path de la plantilla es válido.|
|[IssetArgument](#IssetArgument)|Auxiliar del método nglRind::rindIfcase, retorna el código que verifica la existencia de un argument...|
|[LoopVarName](#LoopVarName)|Auxiliar del método nglRind::rindLoop, gestiona los nombres de loops.|
|[MakeMatch](#MakeMatch)|Auxiliar de nglRind::TagReader, genera el código de una variable.|
|[PHPFunctions](#PHPFunctions)|Parsea la cadena $sAllowedPHPFunctions y setea las funciones PHP permitidas en las plantillas en la ...|
|[PathBuilder](#PathBuilder)|Construye el path de la plantilla activa en base a los argumentos y atributos cargados.|
|[ProcessCode](#ProcessCode)|Procesa el código fuente aplicando:Limpieza de comentariosReemplazo de comandos simplesReemplazo de ...|
|[ProcessConstants](#ProcessConstants)|Retorna el código con las llamadas a las constantes citadas en $_SET["CONSTANTS"].|
|[PutSlashes](#PutSlashes)|Auxiliar de ::InNotInArgument, escapa con slashes los HTML QUOTES de los argumentos.|
|[QuoteArguments](#QuoteArguments)|Chequea y re-entrecomilla los argumentos de los comandos.|
|[ReadTemplate](#ReadTemplate)|Lee una plantilla a una variable.|
|[ReplaceCommands](#ReplaceCommands)|Reemplaza comandos RIND por código PHP.|
|[ReplaceConstants](#ReplaceConstants)|Auxiliar del método nglRind::Constants para el reemplazo de constantes.|
|[ReservedStrings](#ReservedStrings)|Reemplaza las palabras reservadas por las variables $RIND_... y viceversa.|
|[ReservedWords](#ReservedWords)|Detecta y reemplaza las funciones, las palabras reservadas y constantes en las plantillas.|
|[SetPaths](#SetPaths)|Arma y setea los paths utilizados por el objeto.Este método deberá ser ejecutado siempre que los val...|
|[SingleCommands](#SingleCommands)|Ejecuta los comandos simples: abort, once y skip.|
|[StripPHP](#StripPHP)|Elimina el código PHP presente en las plantillas.|
|[TagConverter](#TagConverter)|Reemplaza las etiquetas rind por cadenas simplificadas y viceversa.|
|[TagReader](#TagReader)|Lee y parsea etiquetas rind.|
|[VarsDenyAllow](#VarsDenyAllow)|Establece la politica de variables PHP están permitidas en las plantillas.Si el valor de $sVariables...|
|[VarsEscape](#VarsEscape)|Detecta y reemplaza las funciones, las palabras reservadas y constantes en las plantillas.|
|[VarsParser](#VarsParser)|Parser de variables.|
|[VarsProcessor](#VarsProcessor)|Procesa los nombres de las variables.|
|[dynVar](#dynVar)|Genera un nombre de variable aleatorio o basado en una semilla de 8 caracteres de longuitud.|
|[flushCache](#flushCache)|Elimina todas las carpetas y archivos del cache|
|[getRINDVariable](#getRINDVariable)|Retorna el valor hash asignado a cada una de las variables RIND. Este método tiene aplicación dentro...|
|[getSET](#getSET)|Retorna el valor de un índice de la variable $_SET.Cuando el valor sea NULL, se retornarán todo el a...|
|[process](#process)|Procesa una plantilla, cachea el código PHP generado y retorna el path del cache.|
|[quick](#quick)|Ejecuta la plantilla $sFileName sobre el archivo actual y retorna el código resultante.Este método n...|
|[rindDump](#rindDump)|Retorna el código que genera volcados en pantalla rind:dump en las plantillas.|
|[rindEco](#rindEco)|Retorna el código que imprime un valor usando rind:eco en las plantillas.|
|[rindGet](#rindGet)|Retorna un id unico utilizando ngl::unique en las plantillas.|
|[rindHalt](#rindHalt)|Retorna el código que detiene un ejecución usuando rind:halt en las plantillas.|
|[rindHeredoc](#rindHeredoc)|Retorna el código de una expresión de rind:rtn en las plantillas.|
|[rindIfcase](#rindIfcase)|Genera el código PHP para el comando rind:ifcase del sistema de plantillas.|
|[rindIncFile](#rindIncFile)|Retorna el código que incluye un archivo válido en $_SET["INCLUDES"], en las plantillas.|
|[rindJoin](#rindJoin)|Convierte un string separado por comas en un array.|
|[rindJson](#rindJson)|Retorna el código de una variable string generada con rind:strvar en las plantillas.|
|[rindLength](#rindLength)|Retorna el código que devuelve el largo de un valor usando rind:length en las plantillas.|
|[rindLoop](#rindLoop)|Retorna los distintos códigos de bucles rind:loop en las plantillas.|
|[rindMergeFile](#rindMergeFile)|Detecta y ejecuta las inclusiones de sub-plantillas en la plantilla principal.|
|[rindRtn](#rindRtn)|Retorna el código de una expresión de rind:rtn en las plantillas.|
|[rindSet](#rindSet)|Retorna el código que setea una variable en $_SET usando rind:set en las plantillas.|
|[rindSplit](#rindSplit)|Convierte un array en un string separado por comas.|
|[rindUnSet](#rindUnSet)|Retorna el código que desetea un índice de $_SET usando rind:unset en las plantillas.|
|[rindUnique](#rindUnique)|Retorna un id unico utilizando ngl::unique en las plantillas.|
|[setSESS](#setSESS)|Setea el contenido de la variable $_SESSION[NGL_SESSION_INDEX]["SESS"] en el índice SESS de la varia...|
|[setSET](#setSET)|Setea un valor en la variable $_SET, disponible en las plantillas.Si $sRequested es distinto de NULL...|
|[showPaths](#showPaths)|Muestra los paths con los que está seteado el objeto.|
|[stamp](#stamp)|Ejecuta el método nglRind::process y ejecuta el archivo generado.|
|[stripQuotes](#stripQuotes)|Elimina el primer par de comillas dobles del principio y fin.|
|[varName](#varName)|Genera un nombre de variable aleatorio de entre 1 y 32 caracteres de loguitud.|

  
&nbsp;


## ClearCode
Realiza limpieza del código generado por la clase antes de devolverlo como plantilla.<ul><li>variables \$_SET</li><li>reemplazo de RIND_HTML_QUOTE por comillas doble</li><li>reemplazo de RIND_QUOTE por comillas doble</li><li>limpieza de doble comillas concatenadas</li><li>restitucion de caracteres RIND</li><li>palabras reservadas, variables y funciones denegadas</li><li>RIND_DOLLAR_SIGN por \$</li></ul>  

**[string]** =  *private* function ( *string* \$sSource );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sSource**|string||Código fuente|

&nbsp;
___
&nbsp;

## ClearHyphenArguments
Elimina de un array todos valores cuyo indice contenga un guión medio.  

**[array]** =  *private* function ( *array* \$aArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aArguments**|array||Array de valores.|

&nbsp;
___
&nbsp;

## CommentReservedConstants
Auxiliar de las **nglRind::ReservedWords**, comenta las constantes reservadas en las plantillas.  

**[array]** =  *private* function ( *array* \$aMatchs );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aMatchs**|array||Constantes detectas en las plantillas.|

&nbsp;
___
&nbsp;

## CommentReservedFunctions
Auxiliar de las **nglRind::ReservedWords**, comenta las funciones no permitidas.  

**[array]** =  *private* function ( *array* \$aMatchs );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aMatchs**|array||Funciones de no permitidas detectas en las plantillas.|

&nbsp;
___
&nbsp;

## CommentReservedWords
Auxiliar de las **nglRind::ReservedWords**, comenta las palabras reservadas en las plantillas.  

**[array]** =  *private* function ( *array* \$aMatchs );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aMatchs**|array||Palabras reservadas detectas en las plantillas.|

&nbsp;
___
&nbsp;

## ProcessConstants
Retorna el código con las llamadas a las constantes citadas en **\$_SET["CONSTANTS"]**.  

**[string]** =  *private* function ( *string* \$sCode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCode**|string||Código a analizar.|

&nbsp;
___
&nbsp;

## ConstantsAllowed
Parsea la cadena **\$sConstantsAllowed** y setea las constantes PHP permitidas en las plantillas en la variable **\$_SET["CONSTANTS"]**.  

**[void]** =  *protected* function ( *string* \$sAllowedPHPFunctions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sAllowedPHPFunctions**|string||Listado de constantes separado por comas.|

&nbsp;
___
&nbsp;

## dynVar
Genera un nombre de variable aleatorio o basado en una semilla de 8 caracteres de longuitud.  

**[string]** =  *public* function ( *string* \$sNeedle );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sNeedle**|string||Base para el nombre de una variable dinámica.|

&nbsp;
___
&nbsp;

## FillURL
Reemplaza las URLs relativas por absolutas en el código de las plantilas.  

**[string]** =  *private* function ( *string* \$sSource );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sSource**|string||Código a fuente.|

&nbsp;
___
&nbsp;

## FillURLParser
Auxiliar del método **nglRind::FillURL** encargado de efectuar los reemplazos.  

**[string]** =  *private* function ( *string* \$sSource, *array* \$aURLs, *string* \$sURLSelf, *string* \$sTemplateURL );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sSource**|string||Código a fuente.|
|**\$aURLs**|array||Array con las urls detectadas en el código original.|
|**\$sURLSelf**|string||URL del directorio del entorno gráfico.|
|**\$sTemplateURL**|string||URL de la plantilla actual.|

&nbsp;
___
&nbsp;

## FixCode
Analiza el código HTML/NGL y genera un código de etiquetas previo al código PHP final.  

**[array]** =  *private* function ( *array* \$aCode, *string* \$sType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aCode**|array||Código a analizar.|
|**\$sType**|string|FUN|Tipo de etiqueta.|

&nbsp;
___
&nbsp;

## flushCache
Elimina todas las carpetas y archivos del cache  

**[array]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## GetCommand
Captura los comandos **rind**:<ul><li>**cmd_ini** =  posición de inicio del comando en el código fuente.</li><li>**cmd_end** =  posición de fin del comando en el código fuente.</li><li>**function** =  nombre del comando</li><li>**content** =  contenido del argumento **content**</li><li>**source** =  código fuente</li><li>**arguments** =  array de argumentos</li></ul>  

**[array]** =  *private* function ( );
  

&nbsp;
___
&nbsp;

## getSET
Retorna el valor de un índice de la variable **\$_SET**.
Cuando el valor sea NULL, se retornarán todo el array **\$_SET**.  

**[mixed]** =  *public* function ( *string* \$sIndex );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sIndex**|string||Nombre del indice a retornar.|

&nbsp;
___
&nbsp;

## getRINDVariable
Retorna el valor hash asignado a cada una de las variables RIND. Este método tiene aplicación dentro de las librerías XPS  

**[mixed]** =  *public* function ( *string* \$sVarName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sVarName**|string|null|Nombre de variable RIND<ul><li>**RIND_UID** =  = id único</li><li>**RIND_ME** =  = id único entrecomillado</li><li>**RIND_DOLLAR_SIGN** =  = signo \$ (variables)</li><li>**RIND_QUOTE** =  = comillas dentro del pseudo código</li><li>**RIND_HTML_QUOTE** =  = comillas dobles utilizadas en las plantillas</li><li>**RIND_RESERVED** =  = marca de palabras reservadas</li><li>**RIND_DC1** =  = dígito control 1</li><li>**RIND_DC2** =  = dígito control 2</li><li>**RIND_DC3** =  = dígito control 3</li><li>**RIND_LC_BRACKET** =  = llave de apertura {</li><li>**RIND_RC_BRACKET** =  = llave de cierre }</li><li>**RIND_FUN_OPEN** =  = apertura de función</li><li>**RIND_FUN_CLOSE** =  = cierre de función</li><li>**RIND_VAR_OPEN** =  = apertura de variable</li><li>**RIND_VAR_CLOSE** =  = cierre de variable</li><li>**RIND_PHP_OPEN** =  = apertura PHP</li><li>**RIND_PHP_CLOSE** =  = cierre PHP</li><li>**RIND_HDV_OPEN** =  = apertura de variable en JSON</li><li>**RIND_HDV_CLOSE** =  = cierre de variable en JSON</li></ul>|

&nbsp;
___
&nbsp;

## IfcaseInline
Auxiliar del método **nglRind::rindIfcase** para los casos de if inline.  

**[string]** =  *private* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Código del IF.|

&nbsp;
___
&nbsp;

## InNotInArgument
Auxiliar del método **nglRind::rindIfcase**, retorna el código que verifica la existencia de un argumento.  

**[string]** =  *private* function ( *string* \$sIssetArgument );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sIssetArgument**|string||Código a evaluar.|

&nbsp;
___
&nbsp;

## IssetArgument
Auxiliar del método **nglRind::rindIfcase**, retorna el código que verifica la existencia de un argumento.  

**[string]** =  *private* function ( *string* \$sIssetArgument );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sIssetArgument**|string||Código a evaluar.|

&nbsp;
___
&nbsp;

## IsTemplateFile
Chequea si el path de la plantilla es válido.  

**[boolean]** =  *private* function ( *string* \$sTemplateFile );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sTemplateFile**|string||URL de la plantilla.|

&nbsp;
___
&nbsp;

## LoopVarName
Auxiliar del método **nglRind::rindLoop**, gestiona los nombres de **loops**.  

**[string]** =  *private* function ( *string* \$sSource, *string* \$sLoopName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sSource**|string||Código a evaluar.|
|**\$sLoopName**|string||Parte dinámica del nombre.|

&nbsp;
___
&nbsp;

## MakeMatch
Auxiliar de **nglRind::TagReader**, genera el código de una variable.  

**[string]** =  *private* function ( *int* \$nLength, *string* \$sBaseName, *int* \$sCounter );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nLength**|int||Largo del nombre de la variable.|
|**\$sBaseName**|string||Nombre base de la variable.|
|**\$sCounter**|int||Base del contador de la variable.|

&nbsp;
___
&nbsp;

## PathBuilder
Construye el path de la plantilla activa en base a los argumentos y atributos cargados.  

**[string]** =  *protected* function ( *string* \$sFileName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFileName**|string||Nombre del archivo.|

&nbsp;
___
&nbsp;

## PHPFunctions
Parsea la cadena **\$sAllowedPHPFunctions** y setea las funciones PHP permitidas en las plantillas en la variable **\$_SET["PHP_FUNCTIONS"]**.  

**[void]** =  *protected* function ( *string* \$sAllowedPHPFunctions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sAllowedPHPFunctions**|string||Listado de funciones separado por comas.|

&nbsp;
___
&nbsp;

## process
Procesa una plantilla, cachea el código PHP generado y retorna el path del cache.  

**[string]** =  *public* function ( *string* \$sFileName, *string* \$sCacheMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFileName**|string||Nombre del archivo plantilla.|
|**\$sCacheMode**|string|dev|Estable el modo en el que trabajará el cache<ul><li>**none** =  no se utiliza cache, todas las plantilas se leen y procesan cada vez que son llamadas</li><li>**dev** =  se leen y procesan las nuevas y se leen las ya generadas, pero solo se procesan en caso de detectarse cambios</li><li>**use** =  se leen y procesan sólo las nuevas, las ya generadas simplemente se invocan</li><li>**cache** =  sólo se invocan las ya generadas, no se procesan nuevas</li></ul>|

&nbsp;
___
&nbsp;

## quick
Ejecuta la plantilla \$sFileName sobre el archivo actual y retorna el código resultante.
Este método no genere un archivo en el cache.

IMPORTANTE:
Al utilizar el método quick dentro de una clase, se debe tener en cuenta que **nglRind** invoca siempre a las 
variables atraves de la variable \$GLOBALS, por lo que las variables que se pasen a las plantillas deberán setearse 
con la sintaxis: \$GLOBALS["foo"] = "asd123";  

**[string]** =  *public* function ( *string* \$sFileName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFileName**|string||Nombre del archivo plantilla.|

&nbsp;
___
&nbsp;

## ProcessCode
Procesa el código fuente aplicando:<ul><li>Limpieza de comentarios</li><li>Reemplazo de comandos simples</li><li>Reemplazo de constantes</li><li>Limpieza de codigo PHP</li><li>Procesamiento de variables</li><li>Reemplazo de comandos **rind**</li></ul>  

**[boolean]** =  *private* function ( *string* \$sCode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCode**|string||Código fuente.|

&nbsp;
___
&nbsp;

## PutSlashes
Auxiliar de **::InNotInArgument**, escapa con slashes los HTML QUOTES de los argumentos.  

**[string]** =  *private* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena a escapar.|

&nbsp;
___
&nbsp;

## QuoteArguments
Chequea y re-entrecomilla los argumentos de los comandos.  

**[array]** =  *private* function ( *array* \$aArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aArguments**|array||Código fuente.|

&nbsp;
___
&nbsp;

## ReadTemplate
Lee una plantilla a una variable.  

**[string]** =  *private* function ( *string* \$sFileName, *boolean* \$bRINDagsConvert );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFileName**|string||Path de la plantilla.|
|**\$bRINDagsConvert**|boolean||Reemplaza las etiquetas RIND por cadenas simplificadas.|

&nbsp;
___
&nbsp;

## ReplaceCommands
Reemplaza comandos RIND por código PHP.  

**[array]** =  *private* function ( *array* \$aCode, *array* \$vCommand );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aCode**|array||Código fuente.|
|**\$vCommand**|array||Parámetros del comando a reemplazar.|

&nbsp;
___
&nbsp;

## ReplaceConstants
Auxiliar del método **nglRind::Constants** para el reemplazo de constantes.  

**[string]** =  *private* function ( *array* \$aMatches );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aMatches**|array||Array con las constantes detectadas en el código original.|

&nbsp;
___
&nbsp;

## ReservedStrings
Reemplaza las palabras reservadas por las variables **\$RIND_...** y viceversa.  

**[string]** =  *private* function ( *string* \$sCode, *boolean* \$bRevert );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCode**|string||Código fuente.|
|**\$bRevert**|boolean||Indica la dirección del reemplazo, false = ida / true = vuelta.|

&nbsp;
___
&nbsp;

## ReservedWords
Detecta y reemplaza las funciones, las palabras reservadas y constantes en las plantillas.  

**[array]** =  *private* function ( *string* \$sCode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCode**|string||Código fuente.|

&nbsp;
___
&nbsp;

## SetPaths
Arma y setea los paths utilizados por el objeto.
Este método deberá ser ejecutado siempre que los valores de los argumentos involucrados sean alterados para los cambios surgan efecto.  

**[$this]** =  *private* function ( );
  

&nbsp;
___
&nbsp;

## setSESS
Setea el contenido de la variable \$_SESSION[NGL_SESSION_INDEX]["SESS"] en el índice **SESS** de la variable **SET** y lo retorna  

**[$this]** =  *public* function ( *string* \$sIndex );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sIndex**|string||Nombre del indice a retornar.|

&nbsp;
___
&nbsp;

## setSET
Setea un valor en la variable **\$_SET**, disponible en las plantillas.
Si **\$sRequested** es distinto de NULL, se intentará setear el valor de es índice de la variable global **\$_REQUEST**.
El método retornará el valor asignado a **\$sIndex**.
Cuando se utilice setSET en el archivo PHP habrá que asignarle un nombre al objeto **rind**, ya que el utilizar la sintaxis **rind.** causará errores.  

**[$this]** =  *public* function ( *string* \$sIndex, *string* \$mValue, *string* \$sRequested );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sIndex**|string||Nombre del indice a setear.|
|**\$mValue**|string||Valor a guardar en **\$sIndex**.|
|**\$sRequested**|string||Nombre de un indice de la variable global **\$_REQUEST**.|

&nbsp;
___
&nbsp;

## showPaths
Muestra los paths con los que está seteado el objeto.  

**[string]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## SingleCommands
Ejecuta los comandos simples: **abort**, **once** y **skip**.  

**[string]** =  *private* function ( *string* \$sCode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCode**|string||Código fuente.|

&nbsp;
___
&nbsp;

## stamp
Ejecuta el método **nglRind::process** y ejecuta el archivo generado.  

**[string]** =  *public* function ( *string* \$sFileName, *string* \$sCacheMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFileName**|string||Nombre del archivo plantilla.|
|**\$sCacheMode**|string|dev|Estable el modo en el que trabajará el cache<ul><li>**none** =  no se utiliza cache, todas las plantilas se leen y procesan cada vez que son llamadas</li><li>**dev** =  se leen y procesan las nuevas y se leen las ya generadas, pero solo se procesan en caso de detectarse cambios</li><li>**use** =  se leen y procesan sólo las nuevas, las ya generadas simplemente se invocan</li><li>**cache** =  sólo se invocan las ya generadas, no se procesan nuevas</li></ul>|

&nbsp;
___
&nbsp;

## StripPHP
Elimina el código PHP presente en las plantillas.  

**[array]** =  *private* function ( *string* \$sCode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCode**|string||Código fuente.|

&nbsp;
___
&nbsp;

## stripQuotes
Elimina el primer par de comillas dobles del principio y fin.  

**[string]** =  *protected* function ( *string* \$sArgument );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sArgument**|string||Cadena a limpiar.|

&nbsp;
___
&nbsp;

## TagConverter
Reemplaza las etiquetas **rind** por cadenas simplificadas y viceversa.  

**[string]** =  *private* function ( *string* \$sCode, *boolean* \$bRevert );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCode**|string||Código fuente.|
|**\$bRevert**|boolean||Indica la dirección del reemplazo, false = ida / true = vuelta.|

&nbsp;
___
&nbsp;

## TagReader
Lee y parsea etiquetas **rind**.  

**[array]** =  *private* function ( *array* \$aCode, *int* \$nFrom, *string* \$sBreaker, *string* \$sJumper );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aCode**|array||Código fuente.|
|**\$nFrom**|int||Inicio de lectura.|
|**\$sBreaker**|string||Indicador de stop de lectura.|
|**\$sJumper**|string|null|Indicador anidamientos.|

&nbsp;
___
&nbsp;

## varName
Genera un nombre de variable aleatorio de entre 1 y 32 caracteres de loguitud.  

**[string]** =  *protected* function ( *int* \$nLength );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nLength**|int||Largo del nombre de la variable.|

&nbsp;
___
&nbsp;

## VarsDenyAllow
Establece la politica de variables PHP están permitidas en las plantillas.
Si el valor de **\$sVariables** es NULL, se aplicara **\$sType** para todas las variables.  

**[void]** =  *protected* function ( *string* \$sType, *string* \$sVariables );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sType**|string||Política a aplicar, **deny** o **allow**.|
|**\$sVariables**|string|null|Listado de variables (con el signo \$) separadas por comas.|

&nbsp;
___
&nbsp;

## VarsEscape
Detecta y reemplaza las funciones, las palabras reservadas y constantes en las plantillas.  

**[array]** =  *private* function ( *string* \$sCode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCode**|string||Código fuente.|

&nbsp;
___
&nbsp;

## VarsParser
Parser de variables.  

**[array]** =  *private* function ( *array* \$aCode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aCode**|array||Código fuente.|

&nbsp;
___
&nbsp;

## VarsProcessor
Procesa los nombres de las variables.  

**[array]** =  *private* function ( *string* \$sVarName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sVarName**|string||Cadena que contiene el código de la variable.|

&nbsp;
___
&nbsp;

## rindDump
Retorna el código que genera volcados en pantalla **rind:dump** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindEco
Retorna el código que imprime un valor usando **rind:eco** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindHalt
Retorna el código que detiene un ejecución usuando **rind:halt** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindJson
Retorna el código de una variable string generada con **rind:strvar** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindIfcase
Genera el código PHP para el comando **rind:ifcase** del sistema de plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindIncFile
Retorna el código que incluye un archivo válido en **\$_SET["INCLUDES"]**, en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindLength
Retorna el código que devuelve el largo de un valor usando **rind:length** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindLoop
Retorna los distintos códigos de bucles **rind:loop** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindMergeFile
Detecta y ejecuta las inclusiones de sub-plantillas en la plantilla principal.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindRtn
Retorna el código de una expresión de **rind:rtn** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindSet
Retorna el código que setea una variable en **\$_SET** usando **rind:set** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindSplit
Convierte un array en un string separado por comas.  

**[string]** =  *private* function ( *content* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|content||Array de datos.|

&nbsp;
___
&nbsp;

## rindJoin
Convierte un string separado por comas en un array.  

**[array]** =  *private* function ( *content* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|content||Cadena de datos.|

&nbsp;
___
&nbsp;

## rindHeredoc
Retorna el código de una expresión de **rind:rtn** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindGet
Retorna un id unico utilizando **ngl::unique** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindUnique
Retorna un id unico utilizando **ngl::unique** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

## rindUnSet
Retorna el código que desetea un índice de **\$_SET** usando **rind:unset** en las plantillas.  

**[string]** =  *private* function ( *array* \$vArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vArguments**|array||Argumentos del comando.|

&nbsp;
___
&nbsp;

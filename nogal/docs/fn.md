# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# fn
## nglCommon *extends* nglTrunk [main] [20160201]
Compendio de métodos utilizados para resolver tareas rutinarias vinculadas a:<ul><li>arrays</li><li>cadenas</li><li>comprovacion de tipos de datos</li><li>colores</li><li>fechas</li><li>etc</li></ul>nglCommon construye el objeto \$fn dentro del framework, el cual es accedido a través del método **\$ngl("fn")->NOMBRE_DE_METODO(...)**
  
## Variables
`private` $vMimeTypes = MimeTypes obtenidos con nglCommon::apacheMimeTypes  

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[ArrayGrouper](#ArrayGrouper)|M�todo axuliar de nglCommon::arrayGroup|
|[apacheMimeTypes](#apacheMimeTypes)|Retorna los Internet media types indexados por extenci�n proporcionados por el sitio http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime...|
|[arrange](#arrange)|Restituye el orden original de la Cadena o Array $mSource desordenado por nglCommon::disarrange seg�n las posiciones de $aArrange.Es claro que si una...|
|[arrayAppend](#arrayAppend)|A�ade los indices de 1 o mas arrays al array principal, sin importar el tipo de dato y sin sobreescribir indices.Si los indices son del tipo alfanume...|
|[arrayColumn](#arrayColumn)|Devuelve los valores de una sola columna de $aSource, identificado por la clave de columna $mColumnKeyOpcionalmente, se puede proporcionar una clave d...|
|[arrayGoto](#arrayGoto)|Avanza el puntero del array hasta el �ndice indicado por $mKey y retorna los datos|
|[arrayGroup](#arrayGroup)|Agrupa un array bidimensional.Cuando la variable $mStructure sea NULL, los valores �nicos de cada columna se agrupar�n en subarrays.Cuando la variab...|
|[arrayIn](#arrayIn)|Comprueba si un valor se encuentra en un array utilizando in_array o chequeando valor por valor utilizando expresiones regulares. en este �ltimo caso...|
|[arrayInsert](#arrayInsert)|A�ade un elemento al Array en la posici�n determinada|
|[arrayMerge](#arrayMerge)|Agrega N arrays multi-dimensionales en uno|
|[arrayMultiSort](#arrayMultiSort)|Ordena un array multi-dimensional considerando multiples indices, orden y tipos de orden|
|[arrayRebuilder](#arrayRebuilder)|Agrupa los �ndices $aIndexes combinandolos por sus claves. Con la opci�n de reenombrar estas �ltimas.Si $aIndexes = null y $mNewIndexes es una cade...|
|[arrayRepeat](#arrayRepeat)|Retorna un array con indices n�mericos que contiene $nMultiplier repeticiones del array $aInput.|
|[base64Cleaner](#base64Cleaner)|Elimina de una cadena todos los caracteres que no sean v�lidos en una cadena base64 [a-zA-Z0-9+/=]|
|[between](#between)|Verifica si un valor en relaci�n a un rango de valores.between tambien puede ser utilizado para conocer si un valor es mayor o menor a otro, ya que s...|
|[clearPath](#clearPath)|Elimina los slashes de mas en un path o url. Todos los $sSeparator de cierre ser�n eliminados|
|[colorHex](#colorHex)|Retorna un color en valores hexadecimales basandose en RGB|
|[colorRGB](#colorRGB)|Retorna los valores RGB y Transparencia de un color en formato hexadecimal|
|[coockie](#coockie)|Guarda y optiene el valor de una cookie del navegador.Los valores son analizados con ngl::passwd(), por lo que si NGL_PASSWORD_KEY esta activa, los va...|
|[dec2hex](#dec2hex)|Transforma un decimal en hexadecimal sin l�mite de tama�o y con la posibilidad de rellenar con 0 por delante|
|[disarrange](#disarrange)|Desordena de manera c�clica la Cadena o Array $mSource seg�n las posiciones de $aArrange.En la medida en que el desordenamiento avanza sobre $aArran...|
|[dump](#dump)|Retorna el contenido de una variable de acuerdo al tipo de la misma:arrays -> print_rcadenas -> echootros -> var_dumpLos valores son capturados por m?...|
|[emptyToNull](#emptyToNull)|Establece como NULL los valores de $aData, cuyo indice se encuentre en $aKeys, que retornen TRUE a la funcion empty.Si $aKeys es NULL se evaluar�n to...|
|[encoding](#encoding)|Verifica si la cadena $sString se encuentra codificada en $mEncoding$mEncoding debe ser el nombre de una codificaci�n v�lida o un array de nombres.S...|
|[ensureVar](#ensureVar)|Retorna el valor de $mSure cuanto $mVar no esta seteada o es NULL|
|[explodeTrim](#explodeTrim)|Ejecuta la funci�n explode de PHP y a continuaci�n trata a cada uno de los valores con la funci�n trim|
|[exploder](#exploder)|Ejecuta la funci�n explode de PHP de manera recursiva, utilizando los delimitadores para armar un array multi-dimensional|
|[headers](#headers)|Retorna un array con todas las cabeceras enviadas hasta el momento, un una cadena o false para cuando se especifique $sHeader|
|[hex2dec](#hex2dec)|Transforma un hexadecimal en decimal sin l�mite de tama�o|
|[imploder](#imploder)|Une elementos de un array multi dimensional en una cadena.Cuando $mGlue sea declarado como un array, el primer �ndice ser� utilizado para unir los v...|
|[imya](#imya)|Retorna o valida un imya|
|[intPart](#intPart)|Retorna la parte entera de un n�mero|
|[isArrayArray](#isArrayArray)|Comprueba si $aArray es un Array de Arrays. Con $bStrict FALSE s�lo chequear� que el primer valor de $aArray sea un array. Si es TRUE verificar� qu...|
|[isEmpty](#isEmpty)|Comprueba si $mValue esta vac�o. en el caso de que $mValue sea del tipo Array, isEmpty devolver� FALSE si al menosuno de sus �ndices est� vac�o. ...|
|[isInteger](#isInteger)|Comprueba si un valor es un n�mero entero|
|[isJSON](#isJSON)|Comprueba si un valor es una cadena JSON v�lida|
|[isLowerCase](#isLowerCase)|Comprueba si $sString son s�lo letras min�sculas. En el caso de que $mValue sea del tipo Array, isLowerCase devolver� FALSE si al menos en uno de s...|
|[isNull](#isNull)|Comprueba si un valor es NULL.Esto suceder� cuando el m�todo nativo is_null($mValue) retorne true o cuando el valor strtolower($mValue) sea igual a ...|
|[isNumber](#isNumber)|Comprueba si $mNumber es un valor n�merico y retorna su valor en el formato correcto (float o int).Seran considerados n�meros los siguientes formato...|
|[isSerialized](#isSerialized)|Comprueba si $sString es un array serializado. Si $bResult es igual a TRUE el m�todo retornar� un array en caso de TRUE|
|[isTrue](#isTrue)|Comprueba si $mValue es TRUE o FALSE. Si $mValue es String y su valor es '0', 'false', 'null', 'no' u 'off', el valor de retorno ser� FALSE|
|[isURL](#isURL)|Retorna TRUE (o el protocolo) si $sFilePath es una URL http, ftp o comienza con //Para este �ltimo caso, cuando se solicite el protocolo, se retornar...|
|[isUTF8](#isUTF8)|Comprueba si $sString es una cadena UTF-8|
|[isUpperCase](#isUpperCase)|Comprueba si $sString son s�lo letras may�sculas. En el caso de que $mValue sea del tipo Array, isUpperCase devolver� FALSE si al menos en uno de s...|
|[memory](#memory)|Devuelve el valor de la cantidad de memoria asignada a PHP, formateado con strSizeEncode()|
|[mimeType](#mimeType)|Retorna el Mime Type de la extensi�n proporcionada.|
|[once](#once)|Genera o chequea un c�digo �nico guardado en la session activa.Cuando se ejecuta el m�todo sin el argumento $sCode, este generar� un ONCECODE, lo ...|
|[round05](#round05)|Redondea un n�mero al entero o punto medio mas cercano.El par�metro $nPrecition permite controlar la distancia del redondeo al punto medioSeg�n la ...|
|[secureName](#secureName)|Limpia una cadena para que pueda ser utilizada como nombre de archivo, carpeta, tabla o campo de una base de datos|
|[strBoxAppend](#strBoxAppend)|A�ade $sAppend a $sString desde el final y hasta el largo de $sString.Si $sPrepend es mas corta que $sString se conservar�n los caracteres de esta ?...|
|[strBoxPrepend](#strBoxPrepend)|A�ade $sPrepend a $sString desde el inicio y hasta el largo de $sString.Si $sPrepend es mas corta que $sString se conservar�n los caracteres de esta...|
|[strCommon](#strCommon)|Compara dos cadenas desde el inicio y retorna la subcadena en com�n|
|[strOperator](#strOperator)|Retorna un operador v�lido en funci�n su codificaci�n:eq:= (Equal)noteq:!= (Not equal)lt: (Greater than)lteq:= (Greater than or equal to)like:LIKEr...|
|[strSizeDecode](#strSizeDecode)|Retorna el valor $sSize en bytes. Cuando existan decimales se redondear� el resultado|
|[strSizeEncode](#strSizeEncode)|Retorna el valor $nBytes con el formato KB o MB o GB etc|
|[tokenDecode](#tokenDecode)|Decodifica una cadena codificada con tokenEncode|
|[tokenEncode](#tokenEncode)|Codifica el valor de $sSource en un token de 2540 caracteres y aplicando el c�digo de seguridad $sKey|
|[treeWalk](#treeWalk)|Aplica una funci�n de usuario recursivamente a cada miembro del arbol,entrando en cada uno de los nodos $sChildrenNode. En cada interacci�n se ejecu...|
|[truelize](#truelize)|Crea un nuevo Array combinando los valores de $aSource como claves y el booleano TRUE como valor de cada uno.|
|[unaccented](#unaccented)|Reemplaza los caracteres acentuados por su equivalente sin acento|
|[unique](#unique)|Genera una cadena aleatoria de 4 a 4096 caracteres que matchea con el patr�n: [a-zA-Z][a-zA-Z0-9]{4,4096}|
|[uriDecode](#uriDecode)|Decodifica una cadena codificada con uriEncode.El valor retornado podr� ser un string o un array, dependiendo del valor original de $sString|
|[uriEncode](#uriEncode)|Codifica una cadena o array para que pueda ser enviado de manera segura por GET o POST|
|[urlExists](#urlExists)|Comprueba si existe una URL. El chequeo se intenta hacer mediante get_headers o curl_init, si no pueden llevarse a cabo retorna NULL|

  
&nbsp;


## apacheMimeTypes
Retorna los Internet media types indexados por extención proporcionados por el sitio 
http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types
Los datos pueden ser leídos la base interna o directamente desde le sitio oficial.
Si la lectura local falla, el método intentará optenerlos desde el sitio oficial y guardarlos localmente.  

**[array]** =  *public* function ( *boolean* \$bOnlineData );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$bOnlineData**|boolean|false|True para leer los datos desde le sitio oficial|

&nbsp;
___
&nbsp;

## arrange
Restituye el orden original de la Cadena o Array **\$mSource** desordenado por **nglCommon::disarrange** según las posiciones de **\$aArrange**.
Es claro que si una cadena o array es desordena usando **nglCommon::arrange** el mismo podrá ser ordenado por **nglCommon::disarrange**.
Este método retornará el mismo tipo de dato que el valor de entrada **\$mSource**.  

**[array]** =  *public* function ( *mixed* \$mDisarrange, *array* \$aArrange );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mDisarrange**|mixed||String o Array a ordenar|
|**\$aArrange**|array||Secuencia númerica que se utilizará para ordenar **\$mSource**.|
### Ejemplos  
#### ordenamiento  
```php
#array original
$input = array("segundo","cuarto","primero","tercero");

# ordenamiento
$orderly = $ngl()->arrange($input, array(2,9,7,3));

#array de salida
Array (
    [0] => primero
    [1] => segundo
    [2] => tercero
    [3] => cuarto
)
```

&nbsp;
___
&nbsp;

## arrayAppend
Añade los indices de 1 o mas arrays al array principal, sin importar el tipo de dato y sin sobreescribir indices.
Si los indices son del tipo alfanumericos agrupara los nuevos valores.  

**[array]** =  *public* function ( *array* \$array1, *array* \$... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$array1**|array||Array inicial|
|**\$...**|array||Resto de arrays|

&nbsp;
___
&nbsp;

## arrayColumn
Devuelve los valores de una sola columna de \$aSource, identificado por la clave de columna \$mColumnKey
Opcionalmente, se puede proporcionar una clave de índice, \$mIndexKey, para indexar los valores del array devuelto  

**[array]** =  *public* function ( *array* \$aSource, *mixed* \$mColumnKey );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aSource**|array||array de datos|
|**\$mColumnKey**|mixed|||

&nbsp;
___
&nbsp;

## arrayGoto
Avanza el puntero del array hasta el índice indicado por \$mKey y retorna los datos  

**[array]** =  *public* function ( *array* \$aSource, *mixed* \$mKey );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aSource**|array||array de datos|
|**\$mKey**|mixed|0|Indice hasta donde se avanzará el puntero|

&nbsp;
___
&nbsp;

## arrayGroup
Agrupa un array bidimensional.
Cuando la variable \$mStructure sea NULL, los valores únicos de cada columna se agruparán en subarrays.
Cuando la variable \$mStructure sea distinto de NULL, los valores se agruparán según los grupos definidos en ella.  

**[array]** =  *public* function ( *array* \$aSource, *mixed* \$aStructure );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aSource**|array||array de datos|
|**\$aStructure**|mixed||Campo principal de agrupamiento o Array con la estructura de sub-agrupamientos.
En la estructura se determinan cuales serán los diferentes grupos y los indices estarán presentes en cada uno.

Estrucutra:
array(
"MAIN" => array(
"campo_principal_de_agrupamiento",
array("campo1","campo2","campo3")
),
"subgrupo1" => array(
"campo_de_agrupamiento",
array("campo4", "campo5", "campo6")
),
"subgrupo2" => array(
"campo_de_agrupamiento", 
array("campo9", "campo10", array(
"subgrupo2.1" => array(
"campo_de_agrupamiento",
array("campo11","campo11")
)
))
)
);

La directiva MAIN debe estar expresada en mayusculas.
Si es necesario determinar una estructura de sub-grupos, pero no redefinir el grupo principal, MAIN deberá ser un array
que sólo contenga el campo_principal_de_agrupamiento|
### Ejemplos  
#### agrupamiento simple  
```php
# origen de datos
$aSource = array(
    array("id"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>1,"quantity"=>15,"price"=>20),
    array("id"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>2,"quantity"=>10,"price"=>16),
    array("id"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>3,"quantity"=>20,"price"=>20),
    array("id"=>2,"date"=>"2015-11-24","name"=>"Ravelli S.A.","cuit"=>"33-58796321-8","product"=>2,"quantity"=>13,"price"=>16),
    array("id"=>2,"date"=>"2015-11-24","name"=>"Ravelli S.A.","cuit"=>"33-58796321-8","product"=>3,"quantity"=>8,"price"=>20)
);

# ejecución
$ngl()->arrayGroup($aSource);

# resultado del agrupamiento
array(
    "1" => array(
        "id" => 1,
        "date" => "2015-11-23",
        "name" => "Castro Hnos SRL",
        "cuit" => "30-36251478-9",
        "product" => array(1, 2, 3), # <-- agrupado
        "quantity" => array(15, 10, 20), # <-- agrupado
        "price" => array(20, 16) # <-- agrupado
    ),
    "2" => array (
        "id" => 2
        "date" => "2015-11-24",
        "name" => "Ravelli S.A.",
        "cuit" => "33-58796321-8",
        "product" => array(2, 3), # <-- agrupado
        "quantity" => array(13, 8), # <-- agrupado
        "price" => array(16, 20) # <-- agrupado
    )
)
```
#### sub-agrupamientos  
```php
# origen de datos
$aSource = array(
    array("id"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>1,"quantity"=>15,"price"=>20),
    array("id"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>2,"quantity"=>10,"price"=>16),
    array("id"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>3,"quantity"=>20,"price"=>20),
    array("id"=>2,"date"=>"2015-11-24","name"=>"Ravelli S.A.","cuit"=>"33-58796321-8","product"=>2,"quantity"=>13,"price"=>16),
    array("id"=>2,"date"=>"2015-11-24","name"=>"Ravelli S.A.","cuit"=>"33-58796321-8","product"=>3,"quantity"=>8,"price"=>20)
);

# ejecución
$aStructure = array(
    "MAIN" => array("id"),
    "details" => array("product", array("product", "quantity", "price"))
);
$ngl()->arrayGroup($aSource, $aStructure);

# resultado del agrupamiento
array(
    "1" => array(
        "id" => 1,
        "date" => "2015-11-23",
        "name" => "Castro Hnos SRL",
        "cuit" => "30-36251478-9",
        "product" => 1,
        "quantity" => 15,
        "price" => 20,
        "details" => array(
            "1" => array("product" => 1, "quantity" => 15, "price" => 20),
            "2" => array("product" => 2, "quantity" => 10, "price" => 16),
            "3" => array("product" => 3, "quantity" => 20, "price" => 20)
        )
    ),
    "2" => array (
        "id" => 2,
        "date" => "2015-11-24",
        "name" => "Ravelli S.A.",
        "cuit" => "33-58796321-8",
        "product" => 2,
        "quantity" => 13,
        "price" => 16,
        "details" => array(
            "2" => array("product" => 2, "quantity" => 13, "price" => 16),
            "3" => array("product" => 3, "quantity" => 8, "price" => 20)
        )
    )
)
```
#### grupo principal  
```php
# origen de datos
$aSource = array(
    array("id"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>1,"quantity"=>15,"price"=>20),
    array("id"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>2,"quantity"=>10,"price"=>16),
    array("id"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>3,"quantity"=>20,"price"=>20),
    array("id"=>2,"date"=>"2015-11-24","name"=>"Ravelli S.A.","cuit"=>"33-58796321-8","product"=>2,"quantity"=>13,"price"=>16),
    array("id"=>2,"date"=>"2015-11-24","name"=>"Ravelli S.A.","cuit"=>"33-58796321-8","product"=>3,"quantity"=>8,"price"=>20)
);

# ejecución
$aStructure = array(
    "MAIN" => array("id", array("name", "cuit", "date")),
    "customers" => array("id", array("name", "cuit")),
    "details" => array("product", array("product", "quantity", "price"))
);
$ngl()->arrayGroup($aSource, $aStructure);

# resultado del agrupamiento
array(
    "1" => array(
        "name" => "Castro Hnos SRL",
        "cuit" => "30-36251478-9",
        "date" => "2015-11-23",
        "details" => array(
            "1" => array("product" => 1, "quantity" => 15, "price" => 20),
            "2" => array("product" => 2, "quantity" => 10, "price" => 16),
            "3" => array("product" => 3, "quantity" => 20, "price" => 20)
        )
    ),
    "2" => array (
        "name" => "Ravelli S.A.",
        "cuit" => "33-58796321-8",
        "date" => "2015-11-24",
        "details" => array(
            "2" => array("product" => 2, "quantity" => 13, "price" => 16),
            "3" => array("product" => 3, "quantity" => 8, "price" => 20)
        )
    )
)
```

&nbsp;
___
&nbsp;

## ArrayGrouper
Método axuliar de nglCommon::arrayGroup  

**[boolean]** =  *private* function ( *string* \$aGrouped, *array* \$mValue, *boolean* \$aStructure );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aGrouped**|string||Patron de búsqueda|
|**\$mValue**|array||Origen de datos|
|**\$aStructure**|boolean|false|Habilita la búsqueda por expresiones regulares, donde \$sNeedle es tratado como un patron regex|

&nbsp;
___
&nbsp;

## arrayIn
Comprueba si un valor se encuentra en un array utilizando in_array o 
chequeando valor por valor utilizando expresiones regulares. en este último caso 
los patrones de búsqueda serán tratados con preg_quote()  

**[boolean]** =  *public* function ( *string* \$sNeedle, *array* \$aHayStack, *boolean* \$bRegex, *string* \$sRegexFlags, *boolean* \$bInverseMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sNeedle**|string||Patron de búsqueda|
|**\$aHayStack**|array||Origen de datos|
|**\$bRegex**|boolean|false|Habilita la búsqueda por expresiones regulares, donde \$sNeedle es tratado como un patron regex|
|**\$sRegexFlags**|string|s|Flags utilizados en el patron de expresiones regulares|
|**\$bInverseMode**|boolean|false|Activa el modo inverso, donde cada valor del array es tratado como un patron regex y comparadon contra \$sNeedle|

&nbsp;
___
&nbsp;

## arrayInsert
Añade un elemento al Array en la posición determinada  

**[array]** =  *public* function ( *array* \$aArray, *mixed* \$mPosition, *mixed* \$aInsert, *boolean* \$bAfter );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aArray**|array||Origen de datos|
|**\$mPosition**|mixed||Posición alfanúmerica de referencia en la que se insertará el nuevo valor|
|**\$aInsert**|mixed||Valor a insertar|
|**\$bAfter**|boolean|true|Determina si el nuevo valor se insertará antes o después del valor de referencia.|

&nbsp;
___
&nbsp;

## arrayMerge
Agrega N arrays multi-dimensionales en uno  

**[array]** =  *public* function ( *array* \$array1, *array* \$... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$array1**|array||Array inicial|
|**\$...**|array||Resto de arrays|

&nbsp;
___
&nbsp;

## arrayMultiSort
Ordena un array multi-dimensional considerando multiples indices, orden y tipos de orden  

**[array]** =  *public* function ( *array* \$aData, *array* \$aFlags );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData**|array||origen de datos|
|**\$aFlags**|array||Array de arrays con las configuraciones de los ordenes.
formato: **array( array( field, [order], [type] ), ..., array( field, [order], [type] ) );**

donde:<ul><li>**field** =  es el indice por el cual se ordenará</li><li>**order** =  dirección del ordenamiento:<ul><li>asc: orden ascendente (valor predeterminado)</li><li>desc: orden descendente</li></ul><li>**type** =  tipo de ordenamiento:<ul><li>0: orden natural sencible a mayúsculas (valor predeterminado)</li><li>1: orden natural insencible a mayúsculas</li><li>2: numerico</li><li>3: orden por cadena sencible a mayúsculas</li><li>4: orden por cadena insencible a mayúsculas</li></ul></li></ul>|
### Ejemplos  
#### $aFlags de un ordenamiento por 2 columnas  
```php
$aFruits = array(
    array("name"=>"lemon", "color"=>"yellow"),
    array("name"=>"orange", "color"=>"orange"),
    array("name"=>"apple", "color"=>"red")
);

arrayMultiSort($aFruits, array(
    array("field"=>"name", "order"=>"desc", "type"=>2),
    array("field"=>"color", "type"=>3)
));
```

&nbsp;
___
&nbsp;

## arrayRebuilder
Agrupa los índices **\$aIndexes** combinandolos por sus claves. Con la opción de reenombrar estas últimas.
Si \$aIndexes = null y \$mNewIndexes es una cadena, el método retornará un array bidimensional donde cada subarray tendrá como 
único indice a \$mNewIndexes y cuyo valor será el valor del indice actual de \$aSource.  

**[array]** =  *public* function ( *array* \$aSource, *mixed* \$mIndexes, *mixed* \$mNewIndexes );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aSource**|array||Array de datos|
|**\$mIndexes**|mixed||Lista de indices a combinar o NULL|
|**\$mNewIndexes**|mixed|$aIndexes|Lista con los nombres de los nuevos indices, que se reemplazarán uno a uno con **\$aIndexes** o una cadena|
### Ejemplos  
#### Ejemplo  
```php
#array original
$input = array(
    "entity" => "FooBar Inc.",
    "start_date" => "2015-11-23",
    "contact_surname" => array(
        "Smith",
        "Stewart",
        "Astley"
    ),
    "contact_firstname" => array(
        "John",
        "Sara",
        "Ralph"
    ),
);

# llamada
$output = $ngl()->arrayRebuilder($input, array("contact_surname", "contact_firstname"));

#array de salida
Array (
    [0] => Array(
        [contact_surname] = Smith
        [contact_firstname] = John
    ),
    [1] => Array(
        [contact_surname] = Smith
        [contact_firstname] = Astley
    ),
    [2] => Array(
        [contact_surname] = Smith
        [contact_firstname] = Ralph
    )
)
```
#### Ejemplo anterior renombrando claves  
```php
# llamada
$output = $ngl()->arrayRebuilder($input, array("contact_surname", "contact_firstname"), array("surname", "firstname"));

#array de salida
Array (
    [0] => Array(
        [surname] = Smith
        [firstname] = John
    ),
    [1] => Array(
        [surname] = Smith
        [firstname] = Astley
    ),
    [2] => Array(
        [surname] = Smith
        [firstname] = Ralph
    )
)
```
#### Ejemplo con $mNewIndexes como cadena  
```php
#array original
$input = array(
    "0" => array("firstname"=>"John", "age"=>23),
    "1" => array("firstname"=>"Sara", "age"=>24),
    "2" => array("firstname"=>"Ralph", "age"=>25)
);

# llamada
$output = $ngl()->arrayRebuilder($input, null, array("contact"));

#array de salida
Array (
    [0] => Array(
        [contact] => Array(
            [firstname] => John
            [age] => 23
        )
    ),
    [1] => Array(
        [contact] => Array(
            [firstname] => Sara
            [age] => 24
        )
    ),
    [2] => Array(
        [contact] => Array(
            [firstname] => Ralph
            [age] => 25
        )
    )
)
```

&nbsp;
___
&nbsp;

## arrayRepeat
Retorna un array con indices númericos que contiene **\$nMultiplier** repeticiones del array **\$aInput**.  

**[array]** =  *public* function ( *array* \$aInput, *int* \$nMultiplier );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aInput**|array||Array a ser repetido|
|**\$nMultiplier**|int||Número de veces que **\$aInput** debe ser repetido.|
### Ejemplos  
#### array númerico  
```php
#array original
$input = array("A", "B", "C");

# repetición
$output = $ngl()->arrayRepeat($input, 3);

#array de salida
Array (
    [0] => A
    [1] => B
    [2] => C
    [3] => A
    [4] => B
    [5] => C
    [6] => A
    [7] => B
    [8] => C
)
```
#### array asociativo  
```php
#array original
$input = array("A"=>"ANANA", "B"=>"BANANA", "C"=>"CIRUELA");

# repetición
$output = $ngl()->arrayRepeat($input, 2);

#array de salida
Array (
    [0] => ANANA
    [1] => BANANA
    [2] => CIRUELA
    [3] => ANANA
    [4] => BANANA
    [5] => CIRUELA
)
```

&nbsp;
___
&nbsp;

## base64Cleaner
Elimina de una cadena todos los caracteres que no sean válidos en una cadena base64 [a-zA-Z0-9+/=]  

**[string]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena a limpiar|

&nbsp;
___
&nbsp;

## between
Verifica si un valor en relación a un rango de valores.
between tambien puede ser utilizado para conocer si un valor es mayor o menor a otro, ya que si no se especifica un valor para \$sMaxValue distinto de null, se asume que \$sMaxValue es igual a \$sMinValue.

Los posibles valores retornados por between son:<ul><li>**0** =  cuando \$mValue es menor a \$sMinValue</li><li>**1** =  cuando \$mValue esta dentro del rango de valores</li><li>**2** =  cuando \$mValue es mayor a \$sMaxValue</li></ul>  

**[integer]** =  *public* function ( *string* \$mValue, *string* \$sMinValue, *string* \$sMaxValue, *boolean* \$bCaseInsensitive );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mValue**|string||Valor a chequear|
|**\$sMinValue**|string||Mínimo valor del rango|
|**\$sMaxValue**|string|null|Máximo valor del rango|
|**\$bCaseInsensitive**|boolean|false|Modo insensible a mayúsculas|

&nbsp;
___
&nbsp;

## clearPath
Elimina los slashes de mas en un path o url. Todos los **\$sSeparator** de cierre serán eliminados  

**[string]** =  *public* function ( *string* \$sPath, *boolean* \$bSlashClose, *string* \$sSeparator, *boolean* \$bRealPath );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string||Path a limpiar|
|**\$bSlashClose**|boolean|false|Cuando el valor es true añade un slash al final del path|
|**\$sSeparator**|string|NGL_DIR_SLASH|Slash utilizado|
|**\$bRealPath**|boolean|false|Cuando es TRUE aplica realpath() a la path|

&nbsp;
___
&nbsp;

## colorHex
Retorna un color en valores hexadecimales basandose en RGB  

**[string]** =  *public* function ( *int* \$nRed, *int* \$nGreen, *int* \$nBlue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nRed**|int|00|Valor de 0 a 255 para el color rojo|
|**\$nGreen**|int|00|Valor de 0 a 255 para el color verde|
|**\$nBlue**|int|00|Valor de 0 a 255 para el color azul|

&nbsp;
___
&nbsp;

## colorRGB
Retorna los valores RGB y Transparencia de un color en formato hexadecimal  

**[array]** =  *public* function ( *string* \$sHexacolor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sHexacolor**|string|#00000000|Valor del color en formato #RRGGBBAA (rojo, verde, azul, alfa)|

&nbsp;
___
&nbsp;

## coockie
Guarda y optiene el valor de una cookie del navegador.
Los valores son analizados con ngl::passwd(), por lo que si NGL_PASSWORD_KEY esta activa, los valores serán enviados al navegador de manera encriptada.  

**[mixed]** =  *public* function ( *string* \$sKey, *string* \$sValue, *mixed* \$mExpire );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sKey**|string||Nombre de la coockie|
|**\$sValue**|string|NULL|Valor de la cookie. Si el valor es NULL o es ignorado, el método intentará retornar el valor actual de la cookie|
|**\$mExpire**|mixed|NULL|Indice de tiempo en el que expira la cookie:<ul><li>**null** =  establece el valor de expiració en 5 años</li><li>**string** =  el valor será tratado con strtotime</li><li>**int** =  valor en segundos</li></ul>|

&nbsp;
___
&nbsp;

## dec2hex
Transforma un decimal en hexadecimal sin límite de tamaño y con la posibilidad de rellenar con 0 por delante  

**[string]** =  *public* function ( *int* \$sDecimal, *int* \$nLength );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sDecimal**|int||Número decimal|
|**\$nLength**|int|0|Largo total de la cadena. Si es inferior o igual a la longitud del string de entrada, no se realiza el rellenado|

&nbsp;
___
&nbsp;

## disarrange
Desordena de manera cíclica la Cadena o Array **\$mSource** según las posiciones de **\$aArrange**.
En la medida en que el desordenamiento avanza sobre **\$aArrange**, las posiciones obtenidas son eliminadas de **\$mSource** haciendo que este sea cada vez mas pequeño.
Este método retornará el mismo tipo de dato que el valor de entrada **\$mSource**.  

**[array]** =  *public* function ( *mixed* \$mSource, *array* \$aArrange );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mSource**|mixed||String o Array a desordenar|
|**\$aArrange**|array||Secuencia númerica que se utilizará para desordenar **\$mSource**.|
### Ejemplos  
#### desordenamiento  
```php
#array original
$input = array("primero","segundo","tercero","cuarto");

#ordenamiento
$disorderly = $ngl()->disarrange($input, array(2,9,7,3));

#array de salida
Array (
    [0] => segundo
    [1] => cuarto
    [2] => primero
    [3] => tercero
)

#explicación
Array( primero, segundo, tercero, cuarto )
    cuenta "2" posiciones y retorna "segundo"

Array( primero, tercero, cuarto )
    cuenta "9" posiciones y retorna "cuarto"

Array( primero, tercero )
    cuenta "7" posiciones y retorna "primero"

por último retorna "tercero"
```

&nbsp;
___
&nbsp;

## dump
Retorna el contenido de una variable de acuerdo al tipo de la misma:<br /><ul><li>arrays -> print_r</li><li>cadenas -> echo</li><li>otros -> var_dump</li></ul>Los valores son capturados por métodos de control de salida y retornados, no se imprimen
directamente en la pantalla.  

**[string]** =  *public* function ( *mixed* \$mVariable1, *mixed* \$..., *mixed* \$mVariableN );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mVariable1**|mixed||Variable a volcar|
|**\$...**|mixed||Variable a volcar|
|**\$mVariableN**|mixed||Variable a volcar|

&nbsp;
___
&nbsp;

## emptyToNull
Establece como NULL los valores de \$aData, cuyo indice se encuentre en \$aKeys, que retornen TRUE a la funcion empty.
Si \$aKeys es NULL se evaluarán todos los indices.  

**[array]** =  *public* function ( *array* \$aData, *array* \$aKeys );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData**|array||Array de datos|
|**\$aKeys**|array|null|Array con los nombres de las claves del array \$aData que deberán ser evaluadas|

&nbsp;
___
&nbsp;

## encoding
Verifica si la cadena **\$sString** se encuentra codificada en **\$mEncoding**
**\$mEncoding** debe ser el nombre de una codificación válida o un array de nombres.
Si no se especifica un **\$mEncoding** se chequerá la cadena con las codificaciones mas frecuentes.

Una lista completa de las codificaciones soportadas se encuentra en:
https://www.gnu.org/software/libiconv  

**[string o booleano]** =  *public* function ( *string* \$sString, *mixed* \$mEncoding, *boolean* \$bStrict );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena a chequear|
|**\$mEncoding**|mixed|null|Nombre de una codificación válida o un array de nombres|
|**\$bStrict**|boolean|false|Determina si, en caso afirmativo, el método debe retornar el nombre de la codificación o TRUE|

&nbsp;
___
&nbsp;

## ensureVar
Retorna el valor de \$mSure cuanto \$mVar no esta seteada o es NULL  

**[mixed]** =  *public* function ( *mixed* \$mVar, *mixed* \$mSure );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mVar**|mixed||Variable a evaluar|
|**\$mSure**|mixed||Valor que se aplicará cuando \$mVar no exista o sea NULL|

&nbsp;
___
&nbsp;

## exploder
Ejecuta la función **explode** de PHP de manera recursiva, utilizando los delimitadores para armar un array multi-dimensional  

**[array]** =  *public* function ( *array* \$aDelimiters, *string* \$sSource, *int* \$nLimit );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aDelimiters**|array||Delimitadores|
|**\$sSource**|string||Cadena de origen|
|**\$nLimit**|int||Si es positivo, el array devuelto contendrá el máximo de \$nLimit elementos, y el último elemento contendrá el resto de la cadena de origen.
Si es negativo, se devolverán todos los componentes a excepción del último -\$nLimit.
Si es cero, se tratará como 1.|

&nbsp;
___
&nbsp;

## explodeTrim
Ejecuta la función **explode** de PHP y a continuación trata a cada uno de los valores con la función **trim**  

**[array]** =  *public* function ( *string* \$sDelimiter, *string* \$sSource, *int* \$nLimit );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sDelimiter**|string||Delimitador|
|**\$sSource**|string||Cadena de origen|
|**\$nLimit**|int||Si es positivo, el array devuelto contendrá el máximo de \$nLimit elementos, y el último elemento contendrá el resto de la cadena de origen.
Si es negativo, se devolverán todos los componentes a excepción del último -\$nLimit.
Si es cero, se tratará como 1.|

&nbsp;
___
&nbsp;

## headers
Retorna un array con todas las cabeceras enviadas hasta el momento, un una cadena o false para cuando se especifique \$sHeader  

**[mixed]** =  *public* function ( *string* \$sHeader );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sHeader**|string||Chequea que esta cabecera haya sido enviada, retornando su valor o FALSE|

&nbsp;
___
&nbsp;

## hex2dec
Transforma un hexadecimal en decimal sin límite de tamaño  

**[string]** =  *public* function ( *string* \$sDecimal );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sDecimal**|string||Número hexadecimal|

&nbsp;
___
&nbsp;

## imploder
Une elementos de un array multi dimensional en una cadena.
Cuando **\$mGlue** sea declarado como un array, el primer índice será utilizado para unir los valores 
y el segundo para unir los distintos niveles del array.
Para mantener una relación con **implode**, si **\$mGlue** no es especificado se asumirá que el único valor pasado es **\$aSource**.  

**[string]** =  *public* function ( *mixed* \$mGlue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mGlue**|mixed||Cadena o array de dos de ellas con las que se unirán los valores|

&nbsp;
___
&nbsp;

## imya
Retorna o valida un **imya**  

**[string o null]** =  *public* function ( *string* \$sImya );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sImya**|string|null|Cuando el valor es NULL se genera un nuevo imya, equivalente a un valor unique(32)
Cuando el valor es distinto de NULL limpia la cadena basandose en el patron [^a-zA-Z0-9] y la corta a los 32 caracteres
Si la cadena resultante cuenta con menos de 32 carecteres el método retornará NULL|

&nbsp;
___
&nbsp;

## intPart
Retorna la parte entera de un número  

**[integer o NULL]** =  *public* function ( *mixed* \$mNumber );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mNumber**|mixed||Valor númerico|

&nbsp;
___
&nbsp;

## isArrayArray
Comprueba si **\$aArray** es un Array de Arrays. 
Con **\$bStrict** FALSE sólo chequeará que el primer valor de **\$aArray** sea un array. Si es TRUE verificará que todos los valores sean del tipo array.  

**[boolean]** =  *public* function ( *array* \$aArray, *boolean* \$bStrict );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aArray**|array||Array a comprobar|
|**\$bStrict**|boolean|false|Activa o desactiva el modo estricto|

&nbsp;
___
&nbsp;

## isEmpty
Comprueba si **\$mValue** esta vacío. en el caso de que 
\$mValue sea del tipo Array, isEmpty devolverá FALSE si al menos
uno de sus índices está vacío. Los arrays son examinados de manera recursiva  

**[boolean]** =  *public* function ( *mixed* \$mValue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mValue**|mixed||Valor a comprobar|

&nbsp;
___
&nbsp;

## isInteger
Comprueba si un valor es un número entero  

**[boolean]** =  *public* function ( *mixed* \$mNumber );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mNumber**|mixed||Valor númerico|

&nbsp;
___
&nbsp;

## isJSON
Comprueba si un valor es una cadena JSON válida  

**[mixed]** =  *public* function ( *string* \$sString, *string* \$mType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena a chequear|
|**\$mType**|string|null|Determina el tipo de respuesta (Boolean, Array u Object)<ul><li>**NULL** =  se retornará TRUE o FALSE</li><li>**array** =  se retornarán los datos como un array asociativo cuando el valor sea un JSON, o FALSE</li><li>**object** =  se retornarán los datos como un objeto cuando el valor sea un JSON, o FALSE/li></ul>|

&nbsp;
___
&nbsp;

## isLowerCase
Comprueba si **\$sString** son sólo letras minúsculas. En el caso de que 
\$mValue sea del tipo Array, isLowerCase devolverá FALSE si al menos en 
uno de sus índices existen catacteres que no esten en minúsculas.
Los arrays son examinados de manera recursiva  

**[boolean]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena a comprobar|

&nbsp;
___
&nbsp;

## isNull
Comprueba si un valor es NULL.
Esto sucederá cuando el método nativo **is_null(\$mValue)** retorne true o cuando el valor **strtolower(\$mValue)** sea igual a **null**  

**[boolean]** =  *public* function ( *mixed* \$mValue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mValue**|mixed||Cadena a comprobar|

&nbsp;
___
&nbsp;

## isNumber
Comprueba si **\$mNumber** es un valor númerico y retorna su valor en el formato correcto (float o int).
Seran considerados números los siguientes formatos:<br /><ul><li>123.456 (float)</li><li>123,456	(float)</li><li>123,456.78 (float)</li><li>123.456,78 (float)</li><li>123.456.789 (int)</li><li>123,456,789 (int)</li><li>123456789 (int)</li></ul>  

**[integer, float o null]** =  *public* function ( *mixed* \$mNumber );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mNumber**|mixed||Valor a comprobar|

&nbsp;
___
&nbsp;

## isSerialized
Comprueba si **\$sString** es un array serializado. Si **\$bResult** es igual a TRUE el método retornará un array en caso de TRUE  

**[mixed]** =  *public* function ( *string* \$sString, *boolean* \$bResult );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena a chequear|
|**\$bResult**|boolean|false|Determina el tipo de respuesta|

&nbsp;
___
&nbsp;

## isTrue
Comprueba si **\$mValue** es TRUE o FALSE. Si \$mValue es String y su valor es '0', 'false', 'null', 'no' u 'off', el valor de retorno será FALSE  

**[boolean]** =  *public* function ( *mixed* \$mValue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mValue**|mixed||Valor a comprobar|

&nbsp;
___
&nbsp;

## isUpperCase
Comprueba si **\$sString** son sólo letras mayúsculas. En el caso de que \$mValue sea del tipo Array, isUpperCase devolverá FALSE si al menos en uno de sus índices existen catacteres que no esten en mayúsculas.
Los arrays son examinados de manera recursiva  

**[boolean]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena a comprobar|

&nbsp;
___
&nbsp;

## isURL
Retorna TRUE (o el protocolo) si **\$sFilePath** es una URL http, ftp o comienza con //
Para este último caso, cuando se solicite el protocolo, se retornará "url"  

**[mixed]** =  *public* function ( *string* \$sFilePath, *boolean* \$bScheme );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFilePath**|string||URL a comprobar|
|**\$bScheme**|boolean|false|Determina si en caso de TRUE se debe o no retornar el protocolo|

&nbsp;
___
&nbsp;

## isUTF8
Comprueba si **\$sString** es una cadena UTF-8  

**[boolean]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena a comprobar|

&nbsp;
___
&nbsp;

## memory
Devuelve el valor de la cantidad de memoria asignada a PHP, formateado con strSizeEncode()  

**[string]** =  *public* function ( *boolean* \$bRealUsage, *int* \$nDecimals );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$bRealUsage**|boolean|false|True para obtener el tamaño real de memoria asignada por el sistema.|
|**\$nDecimals**|int|5|Cantidad de decimales despues de la coma|

&nbsp;
___
&nbsp;

## mimeType
Retorna el Mime Type de la extensión proporcionada.  

**[string]** =  *public* function ( *string* \$sExtension );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sExtension**|string||Extensión.|

&nbsp;
___
&nbsp;

## secureName
Limpia una cadena para que pueda ser utilizada como nombre de archivo, carpeta, tabla o campo de una base de datos  

**[string]** =  *public* function ( *string* \$sName, *string* \$sLeave );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sName**|string||Nombre original.|
|**\$sLeave**|string||Conserva estos caracteres|

&nbsp;
___
&nbsp;

## once
Genera o chequea un código único guardado en la session activa.
Cuando se ejecuta el método sin el argumento \$sCode, este generará un **ONCECODE**, lo guardará en la session y lo retornará.
Cuando se pase un \$sCode al método, este chequeará si el mismo existe en la session activa. Si existe lo deseteará y devolverá TRUE; en caso contrario retornará FALSE.
La vigencia de los códigos en la session es de NGL_ONCECODE_TIMELIFE, en segundos.  

**[mixed]** =  *public* function ( *string* \$sCode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCode**|string|null|ONCECODE generado por el método en una ejecución previa|

&nbsp;
___
&nbsp;

## round05
Redondea un número al entero o punto medio mas cercano.
El parámetro \$nPrecition permite controlar la distancia del redondeo al punto medio
Según la presición el redondeo dará con .5 cuando:<ul><li>**0** =  cuando sea x.5</li><li>**1** =  cuando se encuentre entre x.4 y x.6</li><li>**2** =  cuando se encuentre entre x.3 y x.7</li><li>**3** =  cuando se encuentre entre x.2 y x.8</li><li>**4** =  cuando se encuentre entre x.1 y x.9</li><li>**&gt5** =  siempre</li></ul>  

**[float]** =  *public* function ( *int* \$nNumber );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nNumber**|int||Numero a redondear|
### Ejemplos  
#### Redondeos  
```php
round05(3.2, 1) => 3
round05(3.2, 2) => 3
round05(3.2, 3) => 3.5
round05(3.2, 4) => 3.5
round05(3.2, 5) => 3.5
```

&nbsp;
___
&nbsp;

## strBoxAppend
Añade \$sAppend a \$sString desde el final y hasta el largo de \$sString.
Si \$sPrepend es mas corta que \$sString se conservarán los caracteres de esta última que no lleguen a ser desplazados  

**[string]** =  *public* function ( *string* \$sString, *string* \$sAppend );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena contenedora|
|**\$sAppend**|string||Cadena de reemplazo|
### Ejemplos  
#### $sString > $sAppend  
```php
$sString = "lorem";
$sAppend = "sit";
# emsit
```
#### $sString = $sAppend  
```php
$sString = "lorem";
$sAppend = "ipsum";
# ipsum
```
#### $sString < $sAppend  
```php
$sString = "lorem";
$sAppend = "consectetuer";
# etuer
```

&nbsp;
___
&nbsp;

## strBoxPrepend
Añade \$sPrepend a \$sString desde el inicio y hasta el largo de \$sString.
Si \$sPrepend es mas corta que \$sString se conservarán los caracteres de esta última que no lleguen a ser desplazados  

**[string]** =  *public* function ( *string* \$sString, *string* \$sPrepend );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena contenedora|
|**\$sPrepend**|string||Cadena de reemplazo|
### Ejemplos  
#### $sString > $sPrepend  
```php
$sString = "lorem";
$sPrepend = "sit";
sitlo
```
#### $sString = $sPrepend  
```php
$sString = "lorem";
$sPrepend = "ipsum";
ipsum
```
#### $sString < $sPrepend  
```php
$sString = "lorem";
$sPrepend = "consectetuer";
conse
```

&nbsp;
___
&nbsp;

## strCommon
Compara dos cadenas desde el inicio y retorna la subcadena en común  

**[string]** =  *public* function ( *string* \$sString1, *string* \$sString2 );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString1**|string||Primer cadena para la comparación|
|**\$sString2**|string||Segunda cadena para la comparación|

&nbsp;
___
&nbsp;

## strOperator
Retorna un operador válido en función su codificación:<ul><li>**eq** = <em>=</em> (Equal)</li><li>**noteq** = <em>!=</em> (Not equal)</li><li>**lt** = <em><</em> (Less than)</li><li>**gt** = <em>></em> (Greater than)</li><li>**lteq** = <em><=</em> (Less than or equal to)</li><li>**gteq** = <em>>=</em> (Greater than or equal to)</li><li>**like** = <em>LIKE</em></li><li>**rlike** = <em>RLIKE</em></li><li>**and** = <em>AND</em></li><li>**or** = <em>OR</em></li><li>**xor** = <em>XOR</em> (Exclusive OR)</li><li>**in** = <em>IN</em></li><li>**notin** = <em>NOT IN</em></li><li>**is** = <em>IS</em></li><li>**isnot** = <em>IS NOT</em></li></ul>Si **\$sSign** no se encuentra entre las opciones, se retornará el signo =
Si **\$sSign** no es especificado, se retornará un array asosiativo con todos los operadores  

**[mixed]** =  *public* function ( *string* \$sSign, *boolean* \$bEmpty );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sSign**|string||Código del signo que se quiere obtener|
|**\$bEmpty**|boolean|false|Deterina si debe retornarse vacio en caso de no encontrar coincidencia|

&nbsp;
___
&nbsp;

## strSizeDecode
Retorna el valor \$sSize en bytes. Cuando existan decimales se redondeará el resultado  

**[string]** =  *public* function ( *string* \$sSize );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sSize**|string||Tamaño a convertir|

&nbsp;
___
&nbsp;

## strSizeEncode
Retorna el valor \$nBytes con el formato KB o MB o GB etc  

**[string]** =  *public* function ( *int* \$nBytes, *int* \$nDecimals );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nBytes**|int||Número de bytes|
|**\$nDecimals**|int|2|Cantidad de decimales despues de la coma|

&nbsp;
___
&nbsp;

## tokenDecode
Decodifica una cadena codificada con **tokenEncode**  

**[string]** =  *public* function ( *string* \$sToken, *string* \$sKey );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sToken**|string||Cadena codificada|
|**\$sKey**|string||Código se seguridad|

&nbsp;
___
&nbsp;

## tokenEncode
Codifica el valor de **\$sSource** en un token de 2540 caracteres y aplicando el código de seguridad **\$sKey**  

**[string]** =  *public* function ( *string* \$sSource, *string* \$sKey, *string* \$sTokenTitle );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sSource**|string||Cadena de hasta 16 caracteres que se desea tokenizar|
|**\$sKey**|string||Código se seguridad|
|**\$sTokenTitle**|string|NGL TOKEN|Título del token, este aparecera en la línea de encabezado|

&nbsp;
___
&nbsp;

## treeWalk
Aplica una función de usuario recursivamente a cada miembro del arbol,
entrando en cada uno de los nodos \$sChildrenNode. En cada interacción se ejecutará los métodos:<ul><li>\$fFunction</li><li>\$vEvents[branchOpen]</li><li>\$vEvents[nodeOpen]</li><li>\$vEvents[nodeClose]</li><li>\$vEvents[branchClose]</li></ul>  

**[void]** =  *public* function ( *array* \$aData, *function* \$fFunction, *string* \$sChildrenNode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData**|array||Array de datos|
|**\$fFunction**|function||Función del usuario que se ejecutará para cada nodo. En cada ejecución se pasarán los siguientes argumentos:<br /><ul><li>datos del nodo</li><li>nivel de profundidad</li><li>booleano que define si el nodo actual es el primer nodo de la rama</li><li>booleano que define si el nodo actual es el último nodo de la rama</li></ul>|
|**\$sChildrenNode**|string||Nombre de nodo que contiene a los hijos|
### Ejemplos  
#### Formato de árbol #1  
```php
$aFamily = array(
     array(
         "name" => "Emily Summer",
         "age" => 78,
         "_children" => array(
                 array(
                     "name"=>"Marge Charles",
                     "age" => 50,
                     "_children" => array(
                         array("name"=>"Sara Smith", "age"=>20),
                         array("name"=>"Max Smith", "age"=>17)
                 )
             )
         )
     ),
    
     array(
         "name" => "Rod Smith",
         "age" => 80,
         "_children" => array(
                 array(
                     "name"=>"John Smith",
                     "age" => 54,
                     "_children" => array(
                         array("name"=>"Sara Smith", "age"=>20),
                         array("name"=>"Max Smith", "age"=>17)
                 )
             ),

             array(
                 "name"=>"Susan Smith",
                 "age" => 49,
                 "_children" => array(
                     array("name"=>"Ralph Astley")
                 )
             )
         )
     )
);
```
#### Formato de árbol #2  
```php
$aFamily = array(
     "name" => "Emily Summer",
     "age" => 78,
     "_children" => array(
             array(
                 "name"=>"Marge Charles",
                 "age" => 50,
                 "_children" => array(
                     array("name"=>"Sara Smith", "age"=>20),
                     array("name"=>"Max Smith", "age"=>17)
             )
         )
     )
);
```
#### Ejemplo de función del usuario  
```php
$aLs = $ngl("files")->ls("mydocuments", "*", "info", true);

echo "<pre>";
$sColumn = "basename";
$aList = $ngl()->treeWalk($aLs, function($aNode, $nLevel, $bFirst, $bLast) use ($sColumn) {
        $sOutput  = ($nLevel) ? str_repeat("│   ", $nLevel) : "";
        $sOutput .= ($bLast) ? "└─── " : "├─── ";
        $sOutput .= (($aFile["type"]=="dir") ? $aFile[$sColumn]."/" : $aFile[$sColumn]);
        $sOutput .= "\n";
        return $sOutput;
    }
);
echo implode($aList);
echo "</pre>";

# salida
mydocuments/
├── excel/
├── mp3/
│   ├── rock/
│   └── pop/
└── word/
    └── personal/
```

&nbsp;
___
&nbsp;

## truelize
Crea un nuevo Array combinando los valores de \$aSource como claves y el booleano TRUE como valor de cada uno.  

**[array]** =  *public* function ( *array* \$aSource );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aSource**|array|||
### Ejemplos  
#### ejemplo #1  
```php
#array original
$input = array("A", "B", "C", "D");

# transformación
$output = $ngl()->truelize($input);

#array de salida
Array (
    ["A"] => true
    ["B"] => true
    ["C"] => true
    ["D"] => true
)
```

&nbsp;
___
&nbsp;

## unaccented
Reemplaza los caracteres acentuados por su equivalente sin acento  

**[string]** =  *public* function ( *string* \$sAccented );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sAccented**|string||Cadena acentuada|

&nbsp;
___
&nbsp;

## unique
Genera una cadena aleatoria de 4 a 4096 caracteres que matchea con el patrón: [a-zA-Z][a-zA-Z0-9]{4,4096}  

**[string]** =  *public* function ( *int* \$nLength );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nLength**|int|6|Loguitud de la cadena|

&nbsp;
___
&nbsp;

## uriDecode
Decodifica una cadena codificada con **uriEncode**.
El valor retornado podrá ser un string o un array, dependiendo del valor original de **\$sString**  

**[string o array]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena codificada|

&nbsp;
___
&nbsp;

## uriEncode
Codifica una cadena o array para que pueda ser enviado de manera segura por GET o POST  

**[string]** =  *public* function ( *mixed* \$mValue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mValue**|mixed||Cadena o array que se quiere codificar|

&nbsp;
___
&nbsp;

## urlExists
Comprueba si existe una URL. El chequeo se intenta hacer mediante **get_headers** o **curl_init**, 
si no pueden llevarse a cabo retorna **NULL**  

**[boolean]** =  *public* function ( *string* \$sURL );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sURL**|string||URL a chequear|

&nbsp;
___
&nbsp;

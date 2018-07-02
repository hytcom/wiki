# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# shift
## nglShift *extends* nglTrunk [main] [20150225]
Este objeto nuclea métodos de conversión de datos entre las siguientes escructuras:<ul><li>**array** =  Array de datos</li><li>**csv** =  Texto inline o multilinea de valores separados por comas</li><li>**fixed** =  Texto inline o multilinea de valores separados ancho fijo</li><li>**html** =  Tablas html, soporta tablas anidadas. Si se especifican thead se utilizarán como claves</li><li>**json** =  JSON</li><li>**object** =  Objecto de datos o un objeto del tipo iNglDataObjet</li><li>**serialize** =  valor serializado</li><li>**xml** =  XML</li></ul>
  
## Variables
`private` $vCSV = Configuraciones para la lectura de cadenas CSV  

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[CSVParseLine](#CSVParseLine)|auxiliar del m�todo nglShift::csvToArray. convierte una linea CSV en un array|
|[CastValue](#CastValue)|Auxiliar del m�todo nglShift::cast|
|[HTMLTableParser](#HTMLTableParser)|Auxiliar del m�todo htmlToArray|
|[JSONChar](#JSONChar)|Auxiliar del m�todo nglShift::jsonEncode encargado de codificar un caracter para que sea v�lido dentro de una cadena UTF-8|
|[JSONNameValuePair](#JSONNameValuePair)|Auxiliar del m�todo nglShift::jsonEncode encargado generar un par ordenado NOMBRE:VALOR v�lido|
|[JSONReduceString](#JSONReduceString)|Auxiliar del m�todo nglShift::jsonDecode encargado limpiar el c�digo antes de ser parseado|
|[XMLChildren](#XMLChildren)|Auxiliar del m�todo ngl:Babel::xmlToArray utilizado para recorrer el objeto XML de manera recursiva|
|[cast](#cast)|Formatea el valor $mValue seg�n $sCastType, siempre que se encuentre dentro de los tipos:arraybooleandoubleintegerNULLobjectstring|
|[convert](#convert)|Convierte una estructura de datos en otra|
|[csvEncode](#csvEncode)|Genera una cadena formateada como CSV partiendo de un Array|
|[csvToArray](#csvToArray)|convierte un texto CSV (una l�nea o conjunto de ellas) en un array bidimensional|
|[fixedExplode](#fixedExplode)|Convierte una cadena en Array separando sus partes por caracter fijo|
|[fixedImplode](#fixedImplode)|Convierte un Array en una cadena respetando las logitudes de positions. Si la longuitud de la cadena es superior al valor de positions, el valor ser�...|
|[html](#html)|Genera una salida HTML a partir de un Array|
|[htmlToArray](#htmlToArray)|Convierte una Tabla HTML en un array, utilizando el objeto DOMDocument.Las tablas pueden o no tener THEAD y TBODY. En el caso de tener THEAD los valor...|
|[jsonDecode](#jsonDecode)|Decodifica una cadena JSON de un Array|
|[jsonEncode](#jsonEncode)|Codifica un valor en una cadena JSON|
|[jsonFormat](#jsonFormat)|Auxiliar del m�todo nglShift::jsonEncode encargado generar un par ordenado NOMBRE:VALOR v�lido|
|[objFromArray](#objFromArray)|Convierte un Array en un Objeto de manera recursiva|
|[objToArray](#objToArray)|Convierte un objeto en un array asosiativo de manera recursiva|
|[xmlEncode](#xmlEncode)|Convierte un array en una estructura XML|
|[xmlToArray](#xmlToArray)|Vuelca el contenido de un texto XML en un array asosiativo de manera recursiva|

  
&nbsp;


## cast
Formatea el valor **\$mValue** según **\$sCastType**, siempre que se encuentre dentro de los tipos:<ul><li>array</li><li>boolean</li><li>double</li><li>integer</li><li>NULL</li><li>object</li><li>string</li></ul>  

**[mixed]** =  *public* function ( *mixed* \$mValue, *string* \$sCastType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mValue**|mixed||Variable a formatear|
|**\$sCastType**|string|text|tipo de formato:<br /><ul><li>**text** =  texto plano (valor predeterminado)</li><li>**html** =  se aplica htmlspecialchars</li><li>**htmlall** =  se aplica htmlentities</li></ul>|
### Ejemplos  
#### example  
```php
$text = <<<TXT <b>El 'río' de las "pirañas"</b> TXT;

echo $ngl("shift")->cast($text);
# Retorna: <b>El 'río' de las "pirañas"</b>

echo $ngl("shift")->cast($text, "html");
# Retorna: &lt;b&gt;El &#039;río&#039; de las &quot;pirañas&quot;&lt;/b&gt;

echo $ngl("shift")->cast($text, "htmlall");
# Retorna: &lt;b&gt;El &#039;r&iacute;o&#039; de las &quot;pira&ntilde;as&quot;&lt;/b&gt;
```

&nbsp;
___
&nbsp;

## CastValue
Auxiliar del método **nglShift::cast**  

**[mixed]** =  *private* function ( *mixed* \$mValue, *string* \$sCastType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mValue**|mixed||Variable a formatear|
|**\$sCastType**|string||tipo de formato:<br /><ul><li>**text** =  texto plano (valor predeterminado)</li><li>**html** =  se aplica htmlspecialchars</li><li>**htmlall** =  se aplica htmlentities</li></ul>|

&nbsp;
___
&nbsp;

## convert
Convierte una estructura de datos en otra  

**[mixed]** =  *public* function ( *mixed* \$mData, *string* \$sMethod, *array* \$vOptions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mData**|mixed||Estructura de datos original|
|**\$sMethod**|string|object-array|Método de conversión, que se indica separando los tipos de estruturas de origen y destino con un - (guión medio)

Estructuras soportadas:<ul><li>array</li><li>csv</li><li>fixed</li><li>html</li><li>json</li><li>object</li><li>serialize</li><li>xml</li></ul>Ejemplos:<ul><li>array-csv</li><li>json-array</li><li>fixed-csv</li><li>html-json</li></ul>|
|**\$vOptions**|array||Array con las parametrizaciones de los métodos que intervienen en la conversión<ul><li>**class** =  nombre de la clase CSS aplicado en la salida como tabla HTML</li><li>**colnames** =  array con los nombres de las columnas (null)</li><li>**convert_spaces** =  Determina si deben convertirse los caracteres de espacio</li><li>**convert_unicode** =  Determina si los caracteres UTF-8 deberán ser convertidos a formato UNICODE (\uXXXX)</li><li>**joiner** =  caracter por el que se unirán los campos (,)</li><li>**enclose** =  caracter que se utilizará para encerrar los valores de los campos (&quot;)</li><li>**enclosed** =  caracter utilizado para encerrar los valores de los campos (&quot;)</li><li>**eol** =  fin de línea (\r\n)</li><li>**escape** =  caracter de escape para caracteres especiales (\) Salida de datos</li><li>**escaped** =  caracter para escapar los caracteres especiales (\) Entrada de datos</li><li>**format** =  formato de salida para el modo HTML (table|div|list)</li><li>**level** =  actual nivel de anidamiento (-1)</li><li>**splitter** =  caracter separador de campos (,)</li><li>**tag** =  nombre del siguiente tag XML (vacio)</li><li>**use_colnames** =  en combinación con **colnames**, establece los índices del array. 
Con valor true y colnames = null, se utilizarán como índices los valores de la primera fila.
(false)</li><li>**xml_attributes** =  determina si se deben procesar o no los atributos de las etiquetas XML (falso)</li></ul>|
### Ejemplos  
#### xml-csv  
```php
$data = "
    <months>
        <month><name>enero</name><number>01</number></month>
        <month><name>febrero</name><number>02</number></month>
        <month><name>marzo</name><number>03</number></month>
        <month><name>abril</name><number>04</number></month>
        <month><name>mayo</name><number>05</number></month>
        <month><name>junio</name><number>06</number></month>
        <month><name>julio</name><number>07</number></month>
        <month><name>agosto</name><number>08</number></month>
        <month><name>septiembre</name><number>09</number></month>
        <month><name>octubre</name><number>10</number></month>
        <month><name>noviembre</name><number>11</number></month>
        <month><name>diciembre</name><number>12</number></month>
    </months>
";

echo $ngl("shift")->convert($data, "xml-csv");

# salida
"enero","01"
"febrero","02"
"marzo","03"
"abril","04"
"mayo","05"
"junio","06"
"julio","07"
"agosto","08"
"septiembre","09"
"octubre","10"
"noviembre","11"
"diciembre","12"
```
#### array-json  
```php
$data = array(
    array("firstName" => "John" , "lastName" => "Doe", "age"=>36),
    array("firstName" => "Anna" , "lastName" => "Smith", "age"=>15),
    array("firstName" => "Peter" , "lastName" => "Jones", "age"=>42)
);

echo $ngl("shift")->convert($data, "array-json");

# salida
[
    {"firstName":"John","lastName":"Doe","age":36},
    {"firstName":"Anna","lastName":"Smith","age":15},
    {"firstName":"Peter","lastName":"Jones","age":42}
]
```

&nbsp;
___
&nbsp;

## csvEncode
Genera una cadena formateada como CSV partiendo de un Array  

**[string]** =  *public* function ( *array* \$aData, *array* \$vOptions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData**|array||Array de datos|
|**\$vOptions**|array||Array de opciones del método:<br /><ul><li>**colnames** =  array con los nombres de las columnas (null)</li><li>**joiner** =  caracter por el que se unirán los campos (,)</li><li>**enclose** =  caracter que se utilizará para encerrar los valores de los campos (&quot;)</li><li>**escape** =  caracter de escape para caracteres especiales (\)</li><li>**eol** =  fin de línea (\r\n)</li></ul>|

&nbsp;
___
&nbsp;

## CSVParseLine
auxiliar del método nglShift::csvToArray. convierte una linea CSV en un array  

**[array]** =  *private* function ( *string* \$sSplitter, *string* \$sEnclosed, *string* \$sEscaped, *string* \$sEOL );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sSplitter**|string|,|caracter separador de campos|
|**\$sEnclosed**|string|&quot;|caracter utilizado para encerrar los valores de los campos|
|**\$sEscaped**|string|\|caracter para escapar los caracteres especiales|
|**\$sEOL**|string|\r\n|fin de línea|

&nbsp;
___
&nbsp;

## csvToArray
convierte un texto CSV (una línea o conjunto de ellas) en un array bidimensional  

**[string]** =  *public* function ( *array* \$aData, *array* \$vOptions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData**|array||Array de datos|
|**\$vOptions**|array||Array de opciones del método:<br /><ul><li>**use_colnames** =  en combinación con **colnames**, establece los índices del array. 
Con valor true y colnames = null, se utilizarán como índices los valores de la primera fila.
(false)</li><li>**columns** =  array con los nombres de las columnas (null)</li><li>**splitter** =  caracter separador de campos (,)</li><li>**enclosed** =  caracter utilizado para encerrar los valores de los campos (&quot;)</li><li>**escaped** =  caracter para escapar los caracteres especiales (\)</li><li>**eol** =  fin de línea (\r\n)</li></ul>|

&nbsp;
___
&nbsp;

## fixedExplode
Convierte una cadena en Array separando sus partes por caracter fijo  

**[array]** =  *public* function ( *string* \$sString, *array* \$vOptions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena de datos|
|**\$vOptions**|array||Array de opciones del método:<br /><ul><li>**positions** =  posiciones de corte (null)</li><li>**trim** =  determina si debe aplicarse el método trim a cada valor obtenido (false)</li><li>**eol** =  determina si debe tratarse a \$sString como una cadena multilinea (false)</li></ul>|
### Ejemplos  
#### sin TRIM  
```php
$data = "John            Doe       Director    36";
$options = array("positions" => array(16,10,12,2));
print_r($ngl("shift")->fixedExplode($data, $options));

# salida
Array (
    [0] => "John            "
    [1] => "Doe       "
    [2] => "Director    "
    [3] => 36
)
```
#### con TRIM  
```php
$data = "John            Doe       Director    36";
$options = array("positions" => array(16,10,12,2), "trim" => true);
print_r($ngl("shift")->fixedExplode($data, $options));

# salida
Array (
    [0] => "John"
    [1] => "Doe"
    [2] => "Director"
    [3] => 36
)
```

&nbsp;
___
&nbsp;

## fixedImplode
Convierte un Array en una cadena respetando las logitudes de **positions**. Si la longuitud de la cadena es superior al valor de **positions**, el valor será truncado.  

**[array]** =  *public* function ( *array* \$aString, *array* \$vOptions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aString**|array||Array de datos|
|**\$vOptions**|array||Array de opciones del método:<br /><ul><li>**positions** =  posiciones de unión (null)</li><li>**fill** =  caracter de relleno ( espacio )</li><li>**eol** =  fin de línea (\r\n)</li></ul>|
### Ejemplos  
#### ejemplo #1  
```php
$data = array("John", "Doe", "Director", "36");
$options = array("positions" => array(16,10,12,2), "fill" => ".");
echo $ngl("shift")->fixedImplode($data, $options);

# salida
John............Doe.......Director....36
```
#### ejemplo #2  
```php
$data = array("ENERO", "FEBRERO", "MARZO", "ABRIL", "MAYO", "JUNIO");
$options = array("positions" => array(5,5,5,5,5,5), "fill" => "-");
echo $ngl("shift")->fixedImplode($data, $options);

# salida
ENEROFEBREMARZOABRILMAYO-JUNIO
```

&nbsp;
___
&nbsp;

## html
Genera una salida HTML a partir de un Array  

**[string]** =  *public* function ( *array* \$aData, *string* \$sFormat, *string* \$sClassName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData**|array||Array de datos|
|**\$sFormat**|string|table|Tipo de salida HTML:<br /><ul><li>**table** =  tabla HTML</li><li>**div** =  estructura de DIVs</li><li>**list** =  estructura de UL, LI y SPAN</li></ul>|
|**\$sClassName**|string|data|Nombre de la clase CSS que se asignara a la tabla, filas (*-head/*-row) y columnas (*-cell)|
### Ejemplos  
#### TABLA  
```php
$data = array(
    array("firstName" => "John" , "lastName" => "Doe", "age"=>36),
    array("firstName" => "Anna" , "lastName" => "Smith", "age"=>15),
    array("firstName" => "Peter" , "lastName" => "Jones", "age"=>42)
);
echo $ngl("shift")->html($data, "table", "users");

# salida
<table class="users">
    <tr class="users-head">
        <th class="users-cell">firstName</th>
        <th class="users-cell">lastName</th>
        <th class="users-cell">age</th>
    </tr>
    <tr class="users-row">
        <td class="users-cell">John</td>
        <td class="users-cell">Doe</td>
        <td class="users-cell">36</td>
    </tr>
    <tr class="users-row">
        <td class="users-cell">Anna</td>
        <td class="users-cell">Smith</td>
        <td class="users-cell">15</td>
    </tr>
    <tr class="users-row">
        <td class="users-cell">Peter</td>
        <td class="users-cell">Jones</td>
        <td class="users-cell">42</td>
    </tr>
</table>
```
#### Estructura de DIVs  
```php
$data = array(
    array("firstName" => "John" , "lastName" => "Doe", "age"=>36),
    array("firstName" => "Anna" , "lastName" => "Smith", "age"=>15),
    array("firstName" => "Peter" , "lastName" => "Jones", "age"=>42)
);
echo $ngl("shift")->html($data, "div", "users");

# salida
<div class="users">
    <div class="users-head">
        <div class="users-cell">firstName</div>
        <div class="users-cell">lastName</div>
        <div class="users-cell">age</div>
    </div>
    <div class="users-row">
        <div class="users-cell">John</div>
        <div class="users-cell">Doe</div>
        <div class="users-cell">36</div>
    </div>
    <div class="users-row">
        <div class="users-cell">Anna</div>
        <div class="users-cell">Smith</div>
        <div class="users-cell">15</div>
    </div>
    <div class="users-row">
        <div class="users-cell">Peter</div>
        <div class="users-cell">Jones</div>
        <div class="users-cell">42</div>
    </div>
</div>
```
#### Estructura de UL, LI y SPAN  
```php
$data = array(
    array("firstName" => "John" , "lastName" => "Doe", "age"=>36),
    array("firstName" => "Anna" , "lastName" => "Smith", "age"=>15),
    array("firstName" => "Peter" , "lastName" => "Jones", "age"=>42)
);
echo $ngl("shift")->html($data, "list", "users");

# salida
<ul class="users">
    <li class="users-head">
        <span class="users-cell">firstName</span>
        <span class="users-cell">lastName</span>
        <span class="users-cell">age</span>
    </li>
    <li class="users-row">
        <span class="users-cell">John</span>
        <span class="users-cell">Doe</span>
        <span class="users-cell">36</span>
    </li>
    <li class="users-row">
        <span class="users-cell">Anna</span>
        <span class="users-cell">Smith</span>
        <span class="users-cell">15</span>
    </li>
    <li class="users-row">
        <span class="users-cell">Peter</span>
        <span class="users-cell">Jones</span>
        <span class="users-cell">42</span>
    </li>
</ul>
```

&nbsp;
___
&nbsp;

## htmlToArray
Convierte una Tabla HTML en un array, utilizando el objeto DOMDocument.
Las tablas pueden o no tener THEAD y TBODY. En el caso de tener THEAD los valores de los TH serán 
utilizados como indices alfanuméricos en el Array de salida.
El método soporta multiples tablas y anidamiento de tablas; en este último caso generará sub-arrays  

**[array]** =  *public* function ( *string* \$sHTML );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sHTML**|string||Código HTML que contiene la o las tablas|

&nbsp;
___
&nbsp;

## HTMLTableParser
Auxiliar del método htmlToArray  

**[DOMNodeList]** =  *private* function ( *string* \$table );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$table**|string||Código HTML que contiene la o las tablas|

&nbsp;
___
&nbsp;

## JSONChar
Auxiliar del método nglShift::jsonEncode encargado de codificar un caracter para que sea válido dentro de una cadena UTF-8  

 *private* function ( );
  

&nbsp;
___
&nbsp;

## jsonDecode
Decodifica una cadena JSON de un Array  

**[array]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena JSON|

&nbsp;
___
&nbsp;

## jsonEncode
Codifica un valor en una cadena JSON  

 *public* function ( );
  

&nbsp;
___
&nbsp;

## jsonFormat
Auxiliar del método nglShift::jsonEncode encargado generar un par ordenado NOMBRE:VALOR válido  

**[string]** =  *public* function ( *string* \$sJson, *boolean* \$bCompress, *boolean* \$bHTML, *string* \$sTab, *string* \$sEOL );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sJson**|string||Cadena JSON|
|**\$bCompress**|boolean||Cuando es TRUE, se retorna la cadena original sin saltos de líneas ni tabulaciones|
|**\$bHTML**|boolean||Determina si el resultado debe ser tratado con htmlentities|
|**\$sTab**|string|\t|Tabulador|
|**\$sEOL**|string|\n|Salto de línea|

&nbsp;
___
&nbsp;

## JSONNameValuePair
Auxiliar del método nglShift::jsonEncode encargado generar un par ordenado NOMBRE:VALOR válido  

**[string]** =  *private* function ( *string* \$sName, *mixed* \$mValue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sName**|string||Nombre del índice|
|**\$mValue**|mixed||Valor|

&nbsp;
___
&nbsp;

## JSONReduceString
Auxiliar del método nglShift::jsonDecode encargado limpiar el código antes de ser parseado  

**[string]** =  *private* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena JSON|

&nbsp;
___
&nbsp;

## objToArray
Convierte un objeto en un array asosiativo de manera recursiva  

**[array]** =  *public* function ( *object* \$mObject );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mObject**|object||Objeto a convertir|

&nbsp;
___
&nbsp;

## objFromArray
Convierte un Array en un Objeto de manera recursiva  

**[object]** =  *public* function ( *array* \$mObject );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mObject**|array||Array a convertir|

&nbsp;
___
&nbsp;

## XMLChildren
Auxiliar del método ngl:Babel::xmlToArray utilizado para recorrer el objeto XML de manera recursiva  

**[array]** =  *private* function ( *object* \$vXML, *boolean* \$bAttributes, *int* \$x );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vXML**|object||Objecto XML|
|**\$bAttributes**|boolean||Determina si se deben procesar o no los atributos de las etiquetas XML|
|**\$x**|int||Contador interno|

&nbsp;
___
&nbsp;

## xmlEncode
Convierte un array en una estructura XML  

**[string]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## xmlToArray
Vuelca el contenido de un texto XML en un array asosiativo de manera recursiva  

**[array]** =  *public* function ( *string* \$sXML, *array* \$vOptions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sXML**|string||Estructura XML|
|**\$vOptions**|array||Array de opciones del método:<br /><ul><li>**xml_attributes** =  determina si se deben procesar o no los atributos de las etiquetas XML (falso)</li></ul>|

&nbsp;
___
&nbsp;

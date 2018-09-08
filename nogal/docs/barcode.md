# nogal::barcode
Implementa la clase 'barcode-generator' para generar códigos de barras
___
  
## Variables
`public` $barcode = Objeto original 'barcode-generator'  
`private` $vtypenologies = Tecnologías de códigos de barras soportadas  

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**color**|string|#000000|Color de las barras en formato #RRGGBB|
|**content**|string|test1234|Contenido del código|
|**height**|int|30|Altura máxima de la imagen del código|
|**margin**|int|2|Margen de la imagen generada|
|**size**|string|2|Tamaño de la tipografía en el código|
|**type**|string|code128|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[base64](#base64)|Genera y retorna el código de barras en formato PNG Base64|
|[html](#html)|Genera y retorna el código de barras en formato HTML|
|[image](#image)|Genera y retorna el puntero de la imagen del código de barras|
|[png](#png)|Genera y retorna el código de barras en formato PNG|
|[svg](#svg)|Genera y retorna el código de barras en formato SVG|
|[text](#text)|Genera y retorna una secuencia de 0 y 1 del código de barras|
|**Internos**||
|[SetType](#settype)|Establece el algoritmo utilizado para generar el códigoal invocar al argumento **type**.|

&nbsp;

## base64
> Genera y retorna el código de barras en formato PNG Basa64  

**[string]** =  *public* function ( *string* $sContent, *string* $sType, *string* $nSize, *int* $nHeight, *string* $sColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sContent**|string|*arg::content*|Contenido del código|
|**$sType**|string|*arg::type*|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
|**$nSize**|string|*arg::size*Tamaño de la tipografía en el código|
|**$nHeight**|int|*arg::height*|Altura máxima de la imagen del código|
|**$sColor**|string|*arg::color*|Color de las barras en formato #RRGGBB|

### Ejemplos  
#### impresión de código
```php
# 50px de alto
echo $ngl("barcode")->height(50)->text("test")
```

&nbsp;
___
&nbsp;

## html
> Genera y retorna el código de barras en formato HTML  

**[string]** =  *public* function ( *string* $sContent, *string* $sType, *string* $nSize, *int* $nHeight, *string* $sColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sContent**|string|*arg::content*|Contenido del código|
|**$sType**|string|*arg::type*|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
|**$nSize**|string|*arg::size*Tamaño de la tipografía en el código|
|**$nHeight**|int|*arg::height*|Altura máxima de la imagen del código|
|**$sColor**|string|*arg::color*|Color de las barras en formato #RRGGBB|

&nbsp;
___
&nbsp;

## image
> Genera y retorna el puntero de la imagen del código de barras  

**[image resource]** =  *public* function ( *string* $sType, *string* $sContent, *string* $nSize, *int* $nHeight, *string* $sColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sContent**|string|*arg::content*|Contenido del código|
|**$sType**|string|*arg::type*|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
|**$nSize**|string|*arg::size*Tamaño de la tipografía en el código|
|**$nHeight**|int|*arg::height*|Altura máxima de la imagen del código|
|**$sColor**|string|*arg::color*|Color de las barras en formato #RRGGBB|

### Ejemplos  
#### impresión de imagen  
```php
$barcode = $ngl("barcode");
$barcode->args(array("size" => 5, "margin" => 2));
$ngl("image.code")->load($barcode->image("test"))->view();
```

&nbsp;
___
&nbsp;

## png
> Genera y retorna el código de barras en formato PNG  

**[string]** =  *public* function ( *string* $sContent, *string* $sType, *string* $nSize, *int* $nHeight, *string* $sColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sContent**|string|*arg::content*|Contenido del código|
|**$sType**|string|*arg::type*|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
|**$nSize**|string|*arg::size*Tamaño de la tipografía en el código|
|**$nHeight**|int|*arg::height*|Altura máxima de la imagen del código|
|**$sColor**|string|*arg::color*|Color de las barras en formato #RRGGBB|

&nbsp;
___
&nbsp;

## svg
> Genera y retorna el código de barras en formato SVG  

**[string]** =  *public* function ( *string* $sContent, *string* $sType, *string* $nSize, *int* $nHeight, *string* $sColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sContent**|string|*arg::content*|Contenido del código|
|**$sType**|string|*arg::type*|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
|**$nSize**|string|*arg::size*|Tamaño de la tipografía en el código|
|**$nHeight**|int|*arg::height*|Altura máxima de la imagen del código|
|**$sColor**|string|*arg::color*|Color de las barras en formato #RRGGBB|

&nbsp;
___
&nbsp;

## text
> Genera y retorna una secuencia de 0 y 1 que representa código de barras. Donde cada valor equivale a un pixel de ancho y donde 1 significa barra y 0 significa espacio vacio

**[string]** =  *public* function ( *string* $sContent, *string* $sType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sContent**|string|*arg::content*|Contenido del código|
|**$sType**|string|*arg::type*|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|

### Ejemplos  
#### impresión del texto  
```php
echo $ngl("barcode")->text("test");

#salida
# 1101001000010011110100101100100001011110010010011110100111100101001100011101011
```

&nbsp;
___
&nbsp;

# Internos
## SetType
> Establece el algoritmo utilizado para generar el códigoal invocar al argumento **type**.

**[int]** =  *private* function ( *string* $sType );  

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
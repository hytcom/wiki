# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# barcode
## nglBarCode *extends* nglStd [instanciable] [%REVISION%]
Implementa la clase 'barcode-generator', que genera códigos de barras (third-party)
  
## Variables
`public` $barcode = Objeto original 'barcode-generator'  
`private` $vtypenologies = Tecnologías de códigos de barras soportadas  

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**color**|string|#000000|Color de las barras en formato #RRGGBB|
|**content**|string|test1234|Contenido del código|
|**margin**|int|2|Margen de la imagen generada|
|**maxheight**|int|30|Altura máxima de la imagen del código|
|**options**|mixed|null|Argumento opcional empleado en algunos algoritmos:<ul><li>**code128** =  letra del comienzo del código (A|B|C)</li><li>**code39** =  checksum (true|false)</li><li>**ean13** =  book (true|false)</li><li>**i25** =  checksum (true|false)</li><li>**s25** =  checksum (true|false)</li></ul>|
|**resolution**|string|1|Resolución de la imagen del código|
|**size**|string|2|Tamaño de la tipografía en el código|
|**type**|string|code128|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[html](#html)|Genera y retorna el c�digo de barras en formato HTML|
|[image](#image)|Genera y retorna el puntero de la imagen del c�digo de barras|
|[png](#png)|Genera y retorna el c�digo de barras en formato PNG|
|[svg](#svg)|Genera y retorna el c�digo de barras en formato SVG|
|[text](#text)|Genera y retorna una secuencia de 0 y 1 del c�digo de barras|

  
&nbsp;


## html
Genera y retorna el código de barras en formato HTML  

**[string]** =  *public* function ( *string* \$sContent, *string* \$sType, *string* \$nSize, *int* \$nMaxHeight, *string* \$sColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sContent**|string|test1234|Contenido del código|
|**\$sType**|string|code128|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
|**\$nSize**|string|2|Tamaño de la tipografía en el código|
|**\$nMaxHeight**|int|30|Altura máxima de la imagen del código|
|**\$sColor**|string|#000000|Color de las barras en formato #RRGGBB|

&nbsp;
___
&nbsp;

## svg
Genera y retorna el código de barras en formato SVG  

**[string]** =  *public* function ( *string* \$sContent, *string* \$sType, *string* \$nSize, *int* \$nMaxHeight, *string* \$sColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sContent**|string|test1234|Contenido del código|
|**\$sType**|string|code128|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
|**\$nSize**|string|2|Tamaño de la tipografía en el código|
|**\$nMaxHeight**|int|30|Altura máxima de la imagen del código|
|**\$sColor**|string|#000000|Color de las barras en formato #RRGGBB|

&nbsp;
___
&nbsp;

## png
Genera y retorna el código de barras en formato PNG  

**[string]** =  *public* function ( *string* \$sContent, *string* \$sType, *string* \$nSize, *int* \$nMaxHeight, *string* \$sColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sContent**|string|test1234|Contenido del código|
|**\$sType**|string|code128|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
|**\$nSize**|string|2|Tamaño de la tipografía en el código|
|**\$nMaxHeight**|int|30|Altura máxima de la imagen del código|
|**\$sColor**|string|#000000|Color de las barras en formato #RRGGBB|

&nbsp;
___
&nbsp;

## image
Genera y retorna el puntero de la imagen del código de barras  

**[image resource]** =  *public* function ( *string* \$sType, *string* \$sContent, *string* \$nSize, *int* \$nMaxHeight, *string* \$sResolution );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sType**|string|code128|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
|**\$sContent**|string|test1234|Contenido del código|
|**\$nSize**|string|2|Tamaño de la tipografía en el código|
|**\$nMaxHeight**|int|30|Altura máxima de la imagen del código|
|**\$sResolution**|string|1|Resolución de la imagen del código|
### Ejemplos  
#### impresión de imagen  
```php
$barcode = $ngl("barcode.");
$barcode->args(array("size" => 5, "margin" => 2));
$ngl("image.code")->load($barcode->image("test"))->view();
```

&nbsp;
___
&nbsp;

## text
Genera y retorna una secuencia de 0 y 1 del código de barras  

**[string]** =  *public* function ( *string* \$sContent, *string* \$sType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sContent**|string|test1234|Contenido del código|
|**\$sType**|string|code128|Algoritmo utilizado para generar el código. Códigos soportados:<ul><li>CODE_39</li><li>CODE_39_CHECKSUM</li><li>CODE_39E</li><li>CODE_39E_CHECKSUM</li><li>CODE_93</li><li>STANDARD_2_5</li><li>STANDARD_2_5_CHECKSUM</li><li>INTERLEAVED_2_5</li><li>INTERLEAVED_2_5_CHECKSUM</li><li>CODE_128</li><li>CODE_128_A</li><li>CODE_128_B</li><li>CODE_128_C</li><li>EAN_2</li><li>EAN_5</li><li>EAN_8</li><li>EAN_13</li><li>UPC_A</li><li>UPC_E</li><li>MSI</li><li>MSI_CHECKSUM</li><li>POSTNET</li><li>PLANET</li><li>RMS4CC</li><li>KIX</li><li>IMB</li><li>CODABAR</li><li>CODE_11</li><li>PHARMA_CODE</li><li>PHARMA_CODE_TWO_TRACKS</li></ul>|
### Ejemplos  
#### impresión del texto  
```php
echo $ngl("barcode.")->text("test")
```

&nbsp;
___
&nbsp;

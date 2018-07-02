# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# image
## nglImage *extends* nglStd [instanciable] [20160201]
Crea un objeto sobre una imagen y permite trabajar con ella.
  
## Variables
`private` $image = Image resource  
`private` $fOutput = Función de salida de imagen  

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**alpha**|string| false|Determina si la siguiente copia tendra fondo transparente|
|**canvas_color**|string|#FFFFFF|Valor hexadecimal del color del canvas|
|**canvas_height**|string|0|Alto del canvas|
|**canvas_width**|string|0|Ancho del canvas|
|**filepath**|mixed|null|Ruta del archivo de imagen, puntero o null|
|**filter_name**|string|null|Filtro que se aplicará sobre la imagen
Los filtros disponibles son:<ul><li>**blur** =  Pone borrosa la imagen</li><li>**brightness** =  Cambia el brillo de la imagen</li><li>**colorize** =  Como **grayscale**, excepto que se puede especificar el color</li><li>**contrast** =  Cambia el contraste de la imagen</li><li>**emboss** =  Pone en relieve la imagen</li><li>**gaussian_blur** =  Pone borrosa la imagen usando el método Gaussiano</li><li>**grayscale** =  Convierte la imagen a escala de grises</li><li>**negative** =  Invierte todos los colores de la imagen</li><li>**pixelate** =  grayscale</li><li>**sharpe** =  Utiliza detección de borde para resaltar los bordes de la imagen</li><li>**sketch** =  Utiliza eliminación media para lograr un efecto superficial</li><li>**smooth** =  Suaviza la imagen</li></ul>|
|**filter_args**|mixed|null|Argumento solicitado por algunos de los filtros<ul><li>**brightness** =  Nivel de brillo, rango: -255 a 255</li><li>**colorize** =  Color hexadecimal con canal alpha: #RRGGBBAA</li><li>**contrast** =  Nivel de contraste</li><li>**pixelate** =  Tamaño de bloque de pixelación</li><li>**smooth** =  Nivel de suavidad</li></ul>|
|**height**|string|0|Alto que se aplicará en la próxima copia de la imagen actual|
|**merge_image**|resource||Puntero de la imagen que se incorporará a la imagen actual|
|**merge_alpha**|string| true|Determina si la imagen **merge_image** será incorporada en modo de transparencia|
|**merge_position**|string|center center|Posición de la imagen **merge_image** en el canvas actual|
|**position**|string|center center|Posición de la imagen en el canvas, este valor puede ser un par ordenado de coordenadas TOP y LEFT separados por ; (punto y coma) ó , (coma) ó alguna de las siguientes combinaciones:<ul><li>top left</li><li>top center</li><li>top right</li><li>center left</li><li>center center</li><li>center right</li><li>bottom left</li><li>bottom center</li><li>bottom right</li></ul>|
|**quality**|string|75|Calidad de la imagen en el método de salida nglImage::write|
|**rc_find**|string|#000000|Valor hexadecimal del color que se desea reemplazar en la imagen|
|**rc_replace**|string|#FFFFFF|Valor hexadecimal del color con el que será reemplazado **rc_find**|
|**rc_tolerance**|string|0|Grado de tolerancia (0-255) aplicado a la hora reemplazar colores|
|**text_content**|string||Texto que se escribirá sobre la imagen actual|
|**text_angle**|int|0|Angulo de escritura|
|**text_color**|string|#000000|Color del texto|
|**text_font**|string|null|Ruta del archivo TTF con la que se escribirá el texto|
|**text_margin**|string|0|Margin aplicado al texto, valor entero o dos enteros separados por un espacio|
|**text_position**|string|center center|Posición del texto dentro del canvas, en igual formato que argument::position|
|**text_size**|int|10|Tamaño del texto|
|**type**|string|jpeg|Tipo de imagen. Pueden ser: jpeg, jpg, png o gif|
|**width**|string|0|Ancho que se aplicará en la próxima copia de la imagen actual|

## Atributos
|Atributo|Tipo|Descripción|
|---|---|---|
|**data**|array|Array con los datos IPTC y EXIF|
|**height**|int|Alto de la imagen actual|
|**image**|resource|Puntero de la imagen actual|
|**info**|array|Array con los datos de getimagesize|
|**mime**|string|MimeType de la imagen actual|
|**path**|string|Ruta de la imagen|
|**size**|int|Tamaño en bytes de la imagen actual|
|**type**|string|Tipo de imagen actual|
|**width**|int|Ancho de la imagen actual|

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[CalculatePosition](#CalculatePosition)|Calcula el TOP y LEFT en base al alto y ancho de la imagen y el alto y ancho del canvas en funci�n del par�metro $sPosition.Cuando $nWidth = $nCanva...|
|[CalculateSizes](#CalculateSizes)|Calcula el el ancho y alto de una imagen y su lienzo manteniendo la proporcionalidadEste m�todo retorna un array de 4 indices:anchoaltoancho del lien...|
|[CreateCopy](#CreateCopy)|Redimensiona el lienzo de la imagen|
|[GetTransparency](#GetTransparency)|Obtiene el grado de transparencia de la imagen|
|[base64](#base64)|Exporta el contenido de imagen para ser usado como origen de datos de  o css|
|[canvas](#canvas)|Redimensiona el lienzo de la imagen|
|[data](#data)|Retorna los datos IPTC y EXIF que pueda contener la imagen|
|[filter](#filter)|Aplica un filtro o efecto sobre la imagen actual|
|[image](#image)|Retorna el puntero de la imagen para ser utilizado en otro proceso|
|[load](#load)|Carga la imagen en el objeto.Si el par�metro $mFile fuese null, se crear� una imagen vacia de 1x1 px|
|[margin](#margin)|A�ade un margen por fuera de los limites de la imagen.Si una imagen mide 100px de ancho y se le a�aden 10px de margen, el nuevo ancho ser� de 120px|
|[merge](#merge)|Inserta una imagen dentro de otra|
|[padding](#padding)|A�ade un margen por dentro de los limites de la imagen.Si una imagen mide 100px de ancho y se le a�aden 10px de padding, el ancho seguir� siendo de...|
|[replace](#replace)|Reemplaza un color por otro.El reemplazo de colores en una imagen no es algo sencillo, mucho colores pueden parecer iguales a la vista, pero no lo son...|
|[resize](#resize)|Redimensiona una imagen|
|[text](#text)|Inserta una imagen dentro de otra|
|[view](#view)|Exportar la imagen al navegador|
|[write](#write)|Exportar la imagen a un archivo|

  
&nbsp;


## base64
Exporta el contenido de imagen para ser usado como origen de datos de <img> o css  

**[string]** =  *public* function ( *string* \$bAlpha );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$bAlpha**|string| false|Determina si la siguiente copia tendra fondo transparente|
### Ejemplos  
#### Uso  
```php
echo "<img src='".$ngl("image.")->load("demo.jpg")->base64()."' />";
```

&nbsp;
___
&nbsp;

## CalculatePosition
Calcula el TOP y LEFT en base al alto y ancho de la imagen y el alto y ancho del canvas en función del parámetro **\$sPosition**.
Cuando \$nWidth = \$nCanvasWidth y \$nHeight = \$nCanvasHeight, el método retonará 0 para el top y el left
Este método retorna un array de 2 indices:<ul><li>top</li><li>left</li></ul>  

**[array]** =  *private* function ( *string* \$sPosition, *int* \$nWidth, *int* \$nHeight, *int* \$nCanvasWidth, *int* \$nCanvasHeight );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPosition**|string||Valores de entrada TOP y LEFT, en el formato de argument::position|
|**\$nWidth**|int||Ancho de la imagen|
|**\$nHeight**|int||Alto de la imagen|
|**\$nCanvasWidth**|int||Ancho del canvas|
|**\$nCanvasHeight**|int||Alto del canvas|

&nbsp;
___
&nbsp;

## CalculateSizes
Calcula el el ancho y alto de una imagen y su lienzo manteniendo la proporcionalidad
Este método retorna un array de 4 indices:<ul><li>ancho</li><li>alto</li><li>ancho del lienzo</li><li>alto del lienzo</li></ul>  

**[array]** =  *private* function ( *int* \$nArgWidth, *int* \$nArgHeight, *int* \$nArgCanvasWidth, *int* \$nArgCanvasHeight );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nArgWidth**|int||Nuevo ancho de la imagen|
|**\$nArgHeight**|int||Nuevo alto de la imagen|
|**\$nArgCanvasWidth**|int||Nuevo ancho del canvas|
|**\$nArgCanvasHeight**|int||Nuevo alto del canvas|

&nbsp;
___
&nbsp;

## canvas
Redimensiona el lienzo de la imagen  

**[$this]** =  *public* function ( *string* \$nNewCanvasWidth, *string* \$nNewCanvasHeight, *string* \$sCanvasColor, *string* \$sPosition, *string* \$bAlpha );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nNewCanvasWidth**|string|0|Ancho del canvas|
|**\$nNewCanvasHeight**|string|0|Alto del canvas|
|**\$sCanvasColor**|string|#FFFFFF|Valor hexadecimal del color del canvas|
|**\$sPosition**|string|center center|Posición de la imagen en el canvas, este valor puede ser un par ordenado de coordenadas TOP y LEFT separados por ; (punto y coma) ó , (coma) ó alguna de las siguientes combinaciones:<ul><li>top left</li><li>top center</li><li>top right</li><li>center left</li><li>center center</li><li>center right</li><li>bottom left</li><li>bottom center</li><li>bottom right</li></ul>|
|**\$bAlpha**|string| false|Determina si la siguiente copia tendra fondo transparente|
### Ejemplos  
#### Cambio del tamaño del canvas  
```php
$ngl("image.")->load("demo.jpg")->canvas(200,200)->view();
```

&nbsp;
___
&nbsp;

## CreateCopy
Redimensiona el lienzo de la imagen  

**[boolean]** =  *private* function ( *int* \$nWidth, *int* \$nHeight, *int* \$nCanvasWidth, *int* \$nCanvasHeight, *boolean* \$bAlpha, *string* \$sPosition, *string* \$sCanvasColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nWidth**|int||Ancho de la imagen|
|**\$nHeight**|int||Alto de la imagen|
|**\$nCanvasWidth**|int||Ancho del lienzo|
|**\$nCanvasHeight**|int||Alto del lienzo|
|**\$bAlpha**|boolean||Determina si la copia tendra fondo transparente|
|**\$sPosition**|string|center center|Valores de entrada TOP y LEFT, en el formato de argument::position|
|**\$sCanvasColor**|string|#FFFFFF|Valor hexadecimal para el color de fondo|

&nbsp;
___
&nbsp;

## data
Retorna los datos IPTC y EXIF que pueda contener la imagen  

**[array]** =  *public* function ( );
  
### Ejemplos  
#### Datos IPTC y EXIF  
```php
print_r($ngl("image.foo")->load("readme.txt")->data());
```

&nbsp;
___
&nbsp;

## filter
Aplica un filtro o efecto sobre la imagen actual  

**[$this]** =  *public* function ( *string* \$sFilter, *mixed* \$mValue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFilter**|string|null|Filtro que se aplicará sobre la imagen
Los filtros disponibles son:<ul><li>**blur** =  Pone borrosa la imagen</li><li>**brightness** =  Cambia el brillo de la imagen</li><li>**colorize** =  Como **grayscale**, excepto que se puede especificar el color</li><li>**contrast** =  Cambia el contraste de la imagen</li><li>**emboss** =  Pone en relieve la imagen</li><li>**gaussian_blur** =  Pone borrosa la imagen usando el método Gaussiano</li><li>**grayscale** =  Convierte la imagen a escala de grises</li><li>**negative** =  Invierte todos los colores de la imagen</li><li>**pixelate** =  grayscale</li><li>**sharpe** =  Utiliza detección de borde para resaltar los bordes de la imagen</li><li>**sketch** =  Utiliza eliminación media para lograr un efecto superficial</li><li>**smooth** =  Suaviza la imagen</li></ul>|
|**\$mValue**|mixed|null|Argumento solicitado por algunos de los filtros<ul><li>**brightness** =  Nivel de brillo, rango: -255 a 255</li><li>**colorize** =  Color hexadecimal con canal alpha: #RRGGBBAA</li><li>**contrast** =  Nivel de contraste</li><li>**pixelate** =  Tamaño de bloque de pixelación</li><li>**smooth** =  Nivel de suavidad</li></ul>|
### Ejemplos  
#### Aplicando filtros  
```php
$img = $ngl("image.foo")->load("demo.jpg");
$img->filter("blur")->filter("emboss")->filter("colorize", "#FF990066")->view();
```

&nbsp;
___
&nbsp;

## GetTransparency
Obtiene el grado de transparencia de la imagen  

**[array]** =  *private* function ( *resource* \$hSourceImage );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$hSourceImage**|resource||Imagen|

&nbsp;
___
&nbsp;

## image
Retorna el puntero de la imagen para ser utilizado en otro proceso  

**[resource]** =  *public* function ( *string* \$bAlpha );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$bAlpha**|string| false|Determina si la siguiente copia tendra fondo transparente|
### Ejemplos  
#### Obtener el puntero de la imagen  
```php
$img = $ngl("image.foo")->load("demo.jpg");
$img->filter("blur")->filter("emboss")->margin(10);

imagepng($img->image(), "demo2.jpg");
```

&nbsp;
___
&nbsp;

## load
Carga la imagen en el objeto.
Si el parámetro \$mFile fuese null, se creará una imagen vacia de 1x1 px  

**[$this o FALSE]** =  *public* function ( *mixed* \$mFile, *string* \$sType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mFile**|mixed|null|Ruta del archivo de imagen, puntero o null|
|**\$sType**|string|jpeg|Tipo de imagen. Pueden ser: jpeg, jpg, png o gif|
### Ejemplos  
#### Archivo de imagen  
```php
$ngl("image.foo")->load("demo.jpg")->view();
```
#### Carga de puntero  
```php
$ngl("image.foo")->load(
    $ngl("qr.bar")->image("test1234")
)->view();
```
#### Imagen vacia  
```php
$img = $ngl("image.foo");
$img->text_font = "./roboto.ttf";
$img->load()->resize(120,40)->text("hola mundo!", "#ffffff")->view();
```

&nbsp;
___
&nbsp;

## margin
Añade un margen por fuera de los limites de la imagen.
Si una imagen mide 100px de ancho y se le añaden 10px de margen, el nuevo ancho será de 120px  

**[$this]** =  *public* function ( *int* \$nMargin, *string* \$sCanvasColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nMargin**|int|argument::margin||
|**\$sCanvasColor**|string|#FFFFFF|Valor hexadecimal del color del canvas|
### Ejemplos  
#### Margen de 10 pixeles  
```php
$ngl("image.")->load("demo.jpg")->margin(10)->view();
```

&nbsp;
___
&nbsp;

## padding
Añade un margen por dentro de los limites de la imagen.
Si una imagen mide 100px de ancho y se le añaden 10px de padding, el ancho seguirá siendo de 100px, pero el fotograma pasará a medir 80px de ancho  

**[$this]** =  *public* function ( *int* \$nPadding, *string* \$sCanvasColor );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nPadding**|int|argument::padding||
|**\$sCanvasColor**|string|#FFFFFF|Valor hexadecimal del color del canvas|
### Ejemplos  
#### Padding de 10 pixeles  
```php
$ngl("image.")->load("demo.jpg")->padding(10)->view();
```

&nbsp;
___
&nbsp;

## merge
Inserta una imagen dentro de otra  

**[$this]** =  *public* function ( *resource* \$image, *string* \$sPosition, *string* \$bAlpha );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$image**|resource||Puntero de la imagen que se incorporará a la imagen actual|
|**\$sPosition**|string|center center|Posición de la imagen **merge_image** en el canvas actual|
|**\$bAlpha**|string| true|Determina si la imagen **merge_image** será incorporada en modo de transparencia|
### Ejemplos  
#### Marca de agua  
```php
$logo = $ngl("image.logo")->load("logo.png");
$img = $ngl("image.photo")->load("demo.jpg");
$img->merge($logo->image(), "center center", true);
$img->view();
```

&nbsp;
___
&nbsp;

## resize
Redimensiona una imagen  

**[$this]** =  *public* function ( *string* \$nNewWidth, *string* \$mNewHeight, *string* \$bAlpha );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nNewWidth**|string|0|Ancho que se aplicará en la próxima copia de la imagen actual|
|**\$mNewHeight**|string|0|Alto que se aplicará en la próxima copia de la imagen actual|
|**\$bAlpha**|string| false|Determina si la siguiente copia tendra fondo transparente|
### Ejemplos  
#### Cambio de alto y ancho  
```php
$ngl("image.")->load("demo.jpg")->resize(800,800)->view();
```
#### Ancho proporcional al alto  
```php
$ngl("image.")->load("demo.jpg")->resize(0,800)->view();
```
#### 300px para el lado mas grande de la imagen  
```php
$ngl("image.")->load("demo.jpg")->resize(300,"max")->view();
```

&nbsp;
___
&nbsp;

## replace
Reemplaza un color por otro.
El reemplazo de colores en una imagen no es algo sencillo, mucho colores pueden parecer iguales a la vista, pero no lo son.
Por ello este método es mas eficiente en el reemplazo de colores plenos en imagenes simples, como códigos QR, de barras o textos  

**[$this]** =  *public* function ( *string* \$sFind, *string* \$sReplace, *string* \$nTolerance );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFind**|string|#000000|Valor hexadecimal del color que se desea reemplazar en la imagen|
|**\$sReplace**|string|#FFFFFF|Valor hexadecimal del color con el que será reemplazado **rc_find**|
|**\$nTolerance**|string|0|Grado de tolerancia (0-255) aplicado a la hora reemplazar colores|
### Ejemplos  
#### Reemplazo sin tolerancia  
```php
# Reemplaza el blanco pleno por rojo
$ngl("image.")->load("demo.jpg")->replace("#FFFFFF", "#FF0000", 0)->view();
```
#### Reemplazo con tolerancia  
```php
# Reemplaza tonalidades de azul por azul pletno
$ngl("image.")->load("demo.jpg")->replace("#0000FF", "#0000FF", 50)->view();
```

&nbsp;
___
&nbsp;

## text
Inserta una imagen dentro de otra  

**[$this]** =  *public* function ( *string* \$sText, *string* \$sColor, *string* \$sPosition, *string* \$sMargin, *int* \$nFont, *int* \$nAngle, *string* \$bAlpha, *string* \$sFont );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sText**|string||Texto que se escribirá sobre la imagen actual|
|**\$sColor**|string|#000000|Color del texto|
|**\$sPosition**|string|center center|Posición del texto dentro del canvas, en igual formato que argument::position|
|**\$sMargin**|string|0|Margin aplicado al texto, valor entero o dos enteros separados por un espacio|
|**\$nFont**|int|10|Tamaño del texto|
|**\$nAngle**|int|0|Angulo de escritura|
|**\$bAlpha**|string| true|Determina si la imagen **merge_image** será incorporada en modo de transparencia|
|**\$sFont**|string|null|Ruta del archivo TTF con la que se escribirá el texto|
### Ejemplos  
#### Imagen vacia con texto  
```php
$img = $ngl("image.");
$img->text_font = "fonts/roboto.ttf";
$img->load()->resize(120,40)->text("Foo Bar Text", "#FFFFFF", "bottom left")->view();
```
#### Texto sobre una imagen  
```php
$img = $ngl("image.");
$img->text_font = "fonts/roboto.ttf";
$img->load("demo.jpg")->text("www.mydomain.com", "#FFFF00", "bottom right", -10)->view();
```

&nbsp;
___
&nbsp;

## view
Exportar la imagen al navegador  

**[void]** =  *public* function ( *string* \$bAlpha );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$bAlpha**|string| false|Determina si la siguiente copia tendra fondo transparente|
### Ejemplos  
#### Salida al navegador  
```php
$ngl("image.")->load("demo.jpg")->view();
```

&nbsp;
___
&nbsp;

## write
Exportar la imagen a un archivo  

**[$this]** =  *public* function ( *mixed* \$sFilePath, *string* \$nQuality );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFilePath**|mixed|null|Ruta del archivo de imagen, puntero o null|
|**\$nQuality**|string|75|Calidad de la imagen en el método de salida nglImage::write|
### Ejemplos  
#### Generar una miniatura  
```php
$ngl("image.")->load("demo.jpg")->resize(140,"max")->write("images/thumb.jpg");
```

&nbsp;
___
&nbsp;

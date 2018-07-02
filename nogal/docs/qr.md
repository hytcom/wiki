# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# qr
## nglGraftQR *extends* nglStd [instanciable] [20150930]
Implementa https://github.com/aferrandini/PHPQRCode
  
## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**content**|string|test1234|Contenido del código|
|**eclevel**|int|L|Nivel de corrección de errores (L|M|Q|H)|
|**size**|int|4|Cantidad de pixels por punto|
|**margin**|int|0|Margen entre el borde de la imagen y el contenido del código|

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[image](#image)|Genera y retorna el puntero de la imagen del c�digo QR|

  
&nbsp;


## image
Genera y retorna el puntero de la imagen del código QR  

**[image resource]** =  *public* function ( *string* \$sContent, *int* \$nMargin, *int* \$nPointSize, *int* \$sECLevel );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sContent**|string|test1234|Contenido del código|
|**\$nMargin**|int|0|Margen entre el borde de la imagen y el contenido del código|
|**\$nPointSize**|int|4|Cantidad de pixels por punto|
|**\$sECLevel**|int|L|Nivel de corrección de errores (L|M|Q|H)|
### Ejemplos  
#### impresión de imagen  
```php
$qr = $ngl("qr.");
$qr->args(array("size" => 5, "margin" => 2));
$ngl("image.code")->load($qr->image("test"))->view();
```

&nbsp;
___
&nbsp;

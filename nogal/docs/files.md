# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# files
## nglFiles *extends* nglStd [main] [20160201]
Métodos frecuentes para el manejo de archivos y directorios.
  
  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[RebuildFILES](#RebuildFILES)||
|[absPath](#absPath)|formatea un path como absoluto limpiando doble barras o referencias atras (|
|[basePaths](#basePaths)|Retorna la porción común de dos paths desde el inicio|
|[copyr](#copyr)|Copia archivos y directorios de manera recursiva|
|[ls](#ls)||
|[lsprint](#lsprint)||
|[maxUploadSize](#maxUploadSize)||
|[mkdirr](#mkdirr)|Crea un directorio|
|[unlinkr](#unlinkr)|Elimina archivos y directorios de manera recursiva|
|[upload](#upload)||

  
&nbsp;


## absPath
formatea un path como absoluto limpiando doble barras o referencias atras (..)  

**[string]** =  *public* function ( *string* \$sPath, *string* \$sSlash );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string||Path|
|**\$sSlash**|string|NGL_DIR_SLASH|Separador de directorios.|
### Ejemplos  
#### Ejemplo 1  
```php
$path = "/home/user/docs/../../user2/readme.txt";
echo $ngl("files")->absPath($path, "/");

#salida
"/home/user2/readme.txt"
```
#### Ejemplo 2  
```php
$path = "/home/user/docs/../../user2/readme.txt";
echo $ngl("files")->absPath($path, "_");

#salida
"home_user2_readme.txt"
```

&nbsp;
___
&nbsp;

## basePaths
Retorna la porción común de dos paths desde el inicio. Previa a la comparación limpia los paths con **nglCommon::clearPath**  

**[string]** =  *public* function ( *string* \$sPath1, *string* \$sPath2, *string* \$sSlash );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath1**|string||path 1|
|**\$sPath2**|string||path 2|
|**\$sSlash**|string|NGL_DIR_SLASH|Separador de directorios.|
### Ejemplos  
#### Ejemplo  
```php
$path1 = "/home/user/docs/readme.txt";
$path2 = "/home/user/docs/images/picture.jpg";
echo $ngl("files")->basePaths($path1, $path2);

#salidas
"/home/user/docs/"
```

&nbsp;
___
&nbsp;

## copyr
Copia archivos y directorios de manera recursiva. Retorna un Array de 2 indices:<ul><li>**0** =  Cantidad de archivos copiados y directorios creados.</li><li>**1** =  Log con el detalla de cada una de las operaciones.</li></ul>  

**[array]** =  *public* function ( *string* \$sSource, *string* \$sDestine, *string* \$sMask, *boolean* \$bRecursive, *boolean* \$bIncludeHidden, *boolean* \$bLog );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sSource**|string||Origen de la copia|
|**\$sDestine**|string||Destino de la copia|
|**\$sMask**|string||Mascara de archivos. Una expresion regular o un array de ellas que será tratado con OR|
|**\$bRecursive**|boolean|true|Determina si se deben incluir carpetas y sub-carpetas|
|**\$bIncludeHidden**|boolean|false|Determina si se deben copiar los archivos y carpetas ocultos|
|**\$bLog**|boolean|false|Activa el log|
### Ejemplos  
#### Copia completa  
```php
$path1 = "/home/user/mydocs";
$path2 = "/home/user2/mydocs";
print_r($ngl("files")->copyr($path1, $path2));

#salidas
Array (
    [0] => 13
    [1] =>
        copy    "/home/user/mydocs/data.pdf" => "/home/user2/mydocs/data.pdf"
        mkdir    "/home/user2/mydocs/docs"
        copy    "/home/user/mydocs/docs/bar.doc" => "/home/user2/mydocs/docs/bar.doc"
        copy    "/home/user/mydocs/docs/foo.doc" => "/home/user2/mydocs/docs/foo.doc"
        mkdir     "/home/user2/mydocs/images"
        copy    "/home/user/mydocs/images/image1.jpg" => "/home/user2/mydocs/images/image1.jpg"
        copy    "/home/user/mydocs/images/image2.jpg" => "/home/user2/mydocs/images/image2.jpg"
        copy    "/home/user/mydocs/images/image3.jpg" => "/home/user2/mydocs/images/image3.jpg"
        copy    "/home/user/mydocs/images/picture1.jpg" => "/home/user2/mydocs/images/picture1.jpg"
        copy    "/home/user/mydocs/images/picture2.jpg" => "/home/user2/mydocs/images/picture2.jpg"
        copy    "/home/user/mydocs/images/picture3.jpg" => "/home/user2/mydocs/images/picture3.jpg"
        copy    "/home/user/mydocs/images/picture4.jpg" => "/home/user2/mydocs/images/picture4.jpg"
        copy    "/home/user/mydocs/readme.txt" => "/home/user2/mydocs/readme.txt"
)
```
#### Copia selectiva  
```php
$path1 = "/home/user/mydocs";
$path2 = "/home/user2/mydocs";
$out = $ngl("files")->copyr($path1, $path2, array("*.doc", "image*"));
print_r($out);

#salidas
Array (
    [0] => 7
    [1] =>
        mkdir    "/home/user2/mydocs/docs"
        copy    "/home/user/mydocs/docs/bar.doc" => "/home/user2/mydocs/docs/bar.doc"
        copy    "/home/user/mydocs/docs/foo.doc" => "/home/user2/mydocs/docs/foo.doc"
        mkdir     "/home/user2/mydocs/images"
        copy    "/home/user/mydocs/images/image1.jpg" => "/home/user2/mydocs/images/image1.jpg"
        copy    "/home/user/mydocs/images/image2.jpg" => "/home/user2/mydocs/images/image2.jpg"
        copy    "/home/user/mydocs/images/image3.jpg" => "/home/user2/mydocs/images/image3.jpg"
)
```

&nbsp;
___
&nbsp;

## ls
lista el contenido de un directorio  

**[array]** =  *public* function ( *string* \$sPath, *string* \$mMask, *string* \$sMode, *boolean* \$bRecursive, *string* \$sChildren, *boolean* \$bIni );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string|.|Directorio|
|**\$mMask**|string|null|Mascara de archivos. Una expresión regular o un array de ellas que será tratado con OR|
|**\$sMode**|string|single|Modos de salida de información.<br /><ul><li>**single** =  array con los paths completos de los archivos y directorios listados</li><li>**signed** =  idem anterior pero con un * antepuesto cuando el path corresponda a un directorio</li><li>**info** =  información detallada de los archivos y directorios listados, sujeto a la disponibilidad del dato<ul><li>**type** =  file o dir</li><li>**basename** =  nombre del archivo</li><li>**extension** =  extensión del archivo</li><li>**filename** =  nombre del archivo sin extensión</li><li>**protocol** =  protocolo del archivo</li><li>**path** =  path completo desde \$sPath</li><li>**bytes** =  tamaño en bytes</li><li>**size** =  tamaño en la unidad de medida mas grande</li><li>**chmod** =  permisos</li><li>**timestamp** =  fecha UNIX</li><li>**date** =  fecha en formato Y-m-d H:i:s</li><li>**image** =  true o false</li></ul></li><li>**single-h** =  idem single pero incluirá los archivos y carpetas que comienzen con . (ocultos)</li><li>**signed-h** =  idem signed pero incluirá los archivos y carpetas que comienzen con . (ocultos)</li><li>**info-h** =  idem info pero incluirá los archivos y carpetas que comienzen con . (ocultos)</li></ul>|
|**\$bRecursive**|boolean|false|Búsqueda en modo recursivo|
|**\$sChildren**|string|_children|Nombre del nodo que se utilizará para anidar resultados cuando \$bRecursive=true|
|**\$bIni**|boolean|true|Reservada para uso interno de la función|
### Ejemplos  
#### Modo SINGLE (no recursivo)  
```php
$ls = $ngl("files")->ls("public_html");
print_r($ls);

# salida
Array (
    [0] => public_html/files
    [1] => public_html/functions.js
    [2] => public_html/gallery.html
    [3] => public_html/images
    [4] => public_html/index.html
    [5] => public_html/robots.txt
    [6] => public_html/styles.css
)
```
#### Modo SINGLE (recursivo)  
```php
$ls = $ngl("files")->ls("public_html", "*", "single", true);
print_r($ls);

# salida
Array (
    [0] => public_html/files
    [1] => public_html/files/document.docx
    [2] => public_html/files/info.docx
    [3] => public_html/functions.js
    [4] => public_html/gallery.html
    [5] => public_html/images
    [6] => public_html/images/image1.gif
    [7] => public_html/images/image2.gif
    [8] => public_html/images/image3.gif
    [9] => public_html/index.html
    [10] => public_html/robots.txt
    [11] => public_html/styles.css
)
```
#### Modo SIGNED (no recursivo)  
```php
$ls = $ngl("files")->ls("public_html", "*", "signed");
print_r($ls);

# salida
Array (
    [0] => *public_html/files
    [1] => public_html/functions.js
    [2] => public_html/gallery.html
    [3] => *public_html/images
    [4] => public_html/index.html
    [5] => public_html/robots.txt
    [6] => public_html/styles.css
)
```
#### Modo INFO (no recursivo)  
```php
$ls = $ngl("files")->ls("public_html/files", "*.docx", "info");
print_r($ls);

# salida
Array (
    [document.docx] => Array (
        [type] => file
        [basename] => document.docx
        [extension] => docx
        [filename] => document
        [protocol] => filesystem
        [path] => public_html/files/document.docx
        [bytes] => 364495
        [size] => 355.95KB
        [chmod] => 0666
        [timestamp] => 1361469382
        [date] => 2013-02-21 14:56:22
        [mime] => application/vnd.openxmlformats-officedocument.wordprocessingml.document
        [image] => 
    )

    [info.docx] => Array (
        [type] => file
        [basename] => info.docx
        [extension] => docx
        [filename] => info
        [protocol] => filesystem
        [path] => public_html/files/info.docx
        [bytes] => 87310
        [size] => 85.26KB
        [chmod] => 0666
        [timestamp] => 1425914852
        [date] => 2015-03-09 12:27:32
        [mime] => application/vnd.openxmlformats-officedocument.wordprocessingml.document
        [image] => 
    )
)
```

&nbsp;
___
&nbsp;

## lsprint
imprime el árbol de un directorio de manera recursiva  

**[array]** =  *public* function ( *string* \$sPath, *string* \$mMask, *string* \$sChildren );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string|.|Directorio|
|**\$mMask**|string|null|Mascara de archivos. Una expresión regular o un array de ellas que será tratado con OR|
|**\$sChildren**|string|_children|Nombre del nodo que se utilizará para anidar resultados cuando \$bRecursive=true|
### Ejemplos  
#### Ejemplo  
```php
echo $ngl("files")->lsprint("bootstrap");

# salida
bootstrap
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   ├── bootstrap-theme.css
│   ├── bootstrap-theme.css.map
│   └── bootstrap-theme.min.css
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    ├── glyphicons-halflings-regular.woff
    └── glyphicons-halflings-regular.woff2
```

&nbsp;
___
&nbsp;

## maxUploadSize
Retorna el máximo tamaño de archivo soportado por al configuración del servidor  

**[int]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## mkdirr
Crea un directorio. Si el directorio ya existe y \$bForce es TRUE, mkdir le agregará al nombre del directorio el sufijo _N donde N es el número de directorio con el mismo nombre +1  

**[boolean]** =  *public* function ( *string* \$sPath, *boolean* \$bForce );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string|.|Directorio|
|**\$bForce**|boolean|false|Fuerza la creación del directorio|

&nbsp;
___
&nbsp;

## RebuildFILES
Toma el array \$aFiles (de estructura igual a \$_FILES) y lo reordena de manera recursiva, para optener una lectura mas natural  

**[array]** =  *private* function ( *array* \$aFiles, *boolean* \$bTop );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aFiles**|array||Array de archivos|
|**\$bTop**|boolean|true|Indica cual es la primer iteración del método|

&nbsp;
___
&nbsp;

## unlinkr
Elimina archivos y directorios de manera recursiva. Retorna un Array de 2 indices:<ul><li>**0** =  Cantidad de archivos y directorios eliminados</li><li>**1** =  Log con el detalla de cada una de las operaciones</li></ul>  

**[array]** =  *public* function ( *string* \$sSource, *string* \$sMask, *boolean* \$bRecursive, *boolean* \$bIncludeHidden, *boolean* \$bLog );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sSource**|string||Target de borrado. Si el target es un directorio y la cadena NO termina con un slash, el mismo tambien será eliminado luego de vaciarse.|
|**\$sMask**|string|*|Mascara de archivos. Una expresion regular o un array de ellas que será tratado con OR|
|**\$bRecursive**|boolean|true|Determina si se deben incluir carpetas y sub-carpetas|
|**\$bIncludeHidden**|boolean|false|Determina si se deben eliminar los archivos y carpetas ocultos|
|**\$bLog**|boolean|true|Activa el log|
### Ejemplos  
#### Borrado completo  
```php
$del = $ngl("files")->unlinkr("/home/user2/mydocs");
print_r($del);

#salidas
Array (
    [0] => 9
    [1] =>
        delete    "/home/user2/mydocs/docs/bar.doc"
        delete    "/home/user2/mydocs/docs/foo.doc"
        delete     "/home/user2/mydocs/docs"
        delete    "/home/user2/mydocs/images/image1.jpg"
        delete    "/home/user2/mydocs/images/image2.jpg"
        delete    "/home/user2/mydocs/images/image3.jpg"
        delete    "/home/user2/mydocs/images/image3.jpg"
        delete     "/home/user2/mydocs/images"
        delete     "/home/user2/mydocs"
)
```
#### Borradon sólo el contenido  
```php
$del = $ngl("files")->unlinkr("/home/user2/mydocs/");
print_r($del);

#salidas
Array (
    [0] => 9
    [1] =>
        delete    "/home/user2/mydocs/docs/bar.doc"
        delete    "/home/user2/mydocs/docs/foo.doc"
        delete     "/home/user2/mydocs/docs"
        delete    "/home/user2/mydocs/images/image1.jpg"
        delete    "/home/user2/mydocs/images/image2.jpg"
        delete    "/home/user2/mydocs/images/image3.jpg"
        delete    "/home/user2/mydocs/images/image3.jpg"
        delete     "/home/user2/mydocs/images"
)
```

&nbsp;
___
&nbsp;

## upload
Aplica move_uploaded_file a los multiples archivos encontrados en \$_FILES y retorna un reporte  

**[array]** =  *public* function ( *mixed* \$mDestine, *int* \$nLimit );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mDestine**|mixed||Ruta de destino o array asociativo de las mismas, donde cada índice será el mismo que en el array \$_FILES|
|**\$nLimit**|int|null|Tamaño máximo soportado para los archivos. Si no se especifica se aplicará el valor de **nglFiles::maxUploadSize**|

&nbsp;
___
&nbsp;

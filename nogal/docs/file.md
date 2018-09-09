# nogal::file
Crea un objeto sobre archivos y/o directorio
  
## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**content**|string||Contenido del archivo|
|**curl_options**|array||Opciones CURL para este tipo de conexiones, [curl-setopt](http://php.net/manual/en/function.curl-setopt.php)|
|**downname**|string|*attr::basename*|Nombre que se asignará al archivo en caso de download|
|**filepath**|string||Ruta del archivo o directorio|
|**length**|int|null|Tamaño máximo en bytes del archivo contemplado para lectura o escritura|
|**mimetype**|string|text/plain|Formato de archivo|
|**outtype**|string|null|Formato de salida para el método view|
|**reload**|boolean|true|Determina si se aplicará nuevamente el método [load](#load) sobre el archivo para actualizar la información|
|**sandbox**|string|const *NGL_PATH_PROJECT*|Directorio de confinamiento para el accionar de los métodos|

## Atributos
|Atributo|Tipo|Descripción|
|---|---|---|
|**basename**|string|Clase a la que pertenece el objeto|
|**bytes**|int|Tamaño del archivo en bytes|
|**chmod**|mixed|Permisos|
|**date**|string|Fecha y hora en formato Y-m-d H:i:s|
|**extension**|string|Extensión del archivo|
|**filename**|string|Nombre del archivo/directorio, sin path ni extensión|
|**image**|boolean|TRUE o FALSE|
|**info**|array|Array que contiene los datos de todos los otros atributos|
|**mime**|string|Mime type|
|**path**|string|Path completo|
|**protocol**|string|Protocolo: filesystem, ftp, ftps, http o https|
|**query**|string|Query String, en el caso de las URLs|
|**size**|string|Tamaño del archivo en la mayor unidad de medida|
|**timestamp**|int|Fecha UNIX|
|**type**|string|file o dir|
  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[append](#append)|Añade el contenido content al final de un archivo. Si el archivo no existe lo cr...|
|[download](#download)|Genera la descarga del archivo hacia el cliente.|
|[fileinfo](#fileinfo)|Retorna al atributo info.|
|[fill](#fill)|Rellena el archivo con content hasta length|
|[load](#load)|Carga el archivo/directorio $sFilePath en el objeto.Si $sFilePath es igual a TRU...|
|[read](#read)|Lee y retorna el contenido del archivo cargado|
|[reload](#reload)|Actualiza la información del objeto releyendo el archivo/directorio de origen y ...|
|[view](#view)|Genera una salida del contenido del archivo con el formato correspondiente a la ...|
|[write](#write)|Escribe/reemplaza el contenido del archivo con content. Si el archivo no existe ...|
|Internos||
|[WriteContent](#WriteContent)|Escribe contenido en un archivo. Este método es utilizado por append y write|
  
&nbsp;

## append
> Añade el contenido **content** al final de un archivo. Si el archivo no existe lo crea  
> Este método es unicamente válido cuando se trata de un archivo local. Ver tambien [write](#write)

**[$this]** =  *public* function ( *string* $sContent, *boolean* $bReload );
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sContent**|string|*arg::content*|Contenido del archivo|
|**$bReload**|boolean|*arg::reload*|Determina si se aplicará nuevamente el método [load](#load) sobre el archivo para actualizar la información|
  
### Ejemplos  
#### Escritura  
```php
$ngl("file")->load("readme.txt")->append("hola mundo!");
```

&nbsp;
___
&nbsp;

## download
> Genera la descarga del archivo hacia el cliente.  

**[string]** =  *public* function ( *string* $sDownName, *string* $sMimeType );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sDownName**|string|*arg::downname*|Contenido del archivo|
|**$sMimeType**|boolean|*arg::mimetype*|Formato de archivo. Si no se especifica uno diferente, se utilizará el informado por [load](#load)|

### Ejemplos  
#### Descarga directa  
```php
$ngl("file")->load("readme.txt")->download();
```

&nbsp;
___
&nbsp;

## fileinfo
> Retorna al atributo **info**, que contiene toda la información del archivo  

**[array]** =  *public* function ( );
  
### Ejemplos  
#### Información del archivo  
```php
$info = $ngl("file")->load("readme.txt")->fileinfo();
print_r($info);

#salida
Array (
    [type] => file
    [basename] => manifest.json
    [extension] => json
    [filename] => manifest
    [protocol] => filesystem
    [path] => /var/www/html/manifest.json
    [bytes] => 1153
    [size] => 1.13KB
    [chmod] => 0755
    [timestamp] => 1527605517
    [date] => 2018-05-29 11:51:57
    [query] => 
    [mime] => application/json
    [image] => 
)
```

&nbsp;
___
&nbsp;

## fill
> Rellena el archivo con **content** hasta **length**  
> Puede ser utilizado para reservar espacio en el disco para un futuro archivo

**[$this]** =  *public* function ( *string* $sContent, *int* $nLength );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sContent**|string|*arg::content*|Contenido del archivo|
|**$nLength**|int|*arg::length*|Tamaño máximo en bytes del archivo contemplado para lectura o escritura|

### Ejemplos  
#### Archivo de 100kb
```php
$ngl("file")->load("zeros.txt")->fill("0", 100000);
```

&nbsp;
___
&nbsp;

## load
> Carga el archivo/directorio *$sFilePath* en el objeto  

**[$this]** =  *public* function ( *string* \$sFilePath );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sFilePath**|string|*arg::filepath*|Ruta del archivo o directorio.<ul><li>Si la ruta comienza con un *DIRECTORY_SEPARATOR *, dos puntos .. ó un punto seguido de un *DIRECTORY_SEPARATOR* la ruta será comprendida dentro de *arg::sandbox*</li><li>Si la ruta compienza con cualquier otro caracter, será comprendida dentro del directorio **NGL_PATH_PUBLIC**</li><li>Si hace referencia a un archivo inexistente, **load** intentará crearlo</li><li>Si es exactamente igual a TRUE se trabajará con un archivo temporal en el directorio temporal del sistema</li></ul>|

&nbsp;
___
&nbsp;

## read
Lee y retorna el contenido del archivo cargado  

**[string]** =  *public* function ( *int* \$nLength, *array* \$aCurlOptions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nLength**|int|null|Tamaño máximo en bytes del archivo contemplado para lectura o escritura|
|**\$aCurlOptions**|array||Opciones CURL para este tipo de conexiones, <a href="http://php.net/manual/en/function.curl-setopt.php" target="_blank">http://php.net/manual/en/function.curl-setopt.php</a>|
### Ejemplos  
#### Lectura local  
```php
$ngl("file.foo")->load("readme.txt");
echo $ngl("file.foo")->read();
echo $ngl("file.foo")->read(10);

#salida
"esto es una prueba"
"esto es un"
```
#### Lectura de API usando CURL  
```php
$ngl("file.foo")->load("http://api.sumaprop.com/propiedad/search/?max_per_page=3&estad os_publicacion=1&tipo_operacion=1&order=direccion­asc");
$options = array(
    "CURLOPT_CUSTOMREQUEST" => "GET",
    "CURLOPT_HTTPHEADER" => array("x­authtoken: 4d186321c1a7f0f354b297e8914ab240")
);
echo $ngl("file.foo")->read(null, $options);
```
#### Solicitud con variables POST  
```php

```

&nbsp;
___
&nbsp;

## reload
Actualiza la información del objeto releyendo el archivo/directorio de origen y vuelve a cargarlo  

**[$this]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## view
Genera una salida del contenido del archivo con el formato correspondiente a la extension especificada  

**[string]** =  *public* function ( *string* \$sExtension );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sExtension**|string|argument:outtype|Extensión especificada para el formato de salida|

&nbsp;
___
&nbsp;

## write
Escribe/reemplaza el contenido del archivo con **content**. Si el archivo no existe lo crea  

**[$this]** =  *public* function ( *string* \$sContent );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sContent**|string||Contenido del archivo|
### Ejemplos  
#### Escritura  
```php
$ngl("file.foo")->load("readme.txt");
$ngl("file.foo")->write("hola mundo!");
```

&nbsp;
___
&nbsp;

## WriteContent
Escribe contenido en un archivo. Este método es utilizado por **append** y **write**  

**[$this]** =  *protected* function ( *string* \$sFilePath, *string* \$sContent, *boolean* \$bReload, *string* \$sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFilePath**|string||Path del archivo de destino|
|**\$sContent**|string||Contenido a escribir|
|**\$bReload**|boolean|true|Determina si se aplicará el método nglFile::reload sobre el archivo para actualizar la información.
Se recomienda usar **false** cuando se realicen sucesivos nglFile::append|
|**\$sMode**|string|wb|Modo de escritura (según fopen)|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
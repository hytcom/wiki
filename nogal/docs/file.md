# file
Crea un objeto sobre un archivo ó directorio permitiendo acceder a sus propiedades, leer y escribir su contenido.
Este objeto está alcanzado por la directiva **SANDBOX**, que es el entorno dentro del cual se establecerá el dominio del objeto, es decir, establece el directorio dentro del cual estará contenido el accionar del método, cualquier intento de leer o escribir un archivo fuera de este entorno resultará en error. Por seguridad, **SANDBOX** toma el valor de la constante [NGL_SANDBOX](constants.md#opcionales) y en caso de que ella no estuviese definida asumirá el valor de [NGL_PATH_PROJECT](constants.md#rutas)
___

  
## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**content**|string||Contenido del archivo|
|**curl_options**|array||Opciones CURL para este tipo de conexiones, [curl-setopt](http://php.net/manual/en/function.curl-setopt.php)|
|**downname**|string|*attr::basename*|Nombre que se asignará al archivo en caso de download|
|**filepath**|string||Ruta del archivo o directorio|
|**length**|int|null|Tamaño máximo en bytes del archivo contemplado para lectura o escritura|
|**mimetype**|string|text/plain|Formato del archivo|
|**outtype**|string|null|Formato de salida para el método view|
|**reload**|boolean|true|Determina si se aplicará nuevamente el método [load](#load) sobre el archivo para actualizar la información|

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
> Carga el archivo/directorio **$sFilePath** en el objeto  
> En este método juega un papel importante el valor

**[$this]** =  *public* function ( *string* $sFilePath);  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sFilePath**|string|*arg::filepath*|Ruta del archivo o directorio.<ul><li>Si la ruta comienza con un **DIRECTORY_SEPARATOR**, dos puntos .. ó un punto seguido de un **DIRECTORY_SEPARATOR** la ruta será comprendida dentro de **SANDBOX**</li><li>Si la ruta compienza con cualquier otro caracter, será comprendida dentro del directorio [NGL_PATH_PUBLIC](constants.md#rutas)</li><li>Si hace referencia a un archivo inexistente, **load** intentará crearlo</li><li>Si es exactamente igual a TRUE se trabajará con un archivo temporal en el directorio temporal del sistema</li></ul>|

### Ejemplos  
#### Carga de archivo local público
```php
$ngl("file")->load("test.txt");
```

#### Carga de archivo local en SANDBOX
```php
$ngl("file")->load("/somedir/test.txt");
```

#### Archivo WEB
```php
$ngl("file")->load("http://cdn.bithive.cloud/json/material-design-colors.json");
```

&nbsp;
___
&nbsp;

## read
> Lee y retorna el contenido del archivo cargado  
> Si se especifica un valor para **$nLength**, se leerán los primeros **$nLength** caracteres del archivo  
> Para el caso de conexiones CURL, las opciones podrán ser pasada por medio de un segundo argumento

**[string]** =  *public* function ( *int* $nLength, *array* $aCurlOptions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$nLength**|int|*arg::length*|Tamaño máximo en bytes del archivo contemplado para lectura o escritura|
|**$aCurlOptions**|array|*arg::curl_options*|Opciones CURL para este tipo de conexiones, [curl-setopt](http://php.net/manual/en/function.curl-setopt.php)|

### Ejemplos  
#### Lectura local  
```php
$file = $ngl("file")->load("readme.txt");
echo $file->read()."\n";
echo $file->read(10);

#salida
"esto es una prueba"
"esto es un"
```

#### Lectura de API usando CURL  
```php
$curl = $ngl("file.foo")->load("http://api.sumaprop.com/propiedad/search/?max_per_page=3&estad os_publicacion=1&tipo_operacion=1&order=direccion­asc");
$options = array(
    "CURLOPT_CUSTOMREQUEST" => "GET",
    "CURLOPT_HTTPHEADER" => array("x­authtoken: 4d186321c1a7f0f354b297e8914ab240")
);
echo $curl->read(null, $options);
```

#### Solicitud con variables POST  
```php
$post = $ngl("file")->load("http://domain.com/contact.php");
$options = array(
    "CURLOPT_CUSTOMREQUEST" => "POST",
    "CURLOPT_POST" => 1,
    "CURLOPT_SAFE_UPLOAD" => false, // a partir de PHP 5.6.0
    "CURLOPT_POSTFIELDS" => array(
        "name" => "John Smith",
        "email" => "jsmith@foobar.com",
        "message" => "Lorem ipsum dolor sit amet, consectetur adipiscing elit."
    )
);

echo $post->read(null, $options);
```

&nbsp;
___
&nbsp;

## reload
> Actualiza la información del objeto releyendo el archivo/directorio de origen y vuelve a cargarlo  

**[$this]** =  *public* function ( );

&nbsp;
___
&nbsp;

## view
> Genera una salida del contenido del archivo con el formato correspondiente a la extension especificada  
> Si no se especifica una extensión el contenido será mostrado con el formato detectado por [load](#load)

**[string]** =  *public* function ( *string* \$sExtension );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sExtension**|string|*arg:outtype*|Extensión especificada para el formato de salida|

&nbsp;
___
&nbsp;

## write
> Escribe/reemplaza el contenido del archivo con **$sContent**. Si el archivo no existe lo crea.

**[$this]** =  *public* function ( *string* \$sContent );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sContent**|string|*arg::content*|Contenido del archivo|

### Ejemplos  
#### Escritura  
```php
$ngl("file")->load("test.txt")->write("hola mundo!");
```

&nbsp;
___
&nbsp;

# Internos
## WriteContent
> Escribe contenido en un archivo. Este método es utilizado por [append](#append) y [write](#write)

**[$this]** =  *protected* function ( *string* $sFilePath, *string* $sContent, *boolean* $bReload, *string* $sMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sFilePath**|string||Path del archivo de destino|
|**$sContent**|string||Contenido a escribir|
|**$bReload**|boolean|true|Determina si se aplicará el método nglFile::reload sobre el archivo para actualizar la información. Para una mejor performance se recomienda usar **false** cuando se realicen sucesivos [append](#append|
|**$sMode**|string|wb|Modo de escritura (según fopen)|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2019 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
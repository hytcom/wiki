# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# file
## nglFile *extends* nglStd [instanciable] [20160201]
Crea un objeto sobre archivos y/o directorio.
  
## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**content**|string||Contenido del archivo|
|**curl_options**|array||Opciones CURL para este tipo de conexiones, <a href="http://php.net/manual/en/function.curl-setopt.php" target="_blank">http://php.net/manual/en/function.curl-setopt.php</a>|
|**downname**|string|attribute::basename|Nombre que se asignará al archivo en caso de download|
|**filepath**|string||Ruta del archivo o directorio|
|**length**|int|null|Tamaño máximo en bytes del archivo contemplado para lectura o escritura|
|**outtype**|string||Formato de salida para el método view|
|**reload**|boolean||Determina si se aplicará nuevamente el método nglFile::load sobre el archivo para actualizar la información|

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
|[WriteContent](#WriteContent)|Escribe contenido en un archivo. Este método es utilizado por append y write|
|[append](#append)|Añade el contenido content al final de un archivo. Si el archivo no existe lo crea|
|[download](#download)|Genera la descarga del archivo hacia el cliente.|
|[fileinfo](#fileinfo)|Retorna al atributo info.|
|[fill](#fill)|Rellena el archivo con content hasta length|
|[load](#load)|Carga el archivo/directorio $sFilePath en el objeto.Si $sFilePath es igual a TRUE se trabajará con un archivo temporal en el directorio temporal del ...|
|[read](#read)|Lee y retorna el contenido del archivo cargado|
|[reload](#reload)|Actualiza la información del objeto releyendo el archivo/directorio de origen y vuelve a cargarlo|
|[view](#view)|Genera una salida del contenido del archivo con el formato correspondiente a la extension especificada|
|[write](#write)|Escribe/reemplaza el contenido del archivo con content. Si el archivo no existe lo crea|

  
&nbsp;


## append
Añade el contenido **content** al final de un archivo. Si el archivo no existe lo crea  

**[$this]** =  *public* function ( );
  
### Ejemplos  
#### Escritura  
```php
$ngl("file.foo")->load("readme.txt");
$ngl("file.foo")->append("hola mundo!");
```

&nbsp;
___
&nbsp;

## download
Genera la descarga del archivo hacia el cliente.  

**[string]** =  *public* function ( *string* \$sDownName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sDownName**|string|argument:downname||

&nbsp;
___
&nbsp;

## fileinfo
Retorna al atributo **info**.  

**[array]** =  *public* function ( );
  
### Ejemplos  
#### Información del archivo  
```php
$ngl("file.foo")->load("readme.txt");
print_r($ngl("file.foo")->fileinfo());

#salida
Array (
    [type] => file
    [basename] => readme.txt
    [extension] => txt
    [filename] => readme
    [protocol] => filesystem
    [path] => tmpeadme.txt
    [bytes] => 4448
    [size] => 4.34KB
    [chmod] => 0666
    [timestamp] => 1425693706
    [date] => 2015-03-06 23:01:46
    [mime] => text/plain
    [image] => 
)
```

&nbsp;
___
&nbsp;

## fill
Rellena el archivo con **content** hasta **length**  

**[$this]** =  *public* function ( *string* \$sContent, *string* \$nLength );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sContent**|string|argument:content||
|**\$nLength**|string|argument:length||

&nbsp;
___
&nbsp;

## load
Carga el archivo/directorio \$sFilePath en el objeto.
Si \$sFilePath es igual a TRUE se trabajará con un archivo temporal en el directorio temporal del sistema  

**[$this]** =  *public* function ( *string* \$sFilePath );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFilePath**|string|argument:filepath||

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
&nbsp;

# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# ftp
## nglFTP *extends* nglStd [instanciable] [20160201]
Gestor de conexiones FTP.

Características Principales:
- listar, crear, renombrar y eliminar archivos y directorios
- descargar archivos y directorios completos al servidor local
- subir archivos y directorios al servidor remoto
  
## Variables
`private` $ftp = FTP resource  
`private` $sSlash = Barra separadora de directorios  
`private` $aLastDir = Array de registro del anidamiento de directorios en el método nglFTP::delete  

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**filepath**|string|null|Nombre del archivo o directorio activo en el servidor remoto|
|**force_create**|boolean|false|Fuerza la creación de directorios en los métodos CD y MAKEDIR|
|**host**|string|127.0.0.1|IP o dominio del servidor remoto|
|**local**|string|null|Nombre del archivo o directorio en el servidor local|
|**ls_mode**|string|single|Modo en el que se ejecutará el método LS:<br /><ul><li>**single** =  array con los paths completos de los archivos y directorios listados</li><li>**signed** =  idem anterior pero con un * antepuesto cuando el path corresponda a un directorio</li><li>**info** =  información detallada de los archivos y directorios listados, sujeto a la disponibilidad del dato<ul><li>**basename** =  nombre del archivo</li><li>**bytes** =  tamaño en bytes</li><li>**chmod** =  permisos</li><li>**date** =  fecha en formato Y-m-d H:i:s</li><li>**extension** =  extensión del archivo</li><li>**filename** =  nombre del archivo sin extensión</li><li>**image** =  true o false</li><li>**path** =  path completo desde \$sPath</li><li>**protocol** =  protocolo del archivo</li><li>**size** =  tamaño en la unidad de medida mas grande</li><li>**timestamp** =  fecha UNIX</li><li>**type** =  file o dir</li></ul></li></ul>|
|**mask**|string||Regex utilizada para filtrar el resultado del método LS|
|**newname**|string|null|Nombre de archivo o directorio para el método REN|
|**pass**|string||Contraseña|
|**passive_mode**|boolean|true|Establese la conexión en modo pasivo|
|**port**|int|21|Puerto del servidor remoto|
|**recursive**|boolean|false|Ejecuta LS en modo recursivo|
|**transfer**|int|FTP_BINARY|Establese el modo de transferencia de archivos: FTP_BINARY ó FTP_ASCII|
|**user**|string|anonymous|Nombre de usuario|

## Atributos
|Atributo|Tipo|Descripción|
|---|---|---|
|**list**|string|Contiene el último listodo de directorios generado por LS|
|**log**|string|Log de la comunicación con el servidor|
|**system**|string|Tipo de sistema operativo del servidor remoto|
|**tree**|array|Contiene el último árbol de directorios generado por TREE|
|**windows**|string|Indica si el sistema operativo es windows|

  
&nbsp;

# Métodos
- [DownloadTree](#DownloadTree)
- [GetChmod](#GetChmod)
- [GetTimestamp](#GetTimestamp)
- [Logger](#Logger)
- [UploadTree](#UploadTree)
- [cd](#cd)
- [connect](#connect)
- [curdir](#curdir)
- [delete](#delete)
- [download](#download)
- [login](#login)
- [ls](#ls)
- [mkdir](#mkdir)
- [passive](#passive)
- [rename](#rename)
- [upload](#upload)

  
&nbsp;


## cd
Cambia de directorio en el servidor remoto  

**[array]** =  *public* function ( *string* \$sPath, *boolean* \$bForce );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string|null|Nombre del archivo o directorio activo en el servidor remoto|
|**\$bForce**|boolean|false|Fuerza la creación de directorios en los métodos CD y MAKEDIR|
&nbsp;
___
&nbsp;

## connect
Establece la conexión con el servidor remoto  

**[boolean]** =  *public* function ( *string* \$sHost, *int* \$nPort );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sHost**|string|127.0.0.1|IP o dominio del servidor remoto|
|**\$nPort**|int|21|Puerto del servidor remoto|
&nbsp;
___
&nbsp;

## curdir
Retorna la ruta del directorio actual  

**[string]** =  *public* function ( );
  
&nbsp;
___
&nbsp;

## delete
Elimina un archivo o directorio  

**[boolean]** =  *public* function ( *string* \$sPath );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string|null|Nombre del archivo o directorio activo en el servidor remoto|
&nbsp;
___
&nbsp;

## download
descarga un archivo o directorio del servidor a la maquina local  

**[boolean]** =  *public* function ( *string* \$sPath, *string* \$sLocalPath, *int* \$nTransfer );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string|null|Nombre del archivo o directorio activo en el servidor remoto|
|**\$sLocalPath**|string|null|Nombre del archivo o directorio en el servidor local|
|**\$nTransfer**|int|FTP_BINARY|Establese el modo de transferencia de archivos: FTP_BINARY ó FTP_ASCII|
&nbsp;
___
&nbsp;

## DownloadTree
Método auxiliar de DOWNLOAD ejecutado por medio del método nglCommon::treeWalk  

**[null]** =  *private* function ( *string* \$vFile, *int* \$nTransfer );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vFile**|string||Array con los datos del nombre del archivo o directorio|
|**\$nTransfer**|int|FTP_BINARY|Modo de transferencia de archivos, constantes FTP_BINARY ó FTP_ASCII|
&nbsp;
___
&nbsp;

## GetChmod
Convierte una cadena de permisos RWX en un valor CHMOD  

**[number]** =  *private* function ( *string* \$sCHMOD );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCHMOD**|string||Cadena de permisos RWX|
&nbsp;
___
&nbsp;

## GetTimestamp
Convierte las fechas de ftp_rawlist en un valor timestamp  

 *private* function ( );
  
&nbsp;
___
&nbsp;

## login
Autentica la sesion en el servidor remoto  

**[$this]** =  *public* function ( *string* \$sUser, *string* \$sPass, *boolean* \$bPassive );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sUser**|string|anonymous|Nombre de usuario|
|**\$sPass**|string||Contraseña|
|**\$bPassive**|boolean|argument::passive||
&nbsp;
___
&nbsp;

## Logger
Registra una cadena en el atributo log  

 *private* function ( *string* \$sLog );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sLog**|string||Cadena que se añadira al log|
&nbsp;
___
&nbsp;

## ls
Lista el contenido de un directorio. Sino se especifica un directorio listará el directorio actual  

**[array]** =  *public* function ( *string* \$sDirname, *string* \$sMask, *string* \$sMode, *boolean* \$bRecursive );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sDirname**|string|null|Nombre del archivo o directorio activo en el servidor remoto|
|**\$sMask**|string||Regex utilizada para filtrar el resultado del método LS|
|**\$sMode**|string|single|Modo en el que se ejecutará el método LS:<br /><ul><li>**single** =  array con los paths completos de los archivos y directorios listados</li><li>**signed** =  idem anterior pero con un * antepuesto cuando el path corresponda a un directorio</li><li>**info** =  información detallada de los archivos y directorios listados, sujeto a la disponibilidad del dato<ul><li>**basename** =  nombre del archivo</li><li>**bytes** =  tamaño en bytes</li><li>**chmod** =  permisos</li><li>**date** =  fecha en formato Y-m-d H:i:s</li><li>**extension** =  extensión del archivo</li><li>**filename** =  nombre del archivo sin extensión</li><li>**image** =  true o false</li><li>**path** =  path completo desde \$sPath</li><li>**protocol** =  protocolo del archivo</li><li>**size** =  tamaño en la unidad de medida mas grande</li><li>**timestamp** =  fecha UNIX</li><li>**type** =  file o dir</li></ul></li></ul>|
|**\$bRecursive**|boolean|false|Ejecuta LS en modo recursivo|
&nbsp;
___
&nbsp;

## mkdir
Crea un directorio.
Si el directorio ya existe y el argumento force_create es TRUE, mkdir le agregará al nombre del directorio el sufijo _N donde N es el número de directorio con el mismo nombre +1  

**[$this]** =  *public* function ( *string* \$sPath, *boolean* \$bForce );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string|null|Nombre del archivo o directorio activo en el servidor remoto|
|**\$bForce**|boolean|false|Fuerza la creación de directorios en los métodos CD y MAKEDIR|
&nbsp;
___
&nbsp;

## passive
Activa/desactiva el modo pasivo. Por defecto todas las conexiones se inician en modo pasivo  

**[$this]** =  *public* function ( *boolean* \$bPassive );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$bPassive**|boolean|true|Establese la conexión en modo pasivo|
&nbsp;
___
&nbsp;

## rename
Cambia el nombre de un archivo o directorio  

**[boolean]** =  *public* function ( *string* \$sPath, *string* \$sNewName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string|null|Nombre del archivo o directorio activo en el servidor remoto|
|**\$sNewName**|string|null|Nombre de archivo o directorio para el método REN|
&nbsp;
___
&nbsp;

## upload
Sube un archivo o directorio al servidor remoto  

**[boolean]** =  *public* function ( *string* \$sLocalPath, *string* \$sPath, *int* \$nTransfer );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sLocalPath**|string|null|Nombre del archivo o directorio en el servidor local|
|**\$sPath**|string|null|Nombre del archivo o directorio activo en el servidor remoto|
|**\$nTransfer**|int|FTP_BINARY|Establese el modo de transferencia de archivos: FTP_BINARY ó FTP_ASCII|
&nbsp;
___
&nbsp;

## UploadTree
método auxiliar de UPLOAD ejecutado por medio del método treeWalk  

**[boolean]** =  *private* function ( *array* \$vFile, *int* \$nTransfer );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vFile**|array||Array con los datos del nombre del archivo o directorio|
|**\$nTransfer**|int|FTP_ASCII|Modo de transferencia de archivos FTP_BINARY ó FTP_ASCII|
&nbsp;
___
&nbsp;

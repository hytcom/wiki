# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# files
## nglFiles *extends* nglStd [main] [20160201]
Métodos frecuentes para el manejo de archivos y directorios.
  
  
&nbsp;

# Métodos
- [RebuildFILES](#RebuildFILES)
- [absPath](#absPath)
- [basePaths](#basePaths)
- [copyr](#copyr)
- [ls](#ls)
- [lsprint](#lsprint)
- [maxUploadSize](#maxUploadSize)
- [mkdirr](#mkdirr)
- [unlinkr](#unlinkr)
- [upload](#upload)

  
&nbsp;


## absPath
formatea un path como absoluto limpiando doble barras o referencias atras (..)  

**[string]** =  *public* function ( *string* \$sPath, *string* \$sSlash );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sPath**|string||Path|
|**\$sSlash**|string|NGL_DIR_SLASH|Separador de directorios.|

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

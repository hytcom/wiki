# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# sess
## nglSession *extends* nglTrunk [main] [20140621]
Gesiona el almacenamiento y recuperación de información asociada con una sesión o varias sesiones.
Esta clase permite gestionar las sesiones en base de datos o de manera nativa.

nglSession construye el objeto \$session dentro de NOGAL, el cual es accedido a través de: **\$ngl("sess")->NOMBRE_DEL_METODO()**

Para sessiones en bases de datos
DROP TABLE IF EXISTS `__ngl_sessions__`;
CREATE TABLE `__ngl_sessions__` (
`id` VARCHAR(32) NOT NULL DEFAULT '',
`expire` INT(11) NOT NULL DEFAULT '0',
`persistent` ENUM('0', '1') NOT NULL DEFAULT '0',
`data` BLOB NOT NULL,
PRIMARY KEY (`id`) 
);
CREATE INDEX `expire_idx` ON `__ngl_sessions__` (`expire` DESC);
CREATE INDEX `persistent_idx` ON `__ngl_sessions__` (`persistent`);
  
## Variables
`private` $sMode = 
			Modo en el que trabajará el objeto:
			
			<ul>
				<li><b>db:</b> configura las sesiones en base de datos</li>
				<li><b>fs:</b> modo filesystem, los archivos de sesión se almacenarán en <b>NGL_PATH_SESSIONS</b></li>
				<li><b>php:</b> modo nativo de PHP</li>
			</ul>
		  
`private` $db = Controlador de base de datos  
`private` $sPath = Ruta en la que se guardarán las sesiones cuando el modo sea <b>fs</b>. Por deeccto   
`int` $nLifeTime = Tiempo máximo de vida de una sesión  

  
&nbsp;

# Métodos
- [GetPersistent](#GetPersistent)
- [close](#close)
- [count](#count)
- [destroy](#destroy)
- [destroyAll](#destroyAll)
- [gc](#gc)
- [id](#id)
- [open](#open)
- [persistent](#persistent)
- [read](#read)
- [showSessions](#showSessions)
- [start](#start)
- [write](#write)

  
&nbsp;


## gc
Elimina las sesiones, no persistentes, cuyo tiempo de vida supere el establecido por la variable PHP **session.gc_maxlifetime**  

**[boolean]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## close
Controlador requerido por PHP para el cierre de las sesiones.
Este método es requerido por **PHP** pero carece de utilidad dentro del objeto.  

**[true]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## count
Retorna el número de sesiones activas. Disponible cuando el modo de almacenamiento no sea **php**  

**[int]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## destroy
Llamada de retorno ejecutada cuando una sesión es destruida  

**[boolean]** =  *public* function ( *string* \$SID );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$SID**|string|sesion activa|ID de sesion a destruir|

&nbsp;
___
&nbsp;

## destroyAll
Destruye todas las sesiones, persistentes o no. Disponible cuando el modo de almacenamiento no sea **php**  

**[void]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## GetPersistent
Chequea si la sesion **\$SID** es o no una sesion persistente  

**[boolean]** =  *private* function ( *string* \$SID );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$SID**|string||ID de sesion a chequear|

&nbsp;
___
&nbsp;

## id
Retorna el ID de la sesion activa  

**[string]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## open
Llamada de retorno que se ejecutada cuando la sesión está siendo abierta.
Este método es requerido por **PHP** pero carece de utilidad dentro del objeto.  

**[boolean]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## persistent
Chequea si la sesion **\$SID** es o no una sesion persistente  

**[boolean]** =  *public* function ( *boolean* \$bPersistent, *string* \$SID );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$bPersistent**|boolean|sesion activa|Indica si la sesion **\$SID** es una sesion persistente|
|**\$SID**|string|sesion activa|ID de sesion a chequear|

&nbsp;
___
&nbsp;

## read
Retorna el contenidos de la sesion en forma de cadena serializada  

**[boolean]** =  *public* function ( *string* \$SID );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$SID**|string|sesion activa|ID de sesion a leer|

&nbsp;
___
&nbsp;

## showSessions
Retorna listado completo de las sesiones activas. 
Cuando el objeto este configurado en modo **fs** retornara el listado de archivos de sesion.  

**[array]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## start
Da inicio al objeto. Configura el modo de sesión y el tiempo máximo de vida de las mismas  

**[void]** =  *public* function ( *mixed* \$handler, *int* \$nLifeTime );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$handler**|mixed|null|Determina el modo en el cual trabajarán las sesiones:<ul><li>**objecto de DB** =  configura las sesiones en base de datos</li><li>**string fs** =  modo filesystem, los archivos de sesión se almacenarán en **NGL_PATH_SESSIONS**</li><li>**null** =  modo nativo de PHP</li></ul>|
|**\$nLifeTime**|int|session.gc_maxlifetime|Tiempo máximo de vida de una sesión|

&nbsp;
___
&nbsp;

## write
Guarda los datos de la variable superglobal **\$_SESSION** como contenido de la sesión **\$SID**  

**[boolean]** =  *public* function ( *string* \$SID );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$SID**|string||ID de sesion a chequear|

&nbsp;
___
&nbsp;

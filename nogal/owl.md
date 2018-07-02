# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# owl
## nglOwl *extends* nglStd [instanciable] [%REVISION%]
Owl es el ORM de NOGAL y permite ejecutar operaciones sobre distintos objetos de base de datos.
Entre las funciones del objecto se encuentran:<ul><li>consulta de listados de registros directos y con referencias cruzadas</li><li>consulta de un registro en particular</li><li>administración de objetos depentientes, como por ejemplo los datos una empresa y sus empleados</li><li>uso de foreignkeys a nivel objeto, sin importar el driver de base de datos</li><li>validación de datos por medio del objeto **nglValidate**</li><li>permite añadir, modificar, suspender y eliminar (eliminado lógico) registros</li><li>eliminación de registros en cascada</li></ul>Por todo ello, nglOwl trabaja con estructura de base de datos determinada.
Para mas inforamción consultar owl_setup.txt
  
## Variables
`private` $db = Objeto de base de datos  
`private` $sObject = Nombre del Objeto activo  
`private` $vObjects = Objetos cargados durante la ejecución  
`private` $sChildTable = Nombre de la tabla del objeto dependiente activo  
`private` $bChildMode = Establece el modo Child para los métodos de escritura  
`private` $bInternalCall = Señala que llamada al método es interna. Usada para evitar el log  
`private` $aCascade = Almacena las sentencias SQL para el borrado logico en cascada del último CrossRows  
`private` $aTmpChildren = Almacena temporalmente los hijos durante nglOwl::select  

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**alvin**|boolean|false|Activa y desactiva los chequeos ALVIN|
|**cascade**|boolean|false|Activa y desactiva el borrado en cascada|
|**child**|string|null|Nombre del Objeto dependiente activo|
|**data**|mixed|array|Objeto o array asociativo con los nombres de las columnas y datos que se usará en los métodos de escritura. Este argumento no es válido para escrituras múltiples|
|**duplicate_children**|boolean|false|Activa y desactiva la copia de registros dependientes en el método **nglOwl::duplicate**|
|**escape**|boolean|null|Determina si **data** debe o no ser tratado por el método **escape** del objeto **\$db**|
|**filter**|string|null|Porción de la código SQL o **JSQL** que complementa al generado por el método **nglOwl::view** y es utilizado para filtrar los resultados del método **nglOwl::getAll**|
|**id**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**inherit**|boolean|true|Determina las suspenciones deben propagarse a las tablas dependientes|
|**insert_mode**|string|INSERT|Modo del método nglOwl::insert. Valores admitidos:<ul><li>**INSERT** =  inserta nuevos registros</li><li>**REPLACE** =  si el nuevo registro duplica un valor PRIMARY KEY o UNIQUE, el antiguo registro es eliminado</li><li>**IGNORE** =  el comando no aborta incluso si ocurren errores durante la ejecución</li></ul>|
|**jsql**|string|null|Sentencia JSQL para ser ejecutada utilizando el método **nglOwl::query**|
|**owlog**|boolean|true|Activa y desactiva el log en la tabla __ngl_owl_log__|
|**owlog_changelog**|boolean|false|Activa y desactiva el uso del changelog en la tabla __ngl_owl_log__|
|**object**|string|null|Nombre del Objeto que se establecerá como activo|
|**use_history**|boolean|false|Activa y desactiva el uso del atributo **history**|
|**view_alias**|string|auto|Política utilizada para nombrar los alias en el método **nglOwl::view**, se antepondrá el nombre de la tabla cuando:<ul><li>**all** =  en todos los campos de todas las tablas</li><li>**joins** =  en todos los campos, salvo en los de la tabla principal</li><li>**auto** =  sólo los campos que tengan un duplicado</li><li>**none** =  ningun campo</li></ul>|
|**view_children**|mixed|false|Determina el tipo de unión con las tablas dependientes<ul><li>**true** =  todas las tablas</li><li>**false** =  ninguna tabla</li><li>**array** =  array con tablas seleccionadas</li></ul>|
|**view_columns**|string|null|Cadena JSQL con los nombres de las columnas que deberá retornar el método **nglOwl::view**.
Sintáxis: ["TABLE.COLUMN","ALIAS"] o "TABLE.COLUMN"
Ej: [["tabla.campo1","foo"], "alias2.campo2", ["campo3","bar"]]|
|**view_joins**|boolean|true|Activa y desactiva la unión con las tablas relacionadas (no dependientes) en el método **nglOwl::view**|
|**view_mode**|string|jsql|Modo de salida de datos en el método **nglOwl::view**:<ul><li>**jsql** =  formato JSON</li><li>**sql** =  formato ANSI SQL</li></ul>|

## Atributos
|Atributo|Tipo|Descripción|
|---|---|---|
|**current**|int|ID del registro activo|
|**history**|string|Almacena el historial de logs durante la ejecución|
|**log**|string|Almacena el log de la última acción ejecutada|
|**object_name**|string|Nombre del Objeto activo|
|**query**|string|Ultima consulta SQL ejecutada|
|**result**|string|Ultimo resultado SQL ejecutado por get o getall|
|**validate**|array|Resultado de la última validación por **nglValidate::validate**|

  
&nbsp;

# Métodos
- [CrossRows](#CrossRows)
- [DeleteInCascade](#DeleteInCascade)
- [GetID](#GetID)
- [Logger](#Logger)
- [OwLog](#OwLog)
- [UpdateData](#UpdateData)
- [Validate](#Validate)
- [child](#child)
- [children](#children)
- [close](#close)
- [columns](#columns)
- [connect](#connect)
- [delete](#delete)
- [describe](#describe)
- [duplicate](#duplicate)
- [get](#get)
- [getAll](#getAll)
- [insert](#insert)
- [query](#query)
- [relationship](#relationship)
- [select](#select)
- [showtables](#showtables)
- [suspend](#suspend)
- [toggle](#toggle)
- [unsuspend](#unsuspend)
- [update](#update)
- [view](#view)
- [viewChildren](#viewChildren)

  
&nbsp;


## child
Prepara el objeto para trabajar con la dependencia **\$sChild** y lo retorna  

**[$this]** =  *public* function ( *string* \$sChild );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sChild**|string|null|Nombre del Objeto dependiente activo|
&nbsp;
___
&nbsp;

## children
Lista las tablas dependientes de objeto activo  

**[array]** =  *public* function ( );
  
&nbsp;
___
&nbsp;

## close
Finaliza la conexión con la base de datos  

**[boolean]** =  *public* function ( );
  
&nbsp;
___
&nbsp;

## columns
Retorna los nombres de la columnas del objeto activo  

**[array]** =  *public* function ( );
  
&nbsp;
___
&nbsp;

## connect
Establece la conexión con la base de datos  

**[$this]** =  *public* function ( *object* \$driver );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$driver**|object||Objecto de Base de Datos|
&nbsp;
___
&nbsp;

## CrossRows
Verifica las referencias de la tabla **\$sTable** con el resto de las tablas del objeto y retorna un array informandolas  

**[array]** =  *private* function ( *string* \$sTable, *string* \$sWhere, *array* \$aConditions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sTable**|string||Nombre de la tabla principal|
|**\$sWhere**|string||Condición WHERE que determina el conjunto de resultados sobre el cual se analizará las referencias|
|**\$aConditions**|array||Array de condiciones FROM y WHERE que determina el conjunto de resultados en modo recursivo|
&nbsp;
___
&nbsp;

## delete
Utiliza el método **nglOwl::UpdateData** para intentar eliminar el o los registros seleccionados, ya sean del una tabla principal o de una dependiente (hijo).
Si el existiesen conflictos de **foreignkey** el borrado no se llevará a cabo y un informe de dependencias será almacenado en los atributos **log** y **history**.
En este último caso **delete** retornará **false**. De lo contrario devolverá el número de elementos borrados.
Si el argumento **cascade** estuviese indicado como **true** y hubiese conflictos de **foreignkey**, se omitirá el informe y el borrado se llevará a cabo de manera recursiva.  

**[int]** =  *public* function ( *mixed* \$aData1, *array* \$... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData1**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**\$...**|array||Lista variable de argumentos de tipo Array similares a **\$aData1**|
&nbsp;
___
&nbsp;

## DeleteInCascade
Ejecuta las sentencias de borrado \$aCascade y retorna el número de registros borrados  

**[int]** =  *private* function ( *mixed* \$aCascade );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aCascade**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
&nbsp;
___
&nbsp;

## describe
Detalles del objeto activo  

**[array]** =  *public* function ( *string* \$sObjectName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sObjectName**|string||Nombre de la tabla/objecto que se establecerá como activo|
&nbsp;
___
&nbsp;

## duplicate
Duplica un registro con o sin sus dependencias (hijos). Las diferentes metodologías de ejecución son:<ul><li>**principal CON dependencias** =  cuando **\$bChildren** = true</li><li>**principal SIN dependencias** =  cuando **\$bChildren** = false</li><li>**dependencia específica** =  cuando se ejecute por medio del método **nglOwl::child** se duplicará el registro dependiente con el id o imya **\$mID**</li><li>**todas las dependencias** =  cuando se ejecute por medio del método **nglOwl::child** y el valor **\$mID** no sea especificado</li></ul>Al finalizar se retornará el **ID** del registro duplicado; si se hubiese ejecutado una inserción multiple se retornará un array con todos los nuevos **IDs**.
Un informe del detalle de las duplicaciones será almacenado en los atributos **log** y **history**.
El **ID** de la última inserción se guardará en el atributo **id**.

**IMPORTANTE** para duplicar sólo dependencias se deberá seleccionar previamente un registro principal.  

**[mixed]** =  *public* function ( *mixed* \$mID, *boolean* \$bChildren );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mID**|mixed||Cadena IMYA o número entero|
|**\$bChildren**|boolean|duplicate_children|Activa y desactiva la copia de registros dependientes|
&nbsp;
___
&nbsp;

## get
Retorna un objeto **iNglDataObjet** con los datos de un registro y todas sus relaciones en base a su **ID** o **IMYA**  

**[object]** =  *public* function ( *mixed* \$mID, *string* \$sAliasMode, *boolean* \$bJoins, *mixed* \$mChildren );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mID**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**\$sAliasMode**|string|auto|Política utilizada para nombrar los alias en el método **nglOwl::view**, se antepondrá el nombre de la tabla cuando:<ul><li>**all** =  en todos los campos de todas las tablas</li><li>**joins** =  en todos los campos, salvo en los de la tabla principal</li><li>**auto** =  sólo los campos que tengan un duplicado</li><li>**none** =  ningun campo</li></ul>|
|**\$bJoins**|boolean|true|Activa y desactiva la unión con las tablas relacionadas (no dependientes) en el método **nglOwl::view**|
|**\$mChildren**|mixed|false|Determina el tipo de unión con las tablas dependientes<ul><li>**true** =  todas las tablas</li><li>**false** =  ninguna tabla</li><li>**array** =  array con tablas seleccionadas</li></ul>|
&nbsp;
___
&nbsp;

## getAll
Retorna un objeto **iNglDataObjet** con todos registros y relaciones en base a su **\$sFilter**  

**[object]** =  *public* function ( *string* \$sFilter, *string* \$sAliasMode, *boolean* \$bJoins, *mixed* \$mChildren, *string* \$sColumns );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sFilter**|string|null|Porción de la código SQL o **JSQL** que complementa al generado por el método **nglOwl::view** y es utilizado para filtrar los resultados del método **nglOwl::getAll**|
|**\$sAliasMode**|string|auto|Política utilizada para nombrar los alias en el método **nglOwl::view**, se antepondrá el nombre de la tabla cuando:<ul><li>**all** =  en todos los campos de todas las tablas</li><li>**joins** =  en todos los campos, salvo en los de la tabla principal</li><li>**auto** =  sólo los campos que tengan un duplicado</li><li>**none** =  ningun campo</li></ul>|
|**\$bJoins**|boolean|true|Activa y desactiva la unión con las tablas relacionadas (no dependientes) en el método **nglOwl::view**|
|**\$mChildren**|mixed|false|Determina el tipo de unión con las tablas dependientes<ul><li>**true** =  todas las tablas</li><li>**false** =  ninguna tabla</li><li>**array** =  array con tablas seleccionadas</li></ul>|
|**\$sColumns**|string|null|Cadena JSQL con los nombres de las columnas que deberá retornar el método **nglOwl::view**.
Sintáxis: ["TABLE.COLUMN","ALIAS"] o "TABLE.COLUMN"
Ej: [["tabla.campo1","foo"], "alias2.campo2", ["campo3","bar"]]|
&nbsp;
___
&nbsp;

## GetID
Obtiene el ID del registro **\$mID** de la tabla **\$sTable** confirmando la existencia del mismo, en base a su IMYA o al propio ID.
Cuando **\$sTable** sea distinto de **NULL** el valor retornado será almacenado en el atributo **id**  

**[none]** =  *private* function ( *mixed* \$mID, *string* \$sTable );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$mID**|mixed||Cadena IMYA o número entero|
|**\$sTable**|string|NULL|Nombre de la tabla en la cual buscar. Cuando el valor es **NULL** se trabajará sobre la tabla principal del objeto activo|
&nbsp;
___
&nbsp;

## insert
Inserta uno o mas registros en las tablas que componen los objetos. Este método también efectua las inserciones en las tablas dependientes (hijos).
Al finalizar se retornará el **ID** de la inseción; si se hubiese ejecutado una inserción multiple se retornará un array con todos los nuevos **IDs**.
Un informe del detalle de las inserciones será almacenado en los atributos **log** y **history**.
El **ID** de la última inserción se guardará en el atributo **current**.

**IMPORTANTE** los campos **id**, **imya** y **state** son administrados por el método, por lo que serán ignorados.  

**[mixed]** =  *public* function ( *array* \$aData1, *array* \$... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData1**|array||Array asociativo (o bidimensional de ellos) con los datos del registro a insertar|
|**\$...**|array||Lista variable de argumentos de tipo Array similares a **\$aData1**|
&nbsp;
___
&nbsp;

## Logger
Registra la salida de LOG de un método en los atributos **log** y **history**. Cuando **\$sStatus** = NULL se reseteará el valor del atributo **log**  

**[none]** =  *private* function ( *string* \$sStatus, *string* \$aDetails );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sStatus**|string|NULL|Definición del estado|
|**\$aDetails**|string|array|Detalles de la operación ejecutada|
&nbsp;
___
&nbsp;

## OwLog
Genera un log sobre cada acción de escritura. El mismo es almacenado en la base datos en la tabla `__ngl_owl_log__` y pasado al método **nglOwl::Logger**.
Los datos que conforman el log son:<ul><li>**table** =  nombre de la tabla afectada</li><li>**row** =  identificador del registro afectado</li><li>**user** =  id del usuario que ejecutó el cambio (valor **self::call("sysvar")->UID**)</li><li>**action** =  acción ejecutada: insert, update, suspend o delete</li><li>**date** =  fecha y hora en la que se ejecutó el cambio (formato **Y-m-d H:i:s**)</li><li>**ip** =  dirección IP desde la cual se ejecutó el cambio</li></ul>  

**[none]** =  *private* function ( *string* \$sTable, *int* \$nRow, *string* \$sAction );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sTable**|string||Nombre de la tabla afectada|
|**\$nRow**|int||Identificador del registro afectado|
|**\$sAction**|string||Acción ejecutada: insert, update, suspend o delete|
&nbsp;
___
&nbsp;

## query
Ejecuta una sentencia JSQL utilizando el método **query** del objeto **\$db** y retorna un objecto del tipo**iNglDBQuery**  

**[iNglDBQuery object]** =  *public* function ( *string* \$sJSQL, *array* \$aArgs );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sJSQL**|string|null|Sentencia JSQL para ser ejecutada utilizando el método **nglOwl::query**|
|**\$aArgs**|array|argument::jsql_args|Argumentos pasados al método, que serán traducidos a la sentencia utilizando vsprintf|
&nbsp;
___
&nbsp;

## relationship
Muestra la estructura relacional del objeto seleccionado  

**[string]** =  *public* function ( );
  
&nbsp;
___
&nbsp;

## select
Selecciona y establece como activo al objeto **\$sObjectName**  

**[object]** =  *public* function ( *string* \$sObjectName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sObjectName**|string||Nombre de la tabla/objecto que se establecerá como activo. Deberá respetar el patrón [a-zA-Z0-9_\-]+|
&nbsp;
___
&nbsp;

## showtables
Retorna un Array con los datos de todos los elementos que componen el sistema **DB**. Cada indice del Array está compuesto por:<ul><li>**name** =  String con el nombre de la tabla</li><li>**columns** =  Array con las columnas de la tabla</li><li>**foreignkey** =  True cuando la tabla contiene foreignkeys</li><li>**relationship** =  True cuando la tabla es ademas la tabla principal de un objeto **DB**</li></ul>  

**[array]** =  *public* function ( );
  
&nbsp;
___
&nbsp;

## suspend
Suspende uno o mas registros aplicando la misma lógica que el método **nglOwl::update**, con la diferencia de que sólo un campo es actualizado; **state = 0**
Cuando un registro principal es suspendido, todos sus registros dependientes también lo son.
Al finalizar se retornará el número de suspensiones ejecutadas exitosamente.
Un informe del detalle de las suspensiones será almacenado en los atributos **log** y **history**.
El **ID** de la última suspensión se guardará en el atributo **id**.  

**[int]** =  *public* function ( *mixed* \$aData1, *array* \$... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData1**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**\$...**|array||Lista variable de argumentos de tipo Array similares a **\$aData1**|
&nbsp;
___
&nbsp;

## toggle
Suspende y/o desuspende uno o mas registros aplicando la misma lógica que el método **nglOwl::update**, con la diferencia de que sólo un campo es actualizado; **state = 0**
Cuando un registro principal es suspendido, todos sus registros dependientes también lo son.
Al finalizar se retornará el número de suspensiones ejecutadas exitosamente.
Un informe del detalle de las suspensiones será almacenado en los atributos **log** y **history**.
El **ID** de la última suspensión se guardará en el atributo **id**.  

**[int]** =  *public* function ( *mixed* \$aData1, *array* \$... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData1**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**\$...**|array||Lista variable de argumentos de tipo Array similares a **\$aData1**|
&nbsp;
___
&nbsp;

## unsuspend
Reactiva registros suspendidos por **nglOwl::suspend**, actualizando **state = 1**
Cuando un registro principal es reactivado, todos sus registros dependientes también lo son.
Al finalizar se retornará el número de reactivaciones ejecutadas exitosamente.
Un informe del detalle de las reactivaciones será almacenado en los atributos **log** y **history**.
El **ID** de la última reactivación se guardará en el atributo **id**.  

**[int]** =  *public* function ( *mixed* \$aData1, *array* \$... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData1**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**\$...**|array||Lista variable de argumentos de tipo Array similares a **\$aData1**|
&nbsp;
___
&nbsp;

## update
Actualiza uno o mas registros en las tablas que componen los objetos. Este método también efectua las actualizaciones en las tablas dependientes (hijos).
Los datos para la operación podrán ser tomados del atributo **ID** y del argumento **data** o de los valores pasados por medio de los parámetros **\$aData1, ,\$...**
El o los registros serán identificados en base a su **ID**, **IMYA** y/o al **PID** del objeto padre.
Al finalizar se retornará el número de actualiciones ejecutadas exitosamente.
Un informe del detalle de las actualizaciones será almacenado en los atributos **log** y **history**.
El **ID** de la última actualición se guardará en el atributo **id**.

**IMPORTANTE** los campos **id**, **imya** y **state** son administrados por el método, por lo que serán ignorados.  

**[int]** =  *public* function ( *mixed* \$aData1, *array* \$... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aData1**|mixed|array|Objeto o array asociativo con los nombres de las columnas y datos que se usará en los métodos de escritura. Este argumento no es válido para escrituras múltiples|
|**\$...**|array||Lista variable de argumentos de tipo Array similares a **\$aData1**|
&nbsp;
___
&nbsp;

## UpdateData
Ejecuta las actualizaciones enviadas por los métodos  

**[int]** =  *private* function ( *mixed* \$aArguments, *int* \$nState );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$aArguments**|mixed|array|Objeto o array asociativo con los nombres de las columnas y datos que se usará en los métodos de escritura. Este argumento no es válido para escrituras múltiples|
|**\$nState**|int||Estado al que se cambiaran los registros:<ul><li>**0** =  eliminado (en la base de datos setea `state` = NULL)</li><li>**1** =  activo (en la base de datos setea `state` = 1)</li><li>**2** =  suspendido (en la base de datos setea `state` = 0)</li></ul>|
&nbsp;
___
&nbsp;

## Validate
Realiza la validación de datos por medio del objeto **nglValidate**  

**[array]** =  *private* function ( *array* \$vData, *string* \$sRules );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$vData**|array||Datos a validar|
|**\$sRules**|string||Conjunto de reglas para la validación|
&nbsp;
___
&nbsp;

## view
Retorna las partes SELECT y FROM de la consulta SQL necesaria para generar una VIEW del objeto activo  

 *public* function ( );
  
&nbsp;
___
&nbsp;

## viewChildren
Retorna las partes SELECT y FROM de la consulta SQL necesaria para generar una VIEW de un hijo del objeto activo  

**[string]** =  *public* function ( *string* \$sOutputMode, *string* \$sAliasMode, *boolean* \$bJoins );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sOutputMode**|string|jsql|Modo de salida de datos en el método **nglOwl::view**:<ul><li>**jsql** =  formato JSON</li><li>**sql** =  formato ANSI SQL</li></ul>|
|**\$sAliasMode**|string|auto|Política utilizada para nombrar los alias en el método **nglOwl::view**, se antepondrá el nombre de la tabla cuando:<ul><li>**all** =  en todos los campos de todas las tablas</li><li>**joins** =  en todos los campos, salvo en los de la tabla principal</li><li>**auto** =  sólo los campos que tengan un duplicado</li><li>**none** =  ningun campo</li></ul>|
|**\$bJoins**|boolean|true|Activa y desactiva la unión con las tablas relacionadas (no dependientes) en el método **nglOwl::view**|
&nbsp;
___
&nbsp;

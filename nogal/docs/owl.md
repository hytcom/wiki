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
|Método|Descripción|
|---|---|
|[CrossRows](#CrossRows)|Verifica las referencias de la tabla $sTable con el resto de las tablas del objeto y retorna un array informandolas|
|[DeleteInCascade](#DeleteInCascade)|Ejecuta las sentencias de borrado $aCascade y retorna el n�mero de registros borrados|
|[GetID](#GetID)|Obtiene el ID del registro $mID de la tabla $sTable confirmando la existencia del mismo, en base a su IMYA o al propio ID.Cuando $sTable sea distinto ...|
|[Logger](#Logger)|Registra la salida de LOG de un m�todo en los atributos log y history. Cuando $sStatus = NULL se resetear� el valor del atributo log|
|[OwLog](#OwLog)|Genera un log sobre cada acci�n de escritura. El mismo es almacenado en la base datos en la tabla `__ngl_owl_log__` y pasado al m�todo nglOwl::Logge...|
|[UpdateData](#UpdateData)|Ejecuta las actualizaciones enviadas por los m�todos|
|[Validate](#Validate)|Realiza la validaci�n de datos por medio del objeto nglValidate|
|[child](#child)|Prepara el objeto para trabajar con la dependencia $sChild y lo retorna|
|[children](#children)|Lista las tablas dependientes de objeto activo|
|[close](#close)|Finaliza la conexi�n con la base de datos|
|[columns](#columns)|Retorna los nombres de la columnas del objeto activo|
|[connect](#connect)|Establece la conexi�n con la base de datos|
|[delete](#delete)|Utiliza el m�todo nglOwl::UpdateData para intentar eliminar el o los registros seleccionados, ya sean del una tabla principal o de una dependiente (h...|
|[describe](#describe)|Detalles del objeto activo|
|[duplicate](#duplicate)|Duplica un registro con o sin sus dependencias (hijos). Las diferentes metodolog�as de ejecuci�n son:principal CON dependencias: cuando $bChildren =...|
|[get](#get)|Retorna un objeto iNglDataObjet con los datos de un registro y todas sus relaciones en base a su ID o IMYA|
|[getAll](#getAll)|Retorna un objeto iNglDataObjet con todos registros y relaciones en base a su $sFilter|
|[insert](#insert)|Inserta uno o mas registros en las tablas que componen los objetos. Este m�todo tambi�n efectua las inserciones en las tablas dependientes (hijos).A...|
|[query](#query)|Ejecuta una sentencia JSQL utilizando el m�todo query del objeto $db y retorna un objecto del tipoiNglDBQuery|
|[relationship](#relationship)|Muestra la estructura relacional del objeto seleccionado|
|[select](#select)|Selecciona y establece como activo al objeto $sObjectName|
|[showtables](#showtables)|Retorna un Array con los datos de todos los elementos que componen el sistema DB. Cada indice del Array est� compuesto por:name: String con el nombre...|
|[suspend](#suspend)|Suspende uno o mas registros aplicando la misma l�gica que el m�todo nglOwl::update, con la diferencia de que s�lo un campo es actualizado; state =...|
|[toggle](#toggle)|Suspende y/o desuspende uno o mas registros aplicando la misma l�gica que el m�todo nglOwl::update, con la diferencia de que s�lo un campo es actua...|
|[unsuspend](#unsuspend)|Reactiva registros suspendidos por nglOwl::suspend, actualizando state = 1Cuando un registro principal es reactivado, todos sus registros dependientes...|
|[update](#update)|Actualiza uno o mas registros en las tablas que componen los objetos. Este m�todo tambi�n efectua las actualizaciones en las tablas dependientes (hi...|
|[view](#view)|Retorna las partes SELECT y FROM de la consulta SQL necesaria para generar una VIEW del objeto activo|
|[viewChildren](#viewChildren)|Retorna las partes SELECT y FROM de la consulta SQL necesaria para generar una VIEW de un hijo del objeto activo|

  
&nbsp;


## child
Prepara el objeto para trabajar con la dependencia **\$sChild** y lo retorna  

**[$this]** =  *public* function ( *string* \$sChild );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sChild**|string|null|Nombre del Objeto dependiente activo|
### Ejemplos  
#### inserción de datos dependientes  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

#selección del registro principal con ID 1
$foo->select('customers')->id = 1;

#insercion de registros vinculados al registro principal
$foo->child('customers_contacts')->insert(
    array('firstname'=>'Martino', 'surname'=>'Herrera', 'age'=>'35', 'gender'=>'M'),
    array('firstname'=>'Monica', 'surname'=>'Castillo', 'age'=>'29', 'gender'=>'F'),
    array('firstname'=>'Juan', 'surname'=>'Rojas', 'age'=>'43', 'gender'=>'M')
);
```
#### lectura de dependencias (hijos)  
```php
#consulta
$foo->select('customers')->id(1);
$data = $foo->child('customers_contacts')->get('2', 'all', true);
print_r($data->get());

#salida
Array (
    [customers_contacts_id] => 2
    [customers_contacts_imya] => J7d66l11xyqk6vB1f691b2bbA5068993
    [customers_contacts_state] => 1
    [customers_contacts_pid] => 1
    [customers_contacts_town] => 2
    [customers_contacts_function] => 1
    [customers_contacts_firstname] => Monica
    [customers_contacts_surname] => Castillo
    [customers_contacts_age] => 29
    [customers_contacts_gender] => F
    [functions_id] => 1
    [functions_imya] => kak273b38cm4c5bfcaTe5fAec8S81xaC
    [functions_state] => 1
    [functions_name] => Tesorero
    [towns_id] => 2
    [towns_imya] => S5kR0d9cA4c9Ow8accd5aO4dBd62eX81
    [towns_state] => 1
    [towns_name] => Belgrano
)
```

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
### Ejemplos  
#### por atributo ID  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

#selección del registro con ID 1
$foo->select('customers')->id = 1;
$foo->delete();
```
#### por ID o IMYA a través de $aData1  
```php
$foo->select('customers')->delete(array('id'=>1));
$foo->select('customers')->delete(array('imya'=>'wdoxbe6a2afqMbc7S0fy2f57hel61ec4'));
```
#### por ID o IMYA a través de $aData1, $aData2, $... (modo múltiple)  
```php
$foo->select('customers')->delete(
    array('id'=>1),
    array('id'=>2),
    array('imya'=>'fq6213Xb5qf4c5a286e8rcOacf66b2rc')
);
```
#### modo cascada FALSE  
```php
$foo->cascade = false;
$ngl()->dump($foo->select('customers')->delete(array('id'=>1)));
$ngl()->dump($foo->log, 'pre');

//-- salida ------------
bool(false)

Array (
    [status] => foreignkeys
    [details] => Array (
        [0] => 3 ROWS IN `customers_contacts` LINKED WITH `customers`
        [1] => 1 ROWS IN `meeting` LINKED WITH `customers_contacts`
    )
)
```
#### modo cascada TRUE  
```php
$foo->cascade = true;
$ngl()->dump($foo->select('customers')->delete(array('id'=>1)));
$ngl()->dump($foo->log, 'pre');

//-- salida ------------
int(5)

Array (
    [status] => success
    [details] => Array (
        [0] => Array (
            [table] => meeting
            [row] => 1
            [user] => 
            [action] => delete
            [date] => 2014-12-31 02:26:58
            [ip] => 127.0.0.1
        )
        [1] => Array (
            [table] => customers_contacts
            [row] => 1
            [user] => 
            [action] => delete
            [date] => 2014-12-31 02:26:58
            [ip] => 127.0.0.1
        )
        [2] => Array (
            [table] => customers_contacts
            [row] => 2
            [user] => 
            [action] => delete
            [date] => 2014-12-31 02:26:58
            [ip] => 127.0.0.1
        )
        [3] => Array (
            [table] => customers_contacts
            [row] => 3
            [user] => 
            [action] => delete
            [date] => 2014-12-31 02:26:58
            [ip] => 127.0.0.1
        )
        [4] => Array (
            [table] => customers
            [row] => 1
            [user] => 
            [action] => delete
            [date] => 2014-12-31 02:26:58
            [ip] => 127.0.0.1
        )
    )
)
```
#### el Contacto con ID 2 del Cliente con ID 1  
```php
$foo->select('customers')->id = 1;
$foo->child('customers_contacts')->delete(array('id'=>2));

// NOTA: si no se seleccionase un ID para el registro principal se producirá un error
```
#### todos los Contactos del Cliente con ID 1  
```php
$foo->select('customers')->id = 1;
$foo->child('customers_contacts')->delete();
```

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
### Ejemplos  
#### principal CON dependencias  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

$foo->select('customers');
$foo->duplicate(1,true);
```
#### principal SIN dependencias  
```php
$foo->select('customers');
$foo->duplicate(1,false);
```
#### dependencia específica  
```php
#duplica el contacto con ID 19 que depende del cliente 6
$foo->select('customers')->id(6);
$foo->child('customers_contacts')->duplicate(19);
```
#### todas las dependencias  
```php
#duplica todos los contactos del cliente 6
$foo->select('customers')->id(6);
$foo->child('customers_contacts')->duplicate();
```

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
### Ejemplos  
#### lectura por ID  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);
print_r($foo->select('customers')->get(1)->get());
```
#### lectura por IMYA con dependencias y relaciones  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);
$data = $foo->select('customers')->get('fq6213Xb5qf4c5a286e8rcOacf66b2rc', 'all', true, true);
print_r($data->getall());
```
#### lectura de sólo dependencias (hijos) con relaciones  
```php
#con relaciones
$foo->select('customers')->id('URI75pbceMb7ca85acLD7160Ze40Kb94');
$data = $foo->child('customers_contacts')->get('J7d66l11xyqk6vB1f691b2bbA5068993', 'all', true);
print_r($data->get());
```

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
### Ejemplos  
#### JSQL filter  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);
$get = $foo->select('customers')->getAll('{
    'where': [['customers.id','eq','(1)']], 
    'order': ['customers.tradename:ASC']
}');
print_r($get->getall());
```
#### Obtener todos los registros Hijos de un ID  
```php
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);
$foo->select('customers')->id('URI75pbceMb7ca85acLD7160Ze40Kb94');
$data = $foo->child('customers_contacts')->getAll();
print_r($data->getall());
```

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
### Ejemplos  
#### inserción por array  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

#preparando datos
$a = array();
$a['tradename'] = 'abcontenidos';
$a['cuit'] = '30111111110';
$a['email'] = 'info@abcontenidos.com';

#insercion de datos
$foo->select('customers');
$foo->insert($a);

#log de la operación
Array (
    [status] => success
    [details] => Array (
        [0] => Array (
            [table] => customers
            [row] => 5
            [user] => 
            [action] => insert
            [date] => 2015-01-01 18:26:35
            [ip] => 127.0.0.1
        )
    )
)
```
#### inserción por objeto  
```php
$a = new stdClass();
$a->tradename = 'abcontenidos';
$a->cuit = '30111111110';
$a->email = 'info@abcontenidos.com';
$foo->select('customers')->insert($a);
```
#### inserción por argument::data  
```php
$vData = array();
$vData['tradename'] = 'abcontenidos';
$vData['cuit'] = '30111111110';
$vData['email'] = 'info@abcontenidos.com';
$foo->data = $vData;

#insercion de datos
$foo->select('customers')->insert();
```
#### inserción multiple  
```php
$foo->select('products');
$foo->insert(
    array('name'=>'Monitor', 'barcode'=>'00000001'),
    array('name'=>'Mouse', 'barcode'=>'00000002'),
    array('name'=>'Teclado', 'barcode'=>'00000003'),
    array('name'=>'Gabinete', 'barcode'=>'00000004')
);
```
#### inserción de registros dependientes (hijos)  
```php
// en un primer paso se debe seleccionar el registro de la tabla principal
// al cual se asociaran los registros dependientes
$foo->select('customers')->id = 1;

// luego se procede a la inserción
$foo->child('customers_contacts')->insert(array('firstname'=>'Alberto', 'surname'=>'Acosta'));

// modo múltiple 
$foo->child('customers_contacts')->insert(
    array('firstname'=>'Martino', 'surname'=>'Herrera'),
    array('firstname'=>'Alejo', 'surname'=>'Castillo'),
    array('firstname'=>'Juan', 'surname'=>'Rojas')
);
```

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
### Ejemplos  
#### MySQL  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

#ejecución
$bar = $foo->query('{
        "columns":["id"], 
        "tables":["customers"], 
        "where":[["id","eq","(3)"]]
    }
);
```
#### SQLite  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('sqlite.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

#ejecución
$bar = $foo->query('{
        "columns":["id"], 
        "tables":["customers"], 
        "where":[["id","eq","(3)"]]
    }
);
```

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
### Ejemplos  
#### selección por ID  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);
$foo->select('customers');
$foo->id(156);
```
#### selección por IMYA  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);
$foo->select('customers');
$foo->id('wdoxbe6a2afqMbc7S0fy2f57hel61ec4');
```

&nbsp;
___
&nbsp;

## showtables
Retorna un Array con los datos de todos los elementos que componen el sistema **DB**. Cada indice del Array está compuesto por:<ul><li>**name** =  String con el nombre de la tabla</li><li>**columns** =  Array con las columnas de la tabla</li><li>**foreignkey** =  True cuando la tabla contiene foreignkeys</li><li>**relationship** =  True cuando la tabla es ademas la tabla principal de un objeto **DB**</li></ul>  

**[array]** =  *public* function ( );
  
### Ejemplos  
#### por atributo ID  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

#listado de tablas
$ngl->dump($foo->showtables());
```

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
### Ejemplos  
#### suspención múltiple y basada en el ID o IMYA  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

#suspención de registro principales
$foo->select('customers')->suspend(
    array('id'=>4),
    array('imya'=>'wdoxbe6a2afqMbc7S0fy2f57hel61ec4')
);

#suspención de registro dependientes
$foo->child('customers_contacts')->suspend(
    array('imya'=>'J7d66l11xyqk6vB1f691b2bbA5068993'),
    array('imya'=>'fq6213Xb5qf4c5a286e8rcOacf66b2rc')
);
```

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
### Ejemplos  
#### campios de estado múltiples y basada en el ID o IMYA  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

#suspención de registro principales
$foo->select('customers')->toggle(
    array('id'=>4),
    array('imya'=>'wdoxbe6a2afqMbc7S0fy2f57hel61ec4')
);

#suspención de registro dependientes
$foo->child('customers_contacts')->toggle(
    array('imya'=>'J7d66l11xyqk6vB1f691b2bbA5068993'),
    array('imya'=>'fq6213Xb5qf4c5a286e8rcOacf66b2rc')
);
```

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
### Ejemplos  
#### reactivación múltiple y basada en el ID o IMYA  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

#reactivación de registro principales
$foo->select('customers')->unsuspend(
    array('id'=>4),
    array('imya'=>'wdoxbe6a2afqMbc7S0fy2f57hel61ec4')
);

#reactivación de registro dependientes
$foo->child('customers_contacts')->unsuspend(
    array('imya'=>'J7d66l11xyqk6vB1f691b2bbA5068993'),
    array('imya'=>'fq6213Xb5qf4c5a286e8rcOacf66b2rc')
);
```

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
### Ejemplos  
#### por atributo ID  
```php
#creacion del objeto y conexion a la db
$conn = $ngl('mysql.test');
$foo = $ngl('owl.foo');
$foo->connect($conn);

#selección del registro con ID 1
$foo->select('customers')->id = 1;

#preparando datos
$a = array();
$a['tradename'] = 'hytcom';
$a['email'] = 'info@hytcom.net'; 

#actualización
$foo->update($a);
```
#### actualización múltiple y basada en el ID o IMYA pasado dentro del grupo de datos  
```php
#selección del objeto y actualización
$foo->select('customers')->update(
    array('id'=>1, 'town'=>2),
    array('imya'=>'wdoxbe6a2afqMbc7S0fy2f57hel61ec4', 'town'=>2)
);
```
#### actualización de registros dependientes (hijos)  
```php
// en un primer paso se debe seleccionar el registro de la tabla principal
// al cual están asociados los registros dependientes
$foo->select('customers')->id = 1;

// luego se procede a la actualización
$foo->child('customers_contacts')->update(array('id'=>1, 'firstname'=>'Carlos Alberto'));

// modo múltiple 
$foo->child('customers_contacts')->update(
    array('id'=>2, 'email'=>'mherrera@hytcom.net'),
    array('id'=>3, 'email'=>'acastillo@hytcom.net'),
    array('id'=>4, 'email'=>'jrojas@hytcom.net')
);
```
#### todos los Contactos del Cliente con ID 2  
```php
$foo->select('customers')->id = 2;
$foo->child('customers_contacts')->update('town'=>'6');
```

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

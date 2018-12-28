# owl
Owl es el ORM de NOGAL y permite ejecutar operaciones sobre distintos objetos de base de datos.
Entre las funciones del objecto se encuentran:
- consulta de listados de registros directos y con referencias cruzadas
- consulta de un registro en particular
- administración de objetos depentientes, como por ejemplo los datos una empresa y sus empleados
- uso de foreignkeys a nivel objeto, sin importar el driver de base de datos
- validación de datos por medio del objeto [validate](validate.md)
- permite añadir, modificar, suspender y eliminar (eliminado lógico) registros
- eliminación de registros en cascada
Para ver un ejemplo de uso completo ver la guía [owl paso a paso](owluso.md)

## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|**$db**|private|Objeto de base de datos|
|**$sObject**|private|Nombre del Objeto activo|
|**$vObjects**|private|Objetos cargados durante la ejecución|
|**$sChildTable**|private|Nombre de la tabla del objeto dependiente activo|
|**$bChildMode**|private|Establece el modo Child para los métodos de escritura|
|**$bInternalCall**|private|Señala que llamada al método es interna. Usada para evitar el log|
|**$aCascade**|private|Almacena las sentencias SQL para el borrado logico en cascada del último CrossRows|
|**$aTmpChildren**|private|Almacena temporalmente los hijos durante un [select](#select)|

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**alvin**|boolean|false|Activa y desactiva los chequeos [alvin](alvin.md)|
|**cascade**|boolean|false|Activa y desactiva el borrado en cascada|
|**child**|string|null|Nombre del Objeto dependiente activo|
|**data**|mixed|array|Objeto o array asociativo con los nombres de las columnas y datos que se usará en los métodos de escritura. Este argumento no es válido para escrituras múltiples|
|**db**|object|null|Objeto de base de datos|
|**duplicate_children**|boolean|false|Activa y desactiva la copia de registros dependientes en el método [duplicate](#duplicate)|
|**escape**|boolean|null|Determina si *arg::data* debe o no ser tratado por el método **escape** del objeto **$db**|
|**filter**|string|null|Porción de la código SQL ó JSQL que complementa al generado por el método [view](#view) y es utilizado para filtrar los resultados del método [getall](#getall)|
|**id**|mixed|null|Selecciona un registro del objeto activo utilizando el propio **ID** o **IMYA**|
|**inherit**|boolean|true|Determina las suspenciones deben propagarse a las tablas dependientes|
|**insert_mode**|string|INSERT|Modo del método nglOwl::insert. Valores admitidos:<ul><li>**INSERT** = inserta nuevos registros</li><li>**REPLACE** = si el nuevo registro duplica un valor PRIMARY KEY o UNIQUE, el antiguo registro es eliminado</li><li>**IGNORE** = el comando no aborta incluso si ocurren errores durante la ejecución</li></ul>|
|**jsql**|string|null|Sentencia JSQL para ser ejecutada utilizando el método [query](#query)|
|**owlog**|boolean|true|Activa y desactiva el log en la tabla **__ngl_owl_log__**|
|**owlog_changelog**|boolean|false|Activa y desactiva el uso del changelog en la tabla **__ngl_owl_log__**|
|**object**|string|null|Nombre del Objeto que se establecerá como activo|
|**use_history**|boolean|false|Activa y desactiva el uso del atributo **attr::history**|
|**view_alias**|string|auto|Política utilizada para nombrar los alias en el método [view](#view), se antepondrá el nombre de la tabla cuando:<ul><li>**all** = en todos los campos de todas las tablas</li><li>**joins** = en todos los campos, salvo en los de la tabla principal</li><li>**auto** = sólo los campos que tengan un duplicado</li><li>**none** = ningun campo</li></ul>|
|**view_children**|mixed|false|Determina el tipo de unión con las tablas dependientes<ul><li>**true** = todas las tablas</li><li>**false** = ninguna tabla</li><li>**array** = array con tablas seleccionadas</li></ul>|
|**view_columns**|string|null|Cadena JSQL con los nombres de las columnas que deberá retornar el método [view](#view).<br />Sintáxis: ["TABLE.COLUMN","ALIAS"] o "TABLE.COLUMN"<br />Ej: [["tabla.campo1","foo"], "alias2.campo2", ["campo3","bar"]]|
|**view_joins**|boolean|true|Activa y desactiva la unión con las tablas relacionadas (no dependientes) en el método [view](#view)|
|**view_mode**|string|jsql|Modo de salida de datos en el método [view](#view):<ul><li>**jsql** = formato JSON</li><li>**sql** = formato ANSI SQL</li></ul>|

## Atributos
|Atributo|Tipo|Descripción|
|---|---|---|
|**current**|int|ID del registro activo|
|**attr::history**|string|Almacena el historial de logs durante la ejecución|
|**attr::log**|string|Almacena el log de la última acción ejecutada|
|**object_name**|string|Nombre del Objeto activo|
|**query**|string|Ultima consulta SQL ejecutada|
|**result**|string|Ultimo resultado SQL ejecutado por get o getall|
|**validate**|array|Resultado de la última validación por [validate](validate.md#validate)|
  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[child](#child)|Prepara el objeto para trabajar con dependencias|
|[children](#children)|Lista las tablas dependientes de objeto activo|
|[close](#close)|Finaliza la conexión con la base de datos|
|[columns](#columns)|Retorna los nombres de la columnas del objeto activo|
|[delete](#delete)|Utiliza el método [UpdateData](#updatedata) para intentar eliminar el o los registros seleccionados|
|[describe](#describe)|Detalles del objeto activo|
|[duplicate](#duplicate)|Duplica un registro con o sin sus dependencias (hijos)|
|[get](#get)|Retorna un objeto de base de datos de un registro y todas sus relaciones|
|[getall](#getall)|Retorna un objeto con todos registros y relaciones|
|[insert](#insert)|Inserta uno ó más registros en las tablas que componen los objetos|
|[load](#load)|Carga el objeto que establece la conexión con la base de datos |
|[query](#query)|Ejecuta una sentencia **JSQL** utilizando el método query del objeto **$db**|
|[relationship](#relationship)|Muestra la estructura relacional del objeto seleccionado|
|[select](#select)|Selecciona y establece como activo al objeto|
|[showtables](#showtables)|Retorna un Array con los datos de todos los elementos que componen el sistema|
|[suspend](#suspend)|Suspende uno ó más registros aplicando la misma lógica que [update](#update)|
|[toggle](#toggle)|Suspende y/o desuspende uno ó más registros|
|[unsuspend](#unsuspend)|Reactiva registros suspendidos por [suspend](#suspend)|
|[update](#update)|Actualiza uno ó más registros en las tablas que componen los objetos|
|[view](#view)|Retorna las partes SELECT y FROM de la consulta SQL necesaria para generar una VIEW|
|[viewChildren](#viewchildren)|Identico a [view](#view) pero en objetos con depentencias|
|Internos||
|[AlvinInit](#alvininit)|Verifica el uso de [alvin](alvin.md) y lo activa en caso de ser requerido|
|[AlvinSQL](#alvinsql)|Modifica las consultas para incorporar las restricciones|
|[CrossRows](#crossrows)|Verifica las referencias de la tabla $sTable con el resto de las tablas del objeto|
|[DeleteInCascade](#deleteincascade)|Ejecuta las sentencias de borrado y retorna el número de registros eliminados|
|[GetID](#getid)|Obtiene el ID de un registro confirmando la existencia del mismo|
|[GetRelationship](#getrelationship)|Obtiene las relaciones de un objeto dentro de la estructura|
|[GetRelationshipChildren](#getrelationshipchildren)|Obtiene las relaciones de los objetos dependientes de un objeto dentro de la estructura|
|[JsonAppener](#jsonappener)|Añade una porsición de código JSQL a otra|
|[JsqlParser](#jsqlparser)|Parsea una cadena JSQL utilizando el método **jsqlParser** del objeto de base de datos|
|[Logger](#logger)|Registra la salida de LOG de un método en los atributos log y history|
|[OwLog](#owlog)|Genera un log sobre cada acción de escritura en la tabla **__ngl_owl_log__**|
|[UpdateData](#updatedata)|Ejecuta las actualizaciones enviadas por los métodos|
|[Validate](#validate)|Realiza la validación de datos por medio del objeto [validate](validate.md)|
  
&nbsp;

## child
> Prepara el objeto para trabajar con la dependencia **$sChild** y lo retorna  

**[$this]** = *public* function ( *string* $sChild );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sChild**|string|*arg::child*|Nombre del Objeto dependiente activo|
### Ejemplos  
#### inserción de datos dependientes  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl");
$foo->load($conn);

#selección del registro principal con ID 1
$foo->select("customers")->id = 1;

#insercion de registros vinculados al registro principal
$foo->child("customers_contacts")->insert(
    array("firstname"=>"Martino", "surname"=>"Herrera", "age"=>"35", "gender"=>"M"),
    array("firstname"=>"Monica", "surname"=>"Castillo", "age"=>"29", "gender"=>"F"),
    array("firstname"=>"Juan", "surname"=>"Rojas", "age"=>"43", "gender"=>"M")
);
```
#### lectura de dependencias (hijos)  
``` php
#consulta
$foo->select("customers")->id(1);
$data = $foo->child("customers_contacts")->get("2", "all", true);
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
> Lista las tablas dependientes de objeto activo  

**[array]** = *public* function ( );
  
&nbsp;
___
&nbsp;

## close
> Finaliza la conexión con la base de datos  

**[boolean]** = *public* function ( );

&nbsp;
___
&nbsp;

## columns
> Retorna los nombres de la columnas del objeto activo  

**[array]** = *public* function ( );

&nbsp;
___
&nbsp;

## delete
> Utiliza el método [UpdateData](#updatedata)* para intentar eliminar el o los registros seleccionados, ya sean del una tabla principal o de una dependiente (hijo).
> Si el existiesen conflictos de **foreignkey** el borrado no se llevará a cabo y un informe de dependencias será almacenado en los atributos **attr::log** y **attr::history**.
> En este último caso **delete** retornará **false**. De lo contrario devolverá el número de elementos borrados.
> Si el argumento **cascade** estuviese indicado como **true** y hubiese conflictos de **foreignkey**, se omitirá el informe y el borrado se llevará a cabo de manera recursiva.  

**[int]** = *public* function ( *mixed* $aData1, *array* $... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aData1**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**$...**|array||Lista variable de argumentos de tipo Array similares a **$aData1**|
### Ejemplos  
#### por atributo ID  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl");
$foo->load($conn);

#selección del registro con ID 1
$foo->select("customers")->id = 1;
$foo->delete();
```

#### por ID o IMYA a través de $aData1  
``` php
$foo->select("customers")->delete(array("id"=>1));
$foo->select("customers")->delete(array("imya"=>"wdoxbe6a2afqMbc7S0fy2f57hel61ec4"));
```

#### por ID o IMYA a través de $aData1, $aData2, $... (modo múltiple)  
``` php
$foo->select("customers")->delete(
    array("id"=>1),
    array("id"=>2),
    array("imya"=>"fq6213Xb5qf4c5a286e8rcOacf66b2rc")
);
```

#### modo cascada FALSE  
``` php
$foo->cascade = false;
$ngl()->dump($foo->select("customers")->delete(array("id"=>1)));
$ngl()->dump($foo->log, "pre");

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
``` php
$foo->cascade = true;
$ngl()->dump($foo->select("customers")->delete(array("id"=>1)));
$ngl()->dump($foo->log, "pre");

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
``` php
$foo->select("customers")->id = 1;
$foo->child("customers_contacts")->delete(array("id"=>2));

// NOTA: si no se seleccionase un ID para el registro principal se producirá un error
```

#### todos los Contactos del Cliente con ID 1  
``` php
$foo->select("customers")->id = 1;
$foo->child("customers_contacts")->delete();
```

&nbsp;
___
&nbsp;

## describe
> Detalles del objeto activo  

**[array]** = *public* function ( );  

&nbsp;
___
&nbsp;

## duplicate
> Duplica un registro con o sin sus dependencias (hijos). Las diferentes metodologías de ejecución son:
> - **principal CON dependencias** = cuando **$bChildren** = true
> - **principal SIN dependencias** = cuando **$bChildren** = false
> - **dependencia específica** = cuando se ejecute por medio del método [child](#child) se duplicará el registro dependiente con el id o imya **$mID**
> - **todas las dependencias** = cuando se ejecute por medio del método [child](#child) y el valor **$mID** no sea especificado
> 
> Al finalizar se retornará el **ID** del registro duplicado; si se hubiese ejecutado una inserción multiple se retornará un array con todos los nuevos **IDs**.
> Un informe del detalle de las duplicaciones será almacenado en los atributos **attr::log** y **attr::history**.
> El **ID** de la última inserción se guardará en el atributo **id**.  
> **IMPORTANTE** para duplicar sólo dependencias se deberá seleccionar previamente un registro principal.  

**[mixed]** = *public* function ( *mixed* $mID, *boolean* $bChildren );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mID**|mixed|*arg::id*|Cadena IMYA o número entero|
|**$bChildren**|boolean|*arg::duplicate_children*|Activa y desactiva la copia de registros dependientes|
### Ejemplos  
#### principal CON dependencias  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);

$foo->select("customers");
$foo->duplicate(1,true);
```

#### principal SIN dependencias  
``` php
$foo->select("customers");
$foo->duplicate(1,false);
```

#### dependencia específica  
``` php
#duplica el contacto con ID 19 que depende del cliente 6
$foo->select("customers")->id(6);
$foo->child("customers_contacts")->duplicate(19);
```

#### todas las dependencias  
``` php
#duplica todos los contactos del cliente 6
$foo->select("customers")->id(6);
$foo->child("customers_contacts")->duplicate();
```

&nbsp;
___
&nbsp;

## get
> Retorna un objeto de base de datos de un registro y todas sus relaciones en base a su **ID** o **IMYA**  

**[object]** = *public* function ( *mixed* $mID, *string* $sAliasMode, *boolean* $bJoins, *mixed* $mChildren );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mID**|mixed|*arg::id*|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**$sAliasMode**|string|*arg::view_alias*|Política utilizada para nombrar los alias en el método [view](#view), se antepondrá el nombre de la tabla cuando:<ul><li>**all** = en todos los campos de todas las tablas</li><li>**joins** = en todos los campos, salvo en los de la tabla principal</li><li>**auto** = sólo los campos que tengan un duplicado</li><li>**none** = ningun campo</li></ul>|
|**$bJoins**|boolean|*arg::view_joins*|Activa y desactiva la unión con las tablas relacionadas (no dependientes) en el método [view](#view)|
|**$mChildren**|mixed|*arg::view_children*|Determina el tipo de unión con las tablas dependientes<ul><li>**true** = todas las tablas</li><li>**false** = ninguna tabla</li><li>**array** = array con tablas seleccionadas</li></ul>|
### Ejemplos  
#### lectura por ID  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);
print_r($foo->select("customers")->get(1)->get());
```

#### lectura por IMYA con dependencias y relaciones  
``` php
$data = $foo->select("customers")->get("fq6213Xb5qf4c5a286e8rcOacf66b2rc", "all", true, true);
print_r($data->getall());
```

#### lectura de sólo dependencias (hijos) con relaciones  
``` php
#con relaciones
$foo->select("customers")->id("URI75pbceMb7ca85acLD7160Ze40Kb94");
$data = $foo->child("customers_contacts")->get("J7d66l11xyqk6vB1f691b2bbA5068993", "all", true);
print_r($data->get());
```

&nbsp;
___
&nbsp;

## getall
> Retorna un objeto con todos registros y relaciones en base a su **$sFilter**

**[object]** = *public* function ( *string* $sFilter, *string* $sAliasMode, *boolean* $bJoins, *mixed* $mChildren, *string* $sColumns );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sFilter**|string|*arg::filter*|Porción de la código SQL o **JSQL** que complementa al generado por el método [view](#view) y es utilizado para filtrar los resultados del método **nglOwl::getAll**|
|**$sAliasMode**|string|*arg::view_alias*|Política utilizada para nombrar los alias en el método [view](#view), se antepondrá el nombre de la tabla cuando:<ul><li>**all** = en todos los campos de todas las tablas</li><li>**joins** = en todos los campos, salvo en los de la tabla principal</li><li>**auto** = sólo los campos que tengan un duplicado</li><li>**none** = ningun campo</li></ul>|
|**$bJoins**|boolean|*arg::view_joins*|Activa y desactiva la unión con las tablas relacionadas (no dependientes) en el método [view](#view)|
|**$mChildren**|mixed|*arg::view_children*|Determina el tipo de unión con las tablas dependientes<ul><li>**true** = todas las tablas</li><li>**false** = ninguna tabla</li><li>**array** = array con tablas seleccionadas</li></ul>|
|**$sColumns**|string|*arg::view_columns*|Cadena JSQL con los nombres de las columnas que deberá retornar el método [view](#view).<br />Sintáxis: ["TABLE.COLUMN","ALIAS"] o "TABLE.COLUMN"<br />Ej: [["tabla.campo1","foo"], "alias2.campo2", ["campo3","bar"]]|
### Ejemplos  
#### JSQL filter  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);
$get = $foo->select("customers")->getAll('{
    "where": [["customers.id","eq","(1)"]], 
    "order": ["customers.tradename:ASC"]
}');
print_r($get->getall());
```
#### Obtener todos los registros Hijos de un ID  
``` php
$foo->select("customers")->id("URI75pbceMb7ca85acLD7160Ze40Kb94");
$data = $foo->child("customers_contacts")->getAll();
print_r($data->getall());
```

&nbsp;
___
&nbsp;

## insert
> Inserta uno ó más registros en las tablas que componen los objetos. Este método también efectua las inserciones en las tablas dependientes (hijos).
> Al finalizar se retornará el **ID** de la inseción; si se hubiese ejecutado una inserción multiple se retornará un array con todos los nuevos **IDs**.
> Un informe del detalle de las inserciones será almacenado en los atributos **attr::log** y **attr::history**.
> El **ID** de la última inserción se guardará en el atributo **current**.  
> 
> **IMPORTANTE** los campos **id**, **imya** y **state** son administrados por el método, por lo que serán ignorados.  

**[mixed]** = *public* function ( *array* $aData1, *array* $... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aData1**|array||Array asociativo (o bidimensional de ellos) con los datos del registro a insertar|
|**$...**|array||Lista variable de argumentos de tipo Array similares a **$aData1**|
### Ejemplos  
#### inserción por array  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);

#preparando datos
$a = array();
$a["tradename"] = "abcontenidos";
$a["cuit"] = "30111111110";
$a["email"] = "info@abcontenidos.com";

#insercion de datos
$foo->select("customers");
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
``` php
$a = new stdClass();
$a->tradename = "abcontenidos";
$a->cuit = "30111111110";
$a->email = "info@abcontenidos.com";
$foo->select("customers")->insert($a);
```

#### inserción por argument::data  
``` php
$vData = array();
$vData["tradename"] = "abcontenidos";
$vData["cuit"] = "30111111110";
$vData["email"] = "info@abcontenidos.com";
$foo->data = $vData;

#insercion de datos
$foo->select("customers")->insert();
```

#### inserción multiple  
``` php
$foo->select("products");
$foo->insert(
    array("name"=>"Monitor", "barcode"=>"00000001"),
    array("name"=>"Mouse", "barcode"=>"00000002"),
    array("name"=>"Teclado", "barcode"=>"00000003"),
    array("name"=>"Gabinete", "barcode"=>"00000004")
);
```

#### inserción de registros dependientes (hijos)  
``` php
// en un primer paso se debe seleccionar el registro de la tabla principal
// al cual se asociaran los registros dependientes
$foo->select("customers")->id = 1;

// luego se procede a la inserción
$foo->child("customers_contacts")->insert(array("firstname"=>"Alberto", "surname"=>"Acosta"));

// modo múltiple 
$foo->child("customers_contacts")->insert(
    array("firstname"=>"Martino", "surname"=>"Herrera"),
    array("firstname"=>"Alejo", "surname"=>"Castillo"),
    array("firstname"=>"Juan", "surname"=>"Rojas")
);
```

&nbsp;
___
&nbsp;

## load
> Carga el objeto que establece la conexión con la base de datos y en caso de existir ejecuta el método connect

**[$this]** = *public* function ( *object* $driver );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$driver**|object|*arg::db*|Objecto de Base de Datos|
### Ejemplos  
#### MySQL  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);
```

&nbsp;
___
&nbsp;

## query
> Ejecuta una sentencia JSQL utilizando el método **query** del objeto **$db** y retorna un objecto del tipo **query result**

**[object]** = *public* function ( *string* $sJSQL, *array* $aArgs );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sJSQL**|string|null|Sentencia en formato [JSQL]((jsql.md)|
|**$aArgs**|array|*arg::jsql*|Argumentos pasados al método, que serán traducidos a la sentencia utilizando vsprintf|
### Ejemplos  
#### MySQL  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);

#ejecución
$bar = $foo->query('{
    "columns":["id"], 
    "tables":["customers"], 
    "where":[["id","eq","(3)"]]
}');
```

#### SQLite  
``` php
$conn = $ngl("sqlite.test");
$foo = $ngl("owl");
$foo->load($conn);

#ejecución
$bar = $foo->query('{
    "columns":["id"], 
    "tables":["customers"], 
    "where":[["id","eq","(3)"]]
}');
```

&nbsp;
___
&nbsp;

## relationship
> Muestra la estructura relacional del objeto seleccionado  

**[string]** = *public* function ( );
  
&nbsp;
___
&nbsp;

## select
Selecciona y establece como activo al objeto **$sObjectName**  

**[object]** = *public* function ( *string* $sObjectName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sObjectName**|string|*arg::object*|Nombre de la tabla/objecto que se establecerá como activo. Deberá respetar el patrón [a-zA-Z0-9_\-]+|
### Ejemplos  
#### selección 
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);
$foo->select("customers");
```

#### selección del objeto y de un registro por ID  
``` php
$foo->select("customers");
$foo->id(156);
```
#### selección del objeto y de un registro IMYA  
``` php
$foo->select("customers");
$foo->id("wdoxbe6a2afqMbc7S0fy2f57hel61ec4");
```

&nbsp;
___
&nbsp;

## showtables
Retorna un Array con los datos de todos los elementos que componen el sistema    
Cada indice del Array está compuesto por:
- **name** = string con el nombre de la tabla
- **columns** = array con las columnas de la tabla
- **foreignkey** = true cuando la tabla contiene foreignkeys
- **relationship** = true cuando la tabla es ademas la tabla principal de un objeto

**[array]** = *public* function ( );

### Ejemplos  
#### por atributo ID  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")>load($conn);

#listado de tablas
$ngl->dump($foo->showtables());
```

&nbsp;
___
&nbsp;

## suspend
> Suspende uno ó más registros aplicando la misma lógica que el método [update](#update), con la diferencia de que sólo un campo es actualizado: **state = 0**  
> Cuando un registro principal es suspendido, todos sus registros dependientes también lo son. Al finalizar se retornará el número de suspensiones ejecutadas exitosamente.  
> Un informe del detalle de las suspensiones será almacenado en los atributos **attr::log** y **attr::history**.  
> El **ID** de la última suspensión se guardará en el atributo **id**.  

**[int]** = *public* function ( *mixed* $aData1, *array* $... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aData1**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**$...**|array||Lista variable de argumentos de tipo Array similares a **$aData1**|
### Ejemplos  
#### suspención múltiple y basada en el ID o IMYA  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);

#suspención de registro principales
$foo->select("customers")->suspend(
    array("id"=>4),
    array("imya"=>"wdoxbe6a2afqMbc7S0fy2f57hel61ec4")
);

#suspención de registro dependientes
$foo->child("customers_contacts")->suspend(
    array("imya"=>"J7d66l11xyqk6vB1f691b2bbA5068993"),
    array("imya"=>"fq6213Xb5qf4c5a286e8rcOacf66b2rc")
);
```

&nbsp;
___
&nbsp;

## toggle
> Suspende y/o desuspende uno ó más registros aplicando la misma lógica que los métodos [suspend](#suspend) y [unsuspend](#unsuspend)

**[int]** = *public* function ( *mixed* $aData1, *array* $... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aData1**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**$...**|array||Lista variable de argumentos de tipo Array similares a **$aData1**|
### Ejemplos  
#### campios de estado múltiples y basada en el ID o IMYA  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);

#suspención de registro principales
$foo->select("customers")->toggle(
    array("id"=>4),
    array("imya"=>"wdoxbe6a2afqMbc7S0fy2f57hel61ec4")
);

#suspención de registro dependientes
$foo->child("customers_contacts")->toggle(
    array("imya"=>"J7d66l11xyqk6vB1f691b2bbA5068993"),
    array("imya"=>"fq6213Xb5qf4c5a286e8rcOacf66b2rc")
);
```

&nbsp;
___
&nbsp;

## unsuspend
> Suspende uno ó más registros aplicando la misma lógica que el método [update](#update), con la diferencia de que sólo un campo es actualizado: **state = 1**  
> Cuando un registro principal es suspendido, todos sus registros dependientes también lo son. Al finalizar se retornará el número de suspensiones ejecutadas exitosamente.  
> Un informe del detalle de las suspensiones será almacenado en los atributos **attr::log** y **attr::history**.  
> El **ID** de la última suspensión se guardará en el atributo **id**.   

**[int]** = *public* function ( *mixed* $aData1, *array* $... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aData1**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**$...**|array||Lista variable de argumentos de tipo Array similares a **$aData1**|
### Ejemplos  
#### reactivación múltiple y basada en el ID o IMYA  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);

#reactivación de registro principales
$foo->select("customers")->unsuspend(
    array("id"=>4),
    array("imya"=>"wdoxbe6a2afqMbc7S0fy2f57hel61ec4")
);

#reactivación de registro dependientes
$foo->child("customers_contacts")->unsuspend(
    array("imya"=>"J7d66l11xyqk6vB1f691b2bbA5068993"),
    array("imya"=>"fq6213Xb5qf4c5a286e8rcOacf66b2rc")
);
```

&nbsp;
___
&nbsp;

## update
> Actualiza uno ó más registros en las tablas que componen los objetos. Este método también efectua las actualizaciones en las tablas dependientes (hijos).  
> Los datos para la operación podrán ser tomados del atributo **ID** y del argumento **data** o de los valores pasados por medio de los parámetros **$aData1, ,$...**  
> El o los registros serán identificados en base a su **ID**, **IMYA** y/o al **PID** del objeto padre.  
> Al finalizar se retornará el número de actualiciones ejecutadas exitosamente.  
> Un informe del detalle de las actualizaciones será almacenado en los atributos **attr::log** y **attr::history**.  
> El **ID** de la última actualición se guardará en el atributo **id**.  
> 
> **IMPORTANTE** los campos **id**, **imya** y **state** son administrados por el método, por lo que serán ignorados.  

**[int]** = *public* function ( *mixed* $aData1, *array* $... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aData1**|mixed|array|Objeto o array asociativo con los nombres de las columnas y datos que se usará en los métodos de escritura. Este argumento no es válido para escrituras múltiples|
|**$...**|array||Lista variable de argumentos de tipo Array similares a **$aData1**|
### Ejemplos  
#### por atributo ID  
``` php
$conn = $ngl("mysql");
$foo = $ngl("owl")->load($conn);

#selección del registro con ID 1
$foo->select("customers")->id(1);

#preparando datos
$a = array();
$a["tradename"] = "abcontenidos";
$a["email"] = "info@abcontenidos.com"; 

#actualización
$foo->update($a);
```

#### actualización múltiple y basada en el ID o IMYA pasado dentro del grupo de datos  
``` php
#selección del objeto y actualización
$foo->select("customers")->update(
    array("id"=>1, "town"=>2),
    array("imya"=>"wdoxbe6a2afqMbc7S0fy2f57hel61ec4", "town"=>2)
);
```

#### actualización de registros dependientes (hijos)  
``` php
// en un primer paso se debe seleccionar el registro de la tabla principal
// al cual están asociados los registros dependientes
$foo->select("customers")->id = 1;

// luego se procede a la actualización
$foo->child("customers_contacts")->update(array("id"=>1, "firstname"=>"Carlos Alberto"));

// modo múltiple 
$foo->child("customers_contacts")->update(
    array("id"=>2, "email"=>"mherrera@abcontenidos.com"),
    array("id"=>3, "email"=>"acastillo@abcontenidos.com"),
    array("id"=>4, "email"=>"jrojas@abcontenidos.com")
);
```

#### todos los Contactos del Cliente con ID 2  
``` php
$foo->select("customers")->id = 2;
$foo->child("customers_contacts")->update("town"=>"6");
```

&nbsp;
___
&nbsp;

## upsert
> Es la combinación de los métodos [insert](#insert) y [update](#update), con la diferencia de que cuando el registro referencia existe es actualizado pero en caso de que no exista es creado.

**[int]** = *public* function ( *mixed* $aData1, *array* $... );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aData1**|mixed|null|Selecciona un registro del objeto activo utilizando el propio ID o IMYA|
|**$...**|array||Lista variable de argumentos de tipo Array similares a **$aData1**|

## view
> Retorna las partes SELECT y FROM de la consulta SQL necesaria para generar una VIEW del objeto activo  

**[string]** = *public* function ( *string* $sOutputMode, *string* $sAliasMode, *boolean* $, *mixed* $mChildren, *string* $sColumns );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sOutputMode**|string|*arg::view_mode*|Modo de salida de datos para el método: <ul><li>**jsql** = formato JSON</li><li>**sql** = formato ANSI SQL</li></ul>|
|**$sAliasMode**|string|*arg::view_alias*|Política utilizada para nombrar los alias, se antepondrá el nombre de la tabla cuando: <ul><li>**all** = en todos los campos de todas las tablas</li><li>**joins** = en todos los campos, salvo en los de la tabla principal</li><li>**auto** = sólo los campos que tengan un duplicado</li><li>**none** = ningun campo</li></ul>|
|**$bJoins**|boolean|*arg::view_joins*|Activa y desactiva la unión con las tablas relacionadas (no dependientes)|
|**$mChildren**|mixed|*arg::view_children*|Activa y desactiva la unión con las tablas dependientes|
|**$sColumns**|string|*arg::view_columns*|Cadena JSQL con los nombres de las columnas que deberá retornar el método. <br />Sintáxis: ["TABLE.COLUMN","ALIAS"] o "TABLE.COLUMN"<br />Ej: [["tabla.campo1","foo"], "alias2.campo2", ["campo3","bar"]]|

&nbsp;
___
&nbsp;

## viewchildren
> Retorna las partes SELECT y FROM de la consulta SQL necesaria para generar una VIEW de un hijo del objeto activo  

**[string]** = *public* function ( *string* $sOutputMode, *string* $sAliasMode, *boolean* $bJoins );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sOutputMode**|string|*arg::view_mode*|Modo de salida de datos en el método [view](#view):<ul><li>**jsql** = formato JSON</li><li>**sql** = formato ANSI SQL</li></ul>|
|**$sAliasMode**|string|*arg::view_alias*|Política utilizada para nombrar los alias en el método [view](#view), se antepondrá el nombre de la tabla cuando:<ul><li>**all** = en todos los campos de todas las tablas</li><li>**joins** = en todos los campos, salvo en los de la tabla principal</li><li>**auto** = sólo los campos que tengan un duplicado</li><li>**none** = ningun campo</li></ul>|
|**$bJoins**|boolean|*arg::view_joins*|Activa y desactiva la unión con las tablas relacionadas (no dependientes) en el método [view](#view)|

&nbsp;
___
&nbsp;

# Internos
## AlvinInit
> Verifica el uso de [alvin](alvin.md) y lo activa en caso de ser requerido

**[boolean]** = *private* function ( );  

&nbsp;
___
&nbsp;

## AlvinSQL
> Modifica las consultas para incorporar las restricciones consignadas en la tabla **__ngl_owl_index__**

**[array]** = *private* function ( *string* $sJSQL, *string* $sAlvinGrant );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sJSQL**|string||Sentencia [jsql](jsql.md)|
|**$sAlvinGrant**|string|select|Tipo de permiso que deberá aplicarse, estos pueden ser: <ul><li>**insert** = valor con el cual se marcarán los registros dados de alta por el usuario</li><li>**select** = listado separo por comas, de las marcas que está autorizado a ver el usuario. El asterísco indica acceso a todos los registros</li><li>**update** = listado separo por comas, de las marcas que está autorizado a modificar el usuario. El asterísco indica acceso a todos los registros</li></ul>|
### Ejemplos  
#### Formato de permisos **alvin-owl** 
``` json
// FORMATO alvin-owl
{
    "object_name": {
        "insert" : "valor unico",
        "select" : "* + el propio ID de insert",
        "update" : "valores,separados,por,comas + el propio ID de insert"
    },
    
    "object_name_2": {
        "insert" : "valor unico",
        "select" : "valores,separados,por,comas + el propio ID de insert",
        "update" : "valores,separados,por,comas + el propio ID de insert"
    }
}

// Ejemplo:
{
    // el usuario marca sus registros con el ID 1
    // puede ver todos los registros
    // tiene acceso a modificar sus propios registros y los de los usuarios 2 y 3
    "sales": {
        "insert" : "1",
        "select" : "*",
        "update" : "1,2,3",
    },

    // el usuario marca sus registros con el ID 1
    // puede ver sus propios registros y los de los usuarios 2 y 3
    // tiene acceso a modificar sólo sus registros
    "ventas": {
        "insert" : "1",
        "select" : "1,2,3",
        "update" : "1",
    }
}
```

&nbsp;
___
&nbsp;

## CrossRows
> Verifica las referencias de la tabla con el resto de las tablas del objeto y retorna un array informando  
> - **info** = reporte de relaciones
> - **cascade** = array bidimensional con los datos de las relaciones en cascada
>     - **0** = nombre de la tabla relacionada
>     - **1** = ids de los registros comprometidos separados por coma
>     - **2** = sentencia JSQL necesaria para eliminar logicamente a los registros comprometidos

**[array]** = *private* function ( *string* $sTable, *string* $sWhere, *array* $aConditions );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTable**|string||Nombre de la tabla principal|
|**$sWhere**|string||Condición WHERE que determina el conjunto de resultados sobre el cual se analizará las referencias|
|**$aConditions**|array||Array de condiciones FROM y WHERE que determina el conjunto de resultados en modo recursivo|

&nbsp;
___
&nbsp;

## DeleteInCascade
> Ejecuta las sentencias de borrado $aCascade y retorna el número de registros borrados  

**[int]** = *private* function ( *mixed* $aCascade );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aCascade**|mixed||Indice **cascade** del resultado arrojado por [CrossRows](#crossrows)|

&nbsp;
___
&nbsp;

## GetID
> Obtiene el ID de un registro (confirmando la existencia del mismo) en base a al propio ID, IMYA ó condicion SQL (en formato array).  
> Cuando **$mID** es un array se espera: ```array("campo", "signo", "valor")```  
> Cuando **$sTable** sea distinto de **NULL** el valor retornado será almacenado en el atributo **id**  

**[mixed]** = *private* function ( *mixed* $mID, *string* $sTable );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mID**|mixed||Cadena IMYA, array o número entero|
|**$sTable**|string|null|Nombre de la tabla en la cual buscar. Cuando el valor es **NULL** se trabajará sobre la tabla principal del objeto activo|

&nbsp;
___
&nbsp;

## GetRelationship
> Obtiene las relaciones JOIN y CHILDREN de un objeto dentro de la estructura

**[array]** = *private* function ( *array* &$aTables, *array* $vObject );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aTables**|array||Array donde se irán guardando las relaciones obtenidas|
|**$vObject**|array||Array con los datos de la estructura OWL del objeto relacionado|

&nbsp;
___
&nbsp;

## GetRelationshipChildren
> Obtiene las relaciones JOIN y CHILDREN de los objetos dependientes de un objeto dentro de la estructura

**[array]** = *private* function ( *array* &$aTables, *array* $vObject, *string* $sAlias );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aTables**|array||Array donde se irán guardando las relaciones obtenidas|
|**$vObject**|array||Array con los datos de la estructura OWL del objeto relacionado|
|**$sAlias**|string|null|Nombre que se le será asigando al objeto relacionado|

&nbsp;
___
&nbsp;

## JsonAppener
> Añade una porsición de código JSQL a otra

**[string]** = *private* function ( *string* $sJSQL, *string* $sExtra );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sJSQL**|string||Cadena JSQL actual|
|**$sExtra**|string||Nueva porción de código|

&nbsp;
___
&nbsp;

## JsqlParser
> Parsea una cadena JSQL utilizando el método **jsqlParser** del objeto de base de datos

**[string]** = *private* function ( *mixed* $mJSQL, *string* $EOL );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mJSQL**|mixed||Cadena JSQL o Array|
|**$EOL**|string|null|Caracter utilizado para el fin de línea|

&nbsp;
___
&nbsp;

## Logger
> Registra la salida de LOG de un método en los atributos **attr::log** y **attr::history**. Cuando **$sStatus** = NULL se reseteará el valor del atributo **attr::log**  

**[void]** = *private* function ( *string* $sStatus, *string* $aDetails );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sStatus**|string|null|Definición del estado|
|**$aDetails**|string|array|Detalles de la operación ejecutada|

&nbsp;
___
&nbsp;

## OwLog
> Genera un log sobre cada acción de escritura. El mismo es almacenado en la base datos en la tabla `**__ngl_owl_log__**` y pasado al método **nglOwl::Logger**.
> Los datos que conforman el log son:
> - **table** = nombre de la tabla afectada
> - **row** = identificador del registro afectado
> - **user** = id del usuario que ejecutó el cambio (valor **sysvar::UID**)
> - **action** = acción ejecutada: insert, update, suspend o delete
> - **date** = fecha y hora en la que se ejecutó el cambio (formato **Y-m-d H:i:s**)
> - **ip** = dirección IP desde la cual se ejecutó el cambio

**[void]** = *private* function ( *string* $sTable, *int* $nRow, *string* $sAction, *array* $aChangeLog );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTable**|string||Nombre de la tabla afectada|
|**$nRow**|int||Identificador del registro afectado|
|**$sAction**|string||Acción ejecutada: insert, update, suspend o delete|
|**$aChangeLog**|array|null|Array con los datos del registro previos a la modificación|

&nbsp;
___
&nbsp;

## UpdateData
> Ejecuta las actualizaciones enviadas por los métodos:
> - [delete](#delete)
> - [insert](#insert)
> - [suspend](#suspend)
> - [toggle](#toggle)
> - [unsuspend](#unsuspend)
> - [update](#update)

**[int]** = *private* function ( *mixed* $aArguments, *int* $nState );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aArguments**|mixed|array|Objeto o array asociativo con los nombres de las columnas y datos que se usará en los métodos de escritura. Este argumento no es válido para escrituras múltiples|
|**$nState**|int|1|Estado al que se cambiaran los registros:<ul><li>**0** = eliminado (en la base de datos setea `state` = NULL)</li><li>**1** = activo (en la base de datos setea `state` = 1)</li><li>**2** = suspendido (en la base de datos setea `state` = 0)</li></ul>|

&nbsp;
___
&nbsp;

## Validate
> Realiza la validación de datos por medio del objeto [validate](validate.md)

**[array]** = *private* function ( *array* $vData, *string* $sRules, *boolean* $bIgnoreDefault);  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$vData**|array||Datos a validar|
|**$sRules**|string||Conjunto de reglas para la validación|
|**$bIgnoreDefault**|boolean|false|Ignora el los valores por defecto de las reglas; por lo general cuando se trata de una actualización de datos|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2019 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
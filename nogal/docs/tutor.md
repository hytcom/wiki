# tutor
Los tutores son los encargados de realizar las tareas de *escritura* dentro del sistema, son los controladores del modelo MVC. En en ellos se declaran los procedimientos de escritura en base de datos, administración de archivos, envio de correos, etc.  
Este objeto no es en sí mismo un tutor, sino que es el padre del cual se desprenden los tutores. Todos los tutores declarados por el usuario deben ser un **extends** de este objeto.  
Los tutores pueden o no ser **bloqueables**, esto significa que sus métodos sólo funcionan cuando previamente se ha ejecutado el método **unlock**, evitando que puedan ser invocados desde la URL sin consentimiento. Luego, el tutor se vuelve a **bloquear** automáticamente.  
Dentro de los tutores de un proyecto es posible definir un tutor **master**, el cual realiza tareas comunes a más de un tutor, como por ejemplo, enviar e-mails.  
Cuando se declara un **tutor** se debe tener en cuenta que estos no aceptan métodos que no sean del tipo **protected** o **private**. Cualquier otro tipo de método será ignorado.  
Para aprender como definir un **tutor** consultar la guía [Definiendo Tutores](tutors.md)
___

## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|**$aAllowedMethods**|private|Listado de métodos permitidos. Los métodos public serán ignorados|
|**$bDebug**|protected|Activa/desactiva el modo **debug**|
|**$bLockable**|private|Determina si el **tutor** es **bloqueable**|
|**$bLocked**|private|Indica si el **tutor** está bloqueado|
|**$master**|protected|Objeto que alberga al principal tutor del proyecto|
|**$sMethodName**|private|Nombre del método a ejecutarse|
|**$sTutorName**|private|Nombre del tutor activo|
|**$tutor**|private|Objeto que alberga al tutor en ejecución|

&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[debugging](#debugging)|Indica si el tutor se encuentra en modo **debug**|
|[load](#load)|Carga un tutor en el objeto **tutor** y lo prepara para su ejecución|
|[run](#run)|Ejecuta un método del tutor cargado|
|Internos||
|[Alvin](#alvin)|Cuando la constante **NGL_ALVIN** se encuentra activa, verifica que la ejecución del tutor se haga desde un usuario logueado|
|[Debug](#debug)|Genera una salida de datos del tipo **debug**|
|[Lock](#lock)|Bloquea el tutor para que no pueda ser ejecutado|
|[Lockable](#lockable)|Define al tutor como **bloqueable** y lo bloquea|
|[MethodName](#methodname)|Establece el método activo|
|[Nulls](#nulls)|Utilizando [emptytonull](fn.md#emptytonull), establece como NULL los argumentos pasados por **$aNulls**|
|[TutorCaller](#tutorcaller)|Evita que un tutor pueda ser ejecutado sin pasar por el objeto **tutor**|
|[TutorName](#tutorname)|Nombre del tutor activo|
|[Unlock](#unlock)|Desbloquea el tutor para que no pueda ser ejecutado|

&nbsp;

## debugging
> Ni el desarrollador que está haciendo uso del tutor, ni el usuario final, pueden poner a un tutor en modo **debug**, esta facultad sólo la tiene el desarrollador del **tutor** desde adentro del mismo.  
> Este método indica si el tutor se encuentra o no en modo **debug**. Este dato es útil para manejar la salida de datos del mismo.

**[boolean]** =  *public* function ( );  

### Ejemplos
#### en caso de debug aborta el script
```php
$tutor = $ngl("tutor")->load("personal");
$response = $tutor->run("update", array(
	"id"=>"URI75pbceMb7ca85acLD7160Ze40Kb94", "hijos"=>3)
);

if($tutor->debugging()) { exit($response); }
```

&nbsp;
___
&nbsp;

## load
> Carga un tutor en el objeto **tutor** y lo prepara para su ejecución.

**[$this]** =  *public* function ( *string* $sTutorName, *mixed* $mArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTutorName**|string||Nombre del tutor|
|**$mArguments**|mixed|null|Variable pasada al método **init** del tutor cargado|

### Ejemplos
#### carga de un tutor
```php
$tutor = $ngl("tutor")->load("personal");
```

&nbsp;
___
&nbsp;

## run
> Ejecuta un método del tutor cargado.

**[mixed]** =  *public* function ( *string* $sMethod, *array* $aArguments );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sMethod**|string||Nombre del método que se desea ejecutar del tutor cargado|
|**$aArguments**|array|null|Argumentos pasados al método|

### Ejemplos
#### actualizacion de datos
```php
$tutor = $ngl("tutor")->load("personal");
$response = $tutor->run("update", array(
	"id"=>"URI75pbceMb7ca85acLD7160Ze40Kb94", "hijos"=>3)
);
```
#### autenticación de usuario
```php
$tutor = $ngl("tutor")->load("users");
$response = $tutor->run("login", array(
	"username"=>$_POST["user"], "passwrod"=>$_POST["pass"])
);
```

&nbsp;
___
&nbsp;

# Internos
Para una mejor comprensión de estos métodos ver, [Definiendo Tutores](tutors.md)

## Alvin
> Cuando la constante **NGL_ALVIN** se encuentra activa, verifica que la ejecución del tutor se haga desde un usuario logueado.  

**[$this]** =  *public* function ( );  

&nbsp;
___
&nbsp;

## Debug
> Efectua un DUMP de las variables pasadas, junto con el **REQUEST_METHOD**, el nombre del tutor y el **método** ejecutado.

**[string]** =  *public* function ( *mixed* $mVar1, ..., *mixed* $mVarN );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mVar1**|mixed||Una o mas variables|

&nbsp;
___
&nbsp;

## Lock
> Bloquea el tutor para que no pueda ser ejecutado. Para rehabilitar la ejecución deberá ejecutarse [Unlock](#unlock)  

**[$this]** =  *public* function ( );  

&nbsp;
___
&nbsp;

## Lockable
> Define al tutor como **bloqueable** y lo bloquea. Un tutor bloqueado no permite la ejecución de sus métodos, y como los métodos para bloquear y desbloquer son del tipo **protected**, el único ambiente posible para su uso es el privado. Esto es especialmente útil en tutores que ejecutan acciones importantes como enviar correos, ya que solo puede invocarse al método que realiza en envío desde otro tutor.

**[$this]** =  *public* function ( );  

&nbsp;
___
&nbsp;

## MethodName
> Establece el método activo  

**[$this]** =  *public* function ( *string* $sName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sName**|string||Nombre del método|

&nbsp;
___
&nbsp;

## Nulls
> Utilizando [emptytonull](fn.md#emptytonull), establece como NULL los valores de **$aData**, cuyo indice se encuentre en **$aNulls** y retornen TRUE a la funcion **empty**

**[$this]** =  *public* function ( *array* $aData, *array* $aNulls );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aData**|array||Array de datos|
|**$aNulls**|array||Array con los nombres de las claves del array $aData|

&nbsp;
___
&nbsp;

## TutorCaller
> Invocarla evita que un tutor pueda ser ejecutado sin pasar por el objeto **tutor**  

**[void]** =  *public* function ( *string* $sCaller );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sCaller**|mixed||**self::requirer()**|
### Ejemplos
#### actualizacion de datos
```php
namespace nogal;
$this->TutorCaller(self::requirer());

class tutorName extends nglTutor {
	.
	.
	.
}
```

&nbsp;
___
&nbsp;

## MethodName
> Establece el tutor activo  

**[$this]** =  *public* function ( *string* $sName );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sName**|string||Nombre del tutor|

&nbsp;
___
&nbsp;

## Unlock
> Desbloquea el tutor para que pueda ser ejecutado. 

**[$this]** =  *public* function ( );  

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
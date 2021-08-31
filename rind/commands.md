# Rind Commands

[Conceptos y Fundamentos](#Conceptos-y-Fundamentos)  
[Comandos](#Comandos)  
[Normalización de Datos](#Normalizacion-de-Datos)  


# Conceptos y Fundamentos
Rind es el sistema de plantillas de NOGAL. Esta compuesto por 17 comandos que permiten realizar todo tipo de operaciones en el ambito HTML.
Las plantillas (documentos HTML) son procesados por Rind y cacheados hasta que sufran algún cambio en su estructura, ahorrando asi tiempo de procesamiento.

A continación enumeramos los conceptos básicos del sistema, como el uso de variables, comandos y estructuras de datos.

&nbsp;

## Variables/Constantes  
```php
{$foo}    variables PHP
{@foo}    constantes PHP permitidas
{#foo}    referencias a los valores internos de cada vuelta de un LOOP
{foo}     referencia a un campo del objeto DATA dentro de un LOOP
{*foo}    referencia a variables _SET dentro del comando HEREDOC o argumentos JSON (<@value json> ... </@value>)
{%foo%}    variables pasadas al archivo de destino en el comando MERGEFILE
{:foo:}    reservada para usos multiples
```

## Variables  
```php
# Para imprimir una variable proveniente del entorno PHP, se deberá citar el misma entre llaves.
# Existen dos formatos en los cuales el sistema entiende las variables:
#     1) accediendo a los indices con corchetes como en PHP
#     2) accediendo a los indices con puntos como en Javascript

# PHP
$foo = "lorem ipsum"; 
$bar = array(); 
$bar["foo"] = "lorem ipsum"; 
$bar["bar"]["foo"] = "lorem ipsum"; 
$foobar = new stdClass();
$foobar->bar = "lorem ipsum"; 
$foobar->foo["bar"] = "lorem ipsum"; 

# HTML
## Sintáxis tradicional
{$foo}
{$bar["foo"]}
{$bar["bar"]["foo"]}
{$foobar->bar}
{$foobar->foo["bar"]}

## Sintáxis Rind
{$foo}
{$bar.foo}
{$bar.bar.foo}
{$foobar->bar}
{$foobar->foo.bar}

# Las variables permitidas y denegas son configuradas en el objeto por medio de los
# argumentos: vars_deny, vars_allow 
# Por otro lado, las varibales $GLOBALS y $ngl estarán SIEMPRE denegadas
```

## Variables Anidadas  
```php
# A la hora de utilizar una variable como indice de otra se pueden utilizar la sintáxis
# tradicional, la Rind ó la combinación de ambas.
# En lo que se refiere a la sintáxis Rind, se utiliza un unico par de llaves {}, 
# el resto de los que sean necesarios deberán ser reemplazados por parentesis ()

# PHP
$colors = array();
$colors["black"] = array("R"=>"00", "G"=>"00", "B"=>"0");
$colors["blue"] = array("R"=>"00", "G"=>"00", "B"=>"FF");
$colors["green"] = array("R"=>"00", "G"=>"FF", "B"=>"0");
$colors["orange"] = array("R"=>"FF", "G"=>"99", "B"=>"00");
$colors["red"] = array("R"=>"FF", "G"=>"00", "B"=>"00");
$colors["white"] = array("R"=>"FF", "G"=>"FF", "B"=>"FF");
$colors["yellow"] = array("R"=>"FF", "G"=>"FF", "B"=>"00");

$preferences = array();
$preferences["text"] = "red";
$preferences["back"] = "yellow";

$current = "text";

# HTML
## Sintáxis tradicional
{$preferences[{$current}]}
{$colors[{$preferences["back"]}]["R"]}
{$colors[{$preferences[{$current}]}]["B"]}

## Sintáxis Rind
{$preferences.($current)}
{$colors.($preferences.back).R}
{$colors.($preferences.($current)).B}

## Sintáxis Mixta
{$colors[{$preferences.back}]["R"]}
{$colors[{$preferences.($current)}]["B"]}
```

En caso de que querramos utilizar un texto que represente una variable, y que éste no sea reemplazado por su valor, debemos utilizar llaves dobles a modo de escape. Esto mismo aplica para las palabras claves dentro de los loops.

``` html
{$foo} = imprime el valor de la variable $foo<br />
{{$foo}} = imprime el texto literal {$foo}
```

## Uso de Comandos
Todos los comandos respetan la sintaxis
```php
<rind:COMANDO>
    <@parametro_1>...</@parametro_1>
    <@parametro_2>...</@parametro_2>
    ...
    <@parametro_N>...</@parametro_N>
</rind:COMANDO>
```

o para comandos en una línea
```php
<rind:COMANDO />
```

Los parámetros son pasados por medio de las etiquetas <@...>, por ejemplo:
```php
<@name>...</@name>
<@value>...</@value>
<@type>...</@type>
<@source>...</@source>
```

También es posible ejecutar el comando con un solo parámetro sin especificar el mismo, en estos casos se asume que el valor pasado corresponde al parámetro **&lt;@content&gt;**
```php
<rind:dump><@content>{$datos}<@content></rind:dump>
<rind:dump>{$datos}</rind:dump>
```
ambas expresiones son equivalentes
&nbsp;

### Existen 3 tipos de comandos soportados en el sistema:

Comando nativos, son los explicados en este documento
```php
<rind:dump>{$datos}</rind:dump>
```

Comandos PHP, son declarados en el argumento php_functions del objeto Rind.
En estos comandos no tienen importancia el nombre de los parámetros, sino su orden.
```php
<rind:php.str_repeat>
    <@string>lorem</@string>
    <@count>lorem</@count>
</rind:php.str_repeat>
```

Nuts, son los definidos por el programador PHP para cada proyecto
```php
<rind:nut.datetime.format>
    <@format>Y-m-d</@format>
    <@date>{$date}</@date>
</rind:nut.datetime.format>
```

**!!!! IMPORTANTE !!!!**  
Rind asume que los argumentos podrán ser entrecomillados con comillas dobles, esto debe tenerse en cuenta a la hora de pasar los argumentos. En el caso de que el argumento utilice comillas dobles, estas deberán ser escapadas o el parámetro deberá ser tratado con los métodos de normalización **base64** o **json**

## Comandos Anidados  
Al anidar comandos es conveniente declarar todos los parámetros para evitar una lectura erronea:
Este eco es correcto porque hay un unico y posible parámetro **&lt;@content&gt;**
```php
<rind:eco>{$date}</rind:eco>
```

Sin embargo en el siguiente ejemplo Rind ignora todas las etiquetas dentro de eco
hasta llegar al **&lt;@content&gt;** del datetime, al cual intrepreta como el **&lt;@content&gt;** del eco
```php
<rind:eco>
    <rind:nut.datetime>
        <@format>Y-m-d</@format>
        <@content>{$date}</@content>
    </rind:nut.datetime>
</rind:eco>
```
Por lo tanto hay que declarar el comando de la siguiente manera
```php
<rind:eco>
    <@content>
        <rind:nut.datetime>
            <@format>Y-m-d</@format>
            <@content>{$date}</@content>
        </rind:nut.datetime>
    </@content>
</rind:eco>
```

## Nuts  
Los nuts son extenciones del objeto nglNut declarados por el programador.
Su implementación es idéntica a la de los comandos nativos o los comandos de PHP, salvo porque:
- es importante respetar el nombre de los parámetros
- no importa el orden en el que sean enviados
- cuantan con SAFEMODE, por lo que el programador PHP puede limitar la ejecución de comandos
  

Para llamar un nut se utiliza las siguiente sintaxis
```php
<rind:nut.NOMBRENUT.NOMBRECOMANDO>
    <@NUTID>...</@NUTID>
    <@parametro_1>...</@parametro_1>
    <@parametro_2>...</@parametro_2>
    ...
    <@parametro_N>...</@parametro_N>
</rind:nut.NOMBRENUT.NOMBRECOMANDO>
```
o esta otra, para cuando el nombre del COMANDO sea igual al nombre del NUT
```php
<rind:nut.NOMBRENUT>
    <@parametro_1>...</@parametro_1>
    <@parametro_2>...</@parametro_2>
    ...
    <@parametro_N>...</@parametro_N>
</rind:nut.NOMBRENUT>
```
@NUTID es un parámetro opcional, que es util cuando se desea llamar a la misma instancia de un nut, por ej:
obtiene datos por medio de un objeto OWL
```php
<rind:set>
    <@name>data</@name>
    <@value>
        <rind:nut.owl.getall>
            <@nutid>mynut</@nutid>
            <@object>some_table</@object>
            <@state>1</@state>
        </rind:nut.owl.getall>
    </@value>
    <@method>object</@method>
</rind:set>
```
retorna la ultima consulta SQL ejecutada por el objeto OWL
```php
<rind:nut.owl.query><@nutid>mynut</@nutid></rind:nut.owl.query>
```

## Nuts - Ejemplo de anidamiento con loop  
```php
<select name="parent">
    <option value="0" selected>- principal -</option>

    <rind:loop> # impresión de datos
        <@name>categories</@name>
        <@source>

            <rind:nut.tree.paths> # nut que genera los paths de categorias
                <@content>
                    <rind:nut.owl.getall> # salida de los datos en formato array

                        <rind:nut.owl.roll> # obteción de datos según el estado
                            <@object>categories</@object>
                            <@state>1</@state>
                            <@order>name:ASC</@order>
                        </rind:nut.owl.roll>

                    </rind:nut.owl.getall> 
                </@content>
                <@column>name</@column>
                <@separator> / </@separator>
            </rind:nut.tree.paths>

        </@source>
        <@type>array</@type>
        <@content>
            <option value="{$_SET.categories.key}">{categories.0}</option>
        </@content>
    </rind:loop>

</select>
```

## Nuts - Ejemplo de anidamiento con loop con filtro  
```php
<select name="parent">
    <option value="0" selected>- principal -</option>

    <rind:loop> # impresión de datos
        <@name>categories</@name>
        <@source>

            <rind:nut.tree.paths> # nut que genera los paths de categorias
                <@content>
                    <rind:nut.owl.getall> # salida de los datos en formato array

                        <rind:nut.owl.roll> # obteción de datos según el filtro
                            <@object>categories</@object>
                            <@filter json>{"where":[["categories.imya","eq","({$_GET.imya})"]]}</@filter>
                            <@order>name:ASC</@order>
                        </rind:nut.owl.roll>

                    </rind:nut.owl.getall> 
                </@content>
                <@column>name</@column>
                <@separator> / </@separator>
            </rind:nut.tree.paths>

        </@source>
        <@type>array</@type>
        <@content>
            <option value="{$_SET.categories.key}">{categories.0}</option>
        </@content>
    </rind:loop>

</select>
```

## Object  
Los objects son objetos declarados en PHP por el programador.
Su implementación es idéntica a la de los nuts, pero tanto el orden como el nombre de los parámetros dependerá del objeto.

Para llamar un object se utiliza las siguiente sintaxis
```php
<rind:object.NOMBREOBJECT.NOMBREMETODO>
    <@parametro_1>...</@parametro_1>
    <@parametro_2>...</@parametro_2>
    ...
    <@parametro_N>...</@parametro_N>
</rind:object.NOMBREOBJECT.NOMBREMETODO>
```

Ejemplo con un puntero de DB leído por un Objeto,
Donde el comando asarray retorna el resultado del puntero como un Array
```php
<table width="100%">
    <thead>
        <tr>
            <td width="15%">Código</td>
            <td>Producto</td>
            <td width="15%">Precio</td>
        </tr>
    </thead>
    <tbody>
        <rind:loop>
            <@name>ncm</@name>
            <@source>
                <rind:object.db.asarray>
                    <@handler>{$products}</@handler>
                </rind:object.db.asarray>
            </@source>
            <@type>array</@type>
            <@content>
                <tr>
                    <td width="15%">{code}</td>
                    <td>{product}</td>
                    <td width="15%">{price}</td>
                </tr>
            </@content>
        </rind:loop>
    </tbody>
</table>
```

&nbsp;
___
&nbsp;

# Comandos
|Comando|Descripción|
|---|---|
|[abort](#abort)|Disponible sólo dentro del comando RindCommands::loop. Termina con la ejecución ...|
|[alvin](#alvin)|Ejecuta una evaluación de permisos a traves de la clase nogal::nglAlvinCuenta co...|
|[comments](comments)|Rind cuenta con una manera de añadir comentarios al código fuente.Los comentario...|
|[dump](#dump)|Vuelca en pantalla el contenido completo de una variable. Esto es especialmente ...|
|[eco](#eco)|Imprime una expresión o resultado de otro comando como datetime, math o length.|
|[halt](#halt)|Aborta la ejecución de todas las acciones. Opcionalmente, al ejecutar RindComman...|
|[ifcase](#ifcase)|Evalúa una condición como verdadera o falsa. En caso de serverdadera se comporta...|
|[incfile](#incfile)|Incluye un archivo dentro de otro durante el proceso de datos. Este comando es e...|
|[length](#length)|Intenta retorna el largo de una variable, cantidad de elementos de un Array una ...|
|[loop](#loop)|Este comando puede ser utilizado para recorrer todo tipo de bucles, ya sean array...|
|[mergefile](#mergefile)|Incluye un archivo dentro de otro previo al proceso de datos. Esto significa que...|
|[once](#once)|Retorna un código [ONCECODE](../../nogal/docs/fn.md#once)|
|[rtn](#rtn)|ESTE ES UN METODO DE NORMALIZACION DE DATOS Por razones de seguridad todas las ex...|
|[set](#set)|Este comando permite setear variables en el ambito de las plantillas.Todos los va...|
|[skip](#skip)|Aborta la ejecución de la actual vuelta y continúa con la siguiente. Disponible ...|
|[unique](#unique)|Genera un [código único](../../nogal/docs/fn.md#unique)|
|[unset](#unset)|Permite desetear variables seteadas por medio del comando RindCommands::set. Los ...|

&nbsp;
  
## alvin
Ejecuta una evaluación de permisos a traves de la clase **nogal::nglAlvin**
Cuenta con tres modos de ejecución:
- **Respuesta booleana** = para ser utilizado como condición de **RindCommands::ifcase**
- **Autorización contenidos** = en caso de una respuesta afirmativa autorizará la carga del contenido encerrado en el bloque
- **Retorno de valores RAW** = retorna una cadena o array de valores **RAW**

En ambos casos, los permisos a ser evaluados deben estar encerrados entre paréntesis y ubicados en la misma línea que el inicio del bloquee **RindCommands::alvin**. 
Al menos que los permisos a evaluar comiencen con "?|" (sin comillas) **RindCommands::alvin** retornará TRUE sólo si TODOS los permisos son válidos. 
Para cuando se quiera evaluar la inexistencia de uno ó mas permisos se deberá utilizar "!|" (sin comillas)  
- Si la cadena de permisos esta vacia o su sintáxis es incorrecta, el comando devolverá **false**
- Si la cadena de permisos es igual a **true** ó **1**, el comando devolverá **true**
- Si la cadena de permisos es igual a **RAW**, el comando retornará un valor RAW valiendose de los parámetros en **rawindex** y **rawdada**. Si los valores **RAW** tuvieran palabras claves con el formato **{:keyword:}**, estas serán reemplazadas con los valores pasados por medio de **rawdata**.
- Para el nombre de usuario **admin** todos los permisos serán ignorados.

Para que **RindCommands::alvin** pueda ejecutarse, el TOKEN de permisos deberá está almacenado en la variable:
**$_SESSION[NGL_SESSION_INDEX]["LOGIN"]["ALVIN_TOKEN"]**  

La política de evaluación está condicionada por el argumento **alvin_mode** del objeto **rind**
- all: todas las validaciones están activas
- none: se ignoran todas las validaciones
- users: se ignoran todas las validaciones para los usuarios declarados. Cadena separada por ,


|Parámetro|Descripción|
|---|---|
|**content**|Cadena de permisos a evaluar.|
|**rawindex**|Cadena de indices RAW; si esta contiene . (puntos), será explotada por ellos y se navegará el resultado en los indices de **RAW** hasta retornar y valor. Si no se especificase un valor para **rawindex**, se retornará el array completo de valores **RAW**|
|**rawdata**|Valores en formato *JSON* con los que se reemplazarán las palabras claves de los valores retornados.|


### Ejemplos  
#### Respuesta Booleana  
```php
/* concede acceso al perfil que tenga los dos permisos */
<rind:ifcase>
    <@iff><rind:alvin>(?|sales.view,sales.add)</rind:alvin></@iff>
    <@then>PUEDE VERME</@then>
    <@else>NO PUEDE VERME</@else>
</rind:ifcase>
```

```php
/* concede acceso a los perfiles que tengan cualquiera de los 2 permisos */
<rind:ifcase>
    <@iff><rind:alvin>(?|sales.view,buy.view)</rind:alvin></@iff>
    <@then>PUEDE VERME</@then>
    <@else>NO PUEDE VERME</@else>
</rind:ifcase>
```

```php
/* concede acceso a los perfiles que no tengan el permiso */
<rind:ifcase>
    <@iff><rind:alvin>(!|sales.report)</rind:alvin></@iff>
    <@then>PUEDE VERME</@then>
    <@else>NO PUEDE VERME</@else>
</rind:ifcase>
```

```php
/* array de valores RAW */
<rind:set>
	<@name>raw</@name>
	<@value><rind:alvin>RAW</rind:alvin></@value>
</rind:set>
<rind:dump>{$_SET.raw}</rind:dump>
```

```php
/* valor RAW con reemplazo de keyword */
<rind:set>
	<@name>filter</@name>
	<@value>
		<rind:alvin>
			<@content>RAW</@content>
			<@rawindex>jsql.empleados</@rawindex>
			<@rawdata json>{"userid":"{$ENV.alvin.id}"}</@rawdata>
		</rind:alvin>
	</@value>
</rind:set>
```

#### Autoriza contenidos  
```php
<rind:alvin>(sales,buy)
    <rind:loop>
        <@name>count</@name>
        <@type>number</@type>
        <@from>1</@from>
        <@limit>10</@limit>
        <@zerofill>2</@zerofill>
        <@content>
            {#numrow}
        </@content>
    </rind:loop>
</rind:alvin>
```

&nbsp;
___
&nbsp;

## abort
Disponible sólo dentro del comando **RindCommands::loop**. Termina con la ejecución del mismo y continua con el resto de la plantilla.
Este es un comando simple (inline), que no posee parámetros.  

 *inline* function ( );
  
### Ejemplos  
#### example  
```php
# Datos
Supongamos un array que contiene todos los saldos de los clientes de una empresa ordenados 
por su deuda de mayor a menor. Para mostrar sólo los que tienen deuda mayor a $1.000 podríamos usar.

# Comando
<rind:loop>
    <@name>accounts</@name>
    <@source>{$aAccounts}</@source>
    <@content>
        <rind:ifcase>
            <@iff>{balance}<=1000</@iff>
            <@then><rind:abort /></@then>
        </rind:ifcase>
        
        {customer} debe {balance}
        
    </@content>
</rind:loop>
```

&nbsp;
___
&nbsp;

## dump
Vuelca en pantalla el contenido completo de una variable. Esto es especialmente util en los casos de arrays asociativos y punteros de MySQL en los cuales se desconocen los nombres de sus indices.
Ante los diferentes tipos de variables **dump** mostrará:<ul><li>**Array** = Contenidos completo del mismo.</li><li>**Objecto del tipo iNglStdObject** = Obtendrá los datos del primer registro de datos y lo retornará como un **array**.</li><li>**Otras Variables** = Simplemete se mostrará el contenido de las mismas.</li></ul>  


|Parámetro|Descripción|
|---|---|
|**content**|Variable o palabra clave.|
|**die**|Cuando es true, después de mostrar los contenidos en pantalla se abortará la ejecución.|
### Ejemplos  
#### Variable  
```php
<rind:dump>{$stats}</rind:dump>
```

&nbsp;
___
&nbsp;

## eco
Imprime una expresión o resultado de otro comando como **datetime**, **math** o **length**.  


|Parámetro|Descripción|
|---|---|
|**content**|Expresión.|
### Ejemplos  
#### example  
```php
# Datos
Para $nDiscount = 6;

# Comando
Oferta!
1 x <b>$5</b>
10 x <b><rind:eco>(5*10)-{$discount}</rind:eco></b>

# Resultado
Oferta!
1 x <b>$5</b>
10 x <b>$44</b>
```
#### example1  
```php
<rind:eco><rind:length>{$products}</rind:length></rind:eco>
```

&nbsp;
___
&nbsp;

## halt
Aborta la ejecución de todas las acciones. Opcionalmente, al ejecutar **RindCommands::halt**, se puede mostrar un mensaje en pantalla.  


|Parámetro|Descripción|
|---|---|
|**content**|Mensaje que se mostrará en pantalla|
|**url**|En caso de proporcionarse, el comando abortará la carga y redirigirá a esta URL|
### Ejemplos  
#### if con exit
```php
<rind:ifcase>
    <@iff>{$sIP}!="192.168.0.1"</@iff>
    <@then>
        <rind:halt>Acceso Denengado!</rind:halt>
    </@then>
</rind:ifcase>
```

#### redirección
```php
<rind:halt><@url>https://hytcom.net</@url></rind:halt>
```

&nbsp;
___
&nbsp;

## ifcase
Evalúa una condición como verdadera o falsa. En caso de ser	verdadera se comportará según **then**, de lo contrario lo hará según las directivas de **else**.
Existe la posibilidad de utilizar **ifcase** sin los parámetros **iff**, **isset**, **in** y **notin** en cuyo caso lo que se evaluará será que el parámetro **then**, siempre que no esté vacío.
También puede utilizarse **content** en lugar de **then**

**RindCommands::ifcase** también soporta el modo abreviado:  
**&lt;rind:ifcase&gt;**( expr )sentencias **&lt;/rind:ifcase&gt;**  
**&lt;rind:ifcase&gt;**( **?** expr )sentencias **&lt;/rind:ifcase&gt;** (para evaluar isset)  
**&lt;rind:ifcase&gt;**( **#** expr )sentencias **&lt;/rind:ifcase&gt;** (para evaluar count/strlen)  
**&lt;rind:ifcase&gt;**( **-** expr )sentencias **&lt;/rind:ifcase&gt;** (retorna **false** cuando la condicion ó su resultado, son vacios)  
**&lt;rind:ifcase&gt;**( **+** expr )sentencias **&lt;/rind:ifcase&gt;** (retorna **true** cuando la condicion ó su resultado, son vacios)

donde se evaluará la expresión entre paréntesis y en caso de TRUE se ejecutarán las sentencias.
La expresión entre paréntesis debe estar en la misma línea que el inicio del bloquee **ifcase**, esto no es necesario en el caso de las sentencias.** Ver ejemplos
Los **RindCommands::ifcase** en modo abreviado **NO PUEDEN SER ANIDADOS**

Los modos **iff**, **isset** e **in/notin** pueden utilizarse simultaneamente, sin embargo no se pueden utilizar **in** y **notin** dentro del mismo **&lt;rind:ifcase&gt;**.

También existe un modo en el que se utiliza **ifcase** como una estructura **switch**, para ello se deberá definir un identificador (**ID**) para cada parámetro de condición, y este deberá matchear con [a-zA-Z0-9_]+<ul><li>**iff:<u>ID</u>**&nbsp;</li><li>**isset:<u>ID</u>**&nbsp;</li><li>**in:<u>ID</u>**&nbsp;</li><li>**notin:<u>ID</u>**&nbsp;</li></ul>Todos los parámetros que compartan el mismo **ID** será evaluados en conjunto y ejecutaran las sentencias del parámetro **then:<u>ID</u>**  


|Parámetro|Descripción|
|---|---|
|**iff**|Condición que se evaluará como TRUE o FALSE.|
|**then**|Acciones que se ejecutarán o contenidos que se mostrarán cuando la evaluación del comando retorne TRUE.|
|**else**|Acciones que se ejecutarán o contenidos que se mostrarán cuando la evaluación del comando retorne FALSE.|
|**isset**|Espera una variable o grupo de ellas y evalúa si estas existen, es decir, si han sido previamente seteada. También es posible evaluar si la variable NO existe, anteponiendo el signo de admiración (!) al nombre de la misma. Este parámetro acepta ser combinado con **iff**, esto significa que para que **RindCommands::ifcase** retorne TRUE, ambas evaluaciones deberán ser TRUE.|
|**empty**|Devuelve TRUE cuando el contenido del argumento es vacío: 0, null ó cadena vacía|
|**noempty**|Devuelve TRUE cuando el contenido del argumento es distinto de vacío|
|**in**|Espera una lista de valores en la que buscará el valor **needle**. Esta lista puede ser un array o una cadena separada por un caracter especifico (**splitter**). Devuelve TRUE si **needle** existe en **in**. |
|**notin**|La inversa de **in**; espera que **needle** no se encuentre en **notin**.|
|**needle**|Valor a buscar cuando se utiliza **in** o **notin**.|
|**setmode**|Si **setmode** está activado el retorno de **RindCommands::ifcase** estará optimizado para ser utilizado como **value** del comando **RindCommands::set**.|
|**splitter**| En el caso de que **in** o **notin** no fuesen un array, este valor se usará como separador de la cadena de valores. El valor predeterminado es punto y coma ( ; ). |
|**content**|Idem **then**|
### Ejemplos  
#### Ejecución de ifcase  
```php
<rind:ifcase>
    <@iff>{$gender}=="M"</@iff>
    <@then>Sr.</@then>
    <@else>Sra.</@else>
</rind:ifcase>
```
#### Doble chequeo  
```php
<script language="JavaScript" type="text/javascript">
    <rind:ifcase>
        <@iff>{$nAge}<18 && {$bAdultContents}</@iff>
        <@then>self.location.href = "http://www.disney.com";</@then>
    </rind:ifcase>
</script>
```
#### Chequeo de Variable  
```php
# Verifica que una variable exista y no este vacia o sea 0 o null.
# Mostrará una dirección de correo sólo en caso de que exista.
<rind:ifcase>{$sEmail}</rind:ifcase>
```
#### In / NotIn  
```php
# IN
<rind:ifcase>
    <@in>lunes;martes;miercoles;jueves;viernes</@in>
    <@needle>{$sDay}</@needle>
    <@then>
        {$sDay} es un día hábil
    </@then>
</rind:ifcase>

# NOTIN
<rind:ifcase>
    <@notin>lunes;martes;miercoles;jueves;viernes</@notin>
    <@needle>{$sDay}</@needle>
    <@then>
        {$sDay} es fin de semana
    </@then>
</rind:ifcase>
```
#### Isset  
```php
# ISSET
<rind:ifcase>
    <@isset>{$bIsAdult}</@isset>
    <@then>Es mayor de edad</@then>
</rind:ifcase>

# ISSET Multiple
<rind:ifcase>
    <@isset>({$bIsChild} && {$bHaveAuth}) || {$bIsAdult}</@isset>
    <@then>Es menor y tiene autorización o es mayor de edad</@then>
</rind:ifcase>

# ISSET e IFF
<script language="JavaScript" type="text/javascript">
    <rind:ifcase>
        <@iff>{$nAge}<18</@iff>
        <@isset>{$bAdultContents}</@isset>
        <@then>self.location.href = "http://www.disney.com";</@then>
    </rind:ifcase>
</script>
```
#### Switch  
```php
# IF
<rind:ifcase>
    <@iff:a>{$nPosition}==1</@iff:a>
    <@then:a>Primero</@then:a>

    <@iff:b>{$nPosition}==2</@iff:b>
    <@then:b>Segundo</@then:b>

    <@iff:c>{$nPosition}==3</@iff:c>
    <@then:c>Tercero</@then:c>

    <@else>Otro</@else>
</rind:ifcase>

# IF e ISSET
<rind:ifcase>
    <@iseet:a>{$bShowScore}</@iseet:a>
    <@iff:a>{$nPosition}==1</@iff:a>
    <@then:a>Primero</@then:a>

    <@iseet:b>{$bShowScore}</@iseet:b>
    <@iff:b>{$nPosition}==2</@iff:b>
    <@then:b>Segundo</@then:b>

    <@iseet:c>{$bShowScore}</@iseet:c>
    <@in:c>3;4;5;6</@in:c>
    <@needle:c>{$nPosition}</@needle:c>
    <@then:c>Otro</@then:c>

    <@iseet:d>{bShowScore} && !{$nPosition}</@iseet:d>
    <@then:d>Falta definir $nPosition</@then:d>
</rind:ifcase>
```
#### Modo abreviado  
```php
# Sintaxis Correctas
<rind:ifcase>({$nAge}>18)Es mayor de edad</rind:ifcase>

<rind:ifcase>({$nAge}>18)
    Es mayor de edad
</rind:ifcase>


# Sintaxis Incorrectas
<rind:ifcase>
    ({$nAge}>18)
    Es mayor de edad
</rind:ifcase>

<rind:ifcase>(
        {$nAge}>18 AND
        {$nAge}<72
    )
    Debe emitir voto
</rind:ifcase>

# Isset
<rind:ifcase>(?{$bIsAdult})Es mayor de edad</rind:ifcase>

# Isset (negado)
<rind:ifcase>(?!{$bIsAdult})Es menor de edad</rind:ifcase>

# Count/Strlen
<rind:ifcase>(#{$sName})Saludos amigo</rind:ifcase>

# Count/Strlen (negado)
<rind:ifcase>(#!{$sName})Saludos desconocido</rind:ifcase>
```
#### Set Mode  
```php
# Para combinar con el comando RindCommands::set
<rind:set>
    <@name>puesto</@name>
    <@value>
        <rind:ifcase>
            <@setmode>1</@setmode>

            <@iff:a>{$nPosition}==1</@iff:a>
            <@then:a>Primero</@then:a>

            <@iff:b>{$nPosition}==2</@iff:b>
            <@then:b>Segundo</@then:b>

            <@iff:c>{$nPosition}==3</@iff:c>
            <@then:c>Tercero</@then:c>

            <@else>Otro</@else>
        </rind:ifcase>
    </@value>
</rind:set>
```

&nbsp;
___
&nbsp;

## incfile
Incluye un archivo dentro de otro durante el proceso de datos. Este comando es equivalente al **include** de **PHP**.
A diferencia de **include**, **RindCommands::incfile** trabaja con un array denominado **$_SET.INCLUDES**, un índice de archivos que evita inclusiones no contempladas por el desarrollador.
Este array es declarado en el archivo **ngl/conf/rind.conf**  


|Parámetro|Descripción|
|---|---|
|**content**|ID del archivo a incluir en el indice **\$_SET.INCLUDES**.|
### Ejemplos  
#### Inclusion  
```php
# Datos
[includes]
menu   = "home/files/menu.php";
index  = "home/modules/index.php";
libs   = "home/modules/inc/libs.php";

# Comando
<rind:incfile>menu</rind:incfile>

# Resultado
Incluirá el archivo "home/files/menu.php"
```

&nbsp;
___
&nbsp;

## length
Intenta retorna el largo de una variable, cantidad de elementos de un Array una cadena de texto.
Este comando retorna un resultado del tipo return.

Los resultados para los diferentes tipos de variables son:<ul><li>**Array** = Cantidad de elementos del mismo.</li><li>**Object** = Cantidad de elementos del mismo, cuando sea del tipo Countable.</li><li>**Cadena de Texto** = Cantidad de caracteres.</li></ul>  


|Parámetro|Descripción|
|---|---|
|**content**|Variable.|
### Ejemplos  
#### example  
```php
<rind:length>{$aProductos}</rind:length>
```

&nbsp;
___
&nbsp;

## loop
Este comando puede ser utilizado para recorrer todo tipo de bucles, ya sean arrays, árboles, recursos de base de datos, xml o simplemente una sucesión númerica.
**RindCommands::loop** dará una vuelta por cada índice, fila o incremento que encuentre en el origen **data**.
Cuando **RindCommands::loop** recorre un bucle almacena los datos recogidos en un índice (**name**) de la variable **SET**.

Indicadores disponibles en cada vuelta:<ul><li>**data** = datos recogidos en la vuelta actual</li><li>**current** = primer elemento de los datos de la vuelta actual, en ocasiones puede coincidir con el valor de **data**</li><li>**first** = retorna **1** sólo en la primer vuelta</li><li>**from** = inicio del loop (comienza en 0)</li><li>**key** = nombre de la clave activa (solo en arrays)</li><li>**numrows** = número total de vueltas</li><li>**last** = retorna **1** sólo en la última vuelta</li><li>**limit** = cantidad de vueltas del loop</li><li>**line** = número de la vuelta actual, comenzando por el 1</li><li>**lines** = alias de **numrows**</li><li>**numrow** = número de la vuelta actual, comenzando por el 0</li><li>**odd** = retorna **0** si la vuelta es par y **1** si es impar</li><li>**parity** = retorna **even** si la vuelta es par y **odd** si es impar</li><li>**previous** = datos recogidos en la vuelta anterior, en la primer vuelta retorna NULL</li></ul>Estos datos podrán ser llamados utilizando las sintáxis:
- dentro de un Loop principal  
**{#indicador}** por ejemplo: **{#numrow}**  

- con Loops anidados  
**{nombre_del_bucle.#indicador} ó {nombre_del_bucle:#indicador}** por ejemplo: **{foobar.#numrow}**  

- forma explícita  
**{\$_SET.nombre_del_bucle.indicador}** por ejemplo: **{$_SET.foobar.numrow}**  

Sin importar el **type** del bucle, todos los datos almacenados en el índice **data** serán tratados como un array asociativo, y para acceder a ellos en cada vuelta sólo bastará con colocar el nombre del sub-índice entre llaves: **{nombre_subindice}**.

Tambien serán aceptados como formatos de llamada, sobre todo para los casos en los cuales se produzca anidamiento de bucles con nombres de índices iguales:<ul><li>**{nombre_bucle.subindice}**&nbsp;</li><li>**{nombre_bucle:subindice}**&nbsp;</li><li>**{\$_SET.nombre_bucle.data.subindice}**&nbsp;</li><li>**{$_SET["nombre_bucle"]["data"]["subindice"]}**&nbsp;</li></ul>  


|Parámetro|Descripción|
|---|---|
|**name**|Nombre del **RindCommands::loop** y por consiguiente del indice de la variable **RindCommands::set**. Este parámetro no es obligatorio, pero se aconseja utilizarlo.|
|**source**|Origen de datos del **RindCommands::loop**. Este puede ser una variable previamente seteada en PHP, una variable **RindCommands::set**, la salida de un **nglNut**, un path o URL de un archivo o simplemente texto plano.|
|**type**|Tipo de bucle, esto determina que tratamiento se le deberá dar a los datos proporcionados por medio de **source**:<ul><li>**array** = array simple o multidimensional. En los casos de arrays simples, se dará una vuelta por cada índice. Valor por defecto.</li><li>**element** = objeto de datos.<br />contenido = será el resultado de ejecutar el comando **get** ó un array vacío cuando no exista<br />numrows = será el resultado de ejecutar el comando **rows** ó 0 si no existe<br />resetear el objeto = será el resultado de ejecutar el comando **reset**, cuando exista</li><li>**owl** = Objeto de Datos del tipo **nglOwl**.</li><li>**vector** = array simple, sólo se dará una vuelta.</li><li>**number** = genera un bucle de repetición basado en los parámetros **from** y **limit**.<br />El mismo  consistirá en un contador iniciado en **from** y dará **limit** vueltas. Si **from** y **limit** no son especificados, sus valores serán 0 y 1 respectivamente.<br />**limit** no podrá ser mayor al valor del argumento **loop_limit** del objeto **Rind**.</li><li>**object** = el contenido de **source** será tratado como un objeto, y se intentará convertirlo en un array asosiativo de manera recursiva.</li></ul>|
|**debug**|Imprime todos los contenidos disponibles en la primer vuelta del loop. Esto es útil para conocer el nombre de los índices disponibles|
|**aggregate**|Calcula valores de agregación sobre los campos solicitados. Los valores agregados son:<br /><ul><li>**sum** = suma</li><li>**avg** = promedio</li><li>**max** = valor máximo</li><li>**min** = valor mínimo</li></ul><br />Este parámetro espera el nombre de uno de los campos del **loop**, o una lista de ellos separados por coma (,)<br />Para llamar a los valores se deberá utilizar la sintáxis: **{#VALOR.CAMPO}**, por ejemplo **{#sum.amount}**|
|**index**|Reemplaza el índice natural del bucle y establece un nuevo orden de salida de datos. Este atributo espera como valor un **array** o una cadena separa por comas(,).|
|**tree**|Especifica que los datos proporcionados por **source** deberán ser tratados como un árbol.<br />El modo **tree** estará  disponible sólo si **source** es del tipo **array**.<br />**tree** es además el índice del sub-array de **source** que contiene los datos de los sub-nodos. Si **tree** = 1 se asumirá que el nombre del índice es **_children**<br /><br />Parámetros adicionales del modo **tree**:<ul><li>**branch_open** = cadena de texto que se imprimirá al comienzo de cada rama. Por defecto **&lt;ul&gt;**</li><li>**branch_close** = cadena de texto que se imprimirá al finalizar cada rama. Por defecto **&lt;/ul&gt;**</li><li>**node_open** = cadena de texto que se imprimirá al comienzo de cada nodo. Por defecto **&lt;li&gt;**</li><li>**node_close** = cadena de texto que se imprimirá al finalizar cada nodo. Por defecto **&lt;/li&gt;**</li></ul>|
|**from**|Indica el número de registro a partir del cual se mostrará el contenido de **content**.|
|**limit**|Indica la cantidad máxima de vueltas que dará el loop a partir de **from**.|
|**step**|Incremento del loop. Utilizar valores negativos para indicar decrementos. Por defecto **1**|
|**order**|Orden en el que se mostrarán los resultados del bucle. Este atributo trabaja sobre el índice de los datos, no sobre los datos. <br />Para conseguir mejores resultados utilizar este atributo en convinación con el atributo **index**.<br /><br />Los valores admitidos son:<ul><li>**asc** = orden ascendente.</li><li>**desc** =  orden descendente.</li><li>**reverse** = invierte el orden natural del índice. Sino se emplea **index** es valor es equivalente a utilizar **DESC**.</li><li>**random** = mostrará los datos en orden aleatorio.</li></ul>|
|**zerofill**|Cantidad de 0 con los que se completa por adelante **numrow** y **line**|
|**content**|Contenidos que se mostrarán como resultado de cada vuelta del bucle.|
|**empty**|Contenidos que se mostrarán como resultado en caso de que el bucle no tuviese datos que mostrar.|
  
### Ejemplos  
#### Array Simple  
```php
# Datos
$aAnimals = array(
    "bird",
    "cat",
    "dog",
    "duck",
    "zebra",
    "horse",
    "cow"
);

# Comando
<rind:loop>
    <@name>animals</@name>
    <@source>{$aAnimals}</@source>
    <@type>array</@type>
    <@content>
        - {animals.0} ó {#current}
    </@content>
</rind:loop>

# Resultado
- bird ó bird
- cat ó cat
- dog ó dog
- duck ó duck
- zebra ó zebra
- horse ó horse
- cow ó cow
```

#### Element  
```php
# Datos
$aGuy = array("id"=>1,"name"=>"John", "surname"=>"Smith", "age"=>"35")

# Comando
<rind:loop>
    <@name>info</@name>
    <@source>{$aGuy}</@source>
    <@type>element</@type>
    <@content>
        <form>
            <input type="hidden" name="id" value="{id}" />
            Nombre <input type="text" name="name" value="{name}" />
            Apellido <input type="text" name="surname" value="{surname}" />
            Edad <input type="text" name="age" value="{age}" />
        </form>
    </@content>
</rind:loop>
```

#### Array Multidimensional  
```php
# Datos
$aUsers = array(
    array("name"=>"John", "surname"=>"Smith", "age"=>"35", "gender"=>"M"),
    array("name"=>"Susan", "surname"=>"Johnson", "age"=>"29", "gender"=>"F"),
    array("name"=>"Carl", "surname"=>"Williams", "age"=>"43", "gender"=>"M")
);

# Comando
<rind:loop>
    <@name>counter</@name>
    <@source>{$aUsers}</@source>
    <@type>array</@type>
    <@aggregate>age</@aggregate>
    <@content>
        {name} de <i>{age}</i> se apellida <b>{surname}</b>
    </@content>
    <@empty>- no se hallaron resultados -</@empty>
</rind:loop>
<h2>Edad promedio: {$_SET["counter"]["avg"]["age"]}</h2>

# Resultado
John de <i>35</i> se apellida <b>Smith</b>
Susan de <i>29</i> se apellida <b>Johnson</b>
Carl de <i>43</i> se apellida <b>Williams</b>
```

#### Numéricos  
```php
# Comando
<select name="days">
    <rind:loop>
        <@name>days</@name>
        <@type>number</@type>
        <@from>1</@from>
        <@limit>31</@limit>
        <@zerofill>2</@zerofill>
        <@content>
            <option value="{#current}">{#current}</option>
        </@content>
    </rind:loop>
</select>

# Resultado
Lista desplegable con los días del mes

# Comando
<select name="month">
    <rind:loop>
        <@name>month</@name>
        <@type>number</@type>
        <@from>1</@from>
        <@limit>13</@limit>
        <@zerofill>2</@zerofill>
        <@content>
            <option value="{#numrow}">{#numrow}</option>
        </@content>
    </rind:loop>
</select>

# Resultado
Lista desplegable con los meses del año

# Comando
<select name="year">
    <rind:loop>
        <@name>years</@name>
        <@type>number</@type>
        <@from>2016</@from>
        <@limit>117</@limit>
        <@step>-1</@step>
        <@content>
            <option value="{$_SET.years.numrow}">{$_SET.years.numrow}</option>
        </@content>
    </rind:loop>
</select>

# Resultado
Lista desplegable con los años en modo descendente, desde el 2016 al 1900

# NOTA: en los ejemplos se puede ver como, en los loops numéricos, es posible imprimir el dato de diferentes maneras
```

#### Arbol
```php
# Datos
$aEmployees = array(array(
    "name" => "Employees",
    "_children" => array(
        array(
            "name"=>"John Smith",
            "_children" => array(
                array("name"=>"Sara"),
                array("name"=>"Max")
            )
        ),
        array(
            "name"=>"Susan Johnson",
            "_children" => array(
                array("name"=>"Ralph")
            )
        ),
        array(
            "name"=>"Carl Williams",
            "_children" => array(
                array("name"=>"Mary"),
                array("name"=>"Thomas"),
            )
        )
    )
));

# Comando
<rind:loop>
    <@name>employees</@name>
    <@source>{$aEmployees}</@source>
    <@type>array</@type>
    <@tree>_children</@tree>
    <@content>
        {name}
    </@content>
</rind:loop>

# Resultado
•Employees
    •John Smith
        ◦Sara
        ◦Max
    •Susan Johnson
        ◦Ralph
    •Carl Williams
        ◦Mary
        ◦Thomas
```

#### Archivo JSON  
```php
<rind:set>
    <@name>jsondata</@name>
    <@value>//cnd.abcontenidos.com/json/material-design-colors.json</@value>
    <@method>file,jsondec</@method>
</rind:set>

<rind:loop>
    <@name>pallette</@name>
    <@source>{$_SET.jsondata}</@source>
    <@content>
        <div>
            <rind:loop>
                <@name>colors</@name>
                <@source>{$_SET.pallette.data}</@source>
                <@content>
                    <div style="background-color:{$_SET.colors.data.0};padding: 5px;">
                        {$_SET.pallette.key}-{$_SET.colors.key} &nbsp; {$_SET.colors.data.0}
                    </div>
                </@content>
            </rind:loop>
        </div>
    </@content>
</rind:loop>
```

&nbsp;
___
&nbsp;

## mergefile
Incluye un archivo dentro de otro previo al proceso de datos. Esto significa que todos los comandos **rind** hallados dentro del archivo incluído, serán tratados como parte del archivo original.
**RindCommands::mergefile** puede ser invocado de dos maneras diferentes:<ul><li>**Modo Simple** = Sólo hay que especificar la ruta del archivo a incluir entre las etiquetas **&lt;rind:mergefile&gt;** y **&lt;/rind:mergefile&gt;**</li><li>**Modo Enriquecido** = Utilizado en los casos en los que se quieren transferir valores al archivo incluído sin tener validarlos por medio del sistema de variables o tener que setearlos previamente con **RindCommands::SET**
Para estos casos el parámetro **source** es utilizado para setear la ruta de inclución y el parámetro **data** para pasar los valores en formato JSON
En la plantilla a incluir, las claves de estos valores deberán estar expresadas en el formato {%NOMBRECLAVE%}. Las claves que no tengan un valor asignado en **data**, serán reemplazadas por cadenas vacias.</li><li>**Modo Multiple** = Este permite incluir varios archivos que esten en la misma carpeta, especificada en la etiqueta **source** 
Luego, en la etiqueta **multiple**, se especificarán las rutas y datos adicionales de cada uno de los archivos (ver ejemplo)</li><li>**Modo Submerge** = Es la combinación los modos **simples** y **multiple**. Permite reempalzar una **clave** del archivo especificado en **source** por el contenido de uno ó más archivos. Para esto nos valdremos de las etiquetas **submerge** y **multiple** (ver ejemplo)</li></ul>Mergefile trabaja en conjunto con el modo de CACHE del objeto **rind**:<ul><li>**none** = como la plantilla principal es leída en cada llamada, las sub-plantillas también lo son</li><li>**dev** = las sub-plantillas son leídas únicamente en la primer lectura de la plantilla principal, o cuando esta es modificada</li><li>**use** = nunca se leen las sub-plantillas, se utiliza el contenido cacheado durante la primer lectura de la plantilla principal</li></ul>  


|Parámetro|Descripción|
|---|---|
|**data**|Cadena JSON con las claves y valores que se desean pasar a la plantilla|
|**data&#8209;NAME**|Valor a pasar a la plantilla, donde NAME es la clave del mismo|
|**source**|Ruta del archivo a ser incluído o carpeta contenedora para el caso de una inclución múltiple.|
|**multiple**|Cadena ó archivo JSON con las rutas relativas a **source** de cada plantilla y los datos enriquecidos para cada una de ellas<br />Formato:<br />[<blockquote>["template1.html", {"title":"foo", "text":"bar"}],<br />["template2.html", {"value":"foo", "option":"bar"}],<br />["templateN.html", {"name":"foo", "lastname":"bar"}]</blockquote>]|
|**submerge**|Cadena mediante la que se especifican la clave donde se cargará el contenido de la/s subplantillas y la ruta principal las mismas, el equivalente a **source**. El formato es: **clave**:**ruta**|

Los parámetros **data-NAME**, **data** y las cadenas de datos de **multiple**, son complementarios. A mismas claves, prevalecerá la mas cercana a la plantilla. 
Tanto en **source** como en la ruta de los archivos JSON de **multiple**, si la misma comienza con /, la lectura se hará desde la carpeta NGL_SANDBOX. De lo contrario se tomará como ruta relativa a la del archivo principal. 

**!!!! IMPORTANTE !!!!**  
No es posible declarar **mergefiles multiples** dentro del contenido del argumento **content** de otro **mergefile**. Para ello se debera utilizar **data-content base64** ( [ver caso](#mergefile-fail) )

### Ejemplos  
#### Modo Simple  
```php
<rind:mergefile>header.html</rind:mergefile>
```
#### Modo Enriquecido  
```php
# Comando
<rind:mergefile>
    <@source>note.html</@source>
    <@data json>
        {"title":"Nota de Prueba", "bgcolor":"#DEDEDE"}
    </@data>
</rind:mergefile>

# En el archivo note.html
<body bgcolor="{%bgcolor%}">
    {%title%}
    ...
</body>
```

#### Modo Multiple  
```php
# Comando
<rind:mergefile>
    <@source>snippets/forms</@source>
    <@multiple json>
        [
            ["input.html", {"name":"surname", "label":"Apellido"}],
            ["input.html", {"name":"firstname", "label":"Nombre"}],
            ["date.html", {"name":"birthday", "label":"Fecha Nacimiento", "type":"date"}],
            ["input.html", {"name":"cuil", "label":"CUIL"}],
            ["countries.html", {"name":"nationality", "label":"Nacionalidad"}]
        ]
    </@multiple>
</rind:mergefile>
```

#### Embed + Modo Enriquecido  
```php
# Para el caso de un LOOP variable, como podria ser el del snippet de un SELECT
<rind:mergefile>
    <@source>loop.html</@source>
    <@data json>
        {"value":"id", "option":"country"}
    </@data>
</rind:mergefile>

# contenido de loop.html
<rind:loop>
    <@name>{%loopname%}</@name>
    <@source>{$aCountries}</@source>
    <@content>
        <option value="{loopname.{%value%}}">{loopname.{%option%}}</option>
    </@content>
</rind:loop>
```

#### Multiples parámetros **data**
En este caso, todos los templates asumirán el valor **form-control** para la clave **cssclass**.
Los dos primeros **switchs** utilizarán como **source** los valores **SI** y **NO**, mientras que el último **switch** utilizará su propio origen de datos.
```php
# Comando
<rind:mergefile>
    <@source>snippets/forms</@source>
    <@data-cssclass base64>form-control</@data-cssclass>
    <@data json>
        {"source": [{"label":"SI","value":"1"},{"label":"NO","value":"0"}]}
    </@data>
    <@multiple json>
        [
            ["input.html", {"name":"fullname", "label":"Nombre Completo"}],
            ["switch.html", {"name":"married", "label":"Casado"}],
            ["switch.html", {"name":"children", "label":"Hijos"}],
            ["switch.html", {
                "name":"newsletter",
                "label":"Suscribirse",
                "source": [{"label":"SI","value":"1"}]
            }],
        ]
    </@multiple>
</rind:mergefile>
```

#### Submerge
En este caso se utiliza **mergefile** para realizar la inclusión de una plantilla principal (modo inverso) en la que se cargará un formulario en la clave **form**
```php
# Comando
<rind:mergefile>
    <@source>/templates/index.html</@source>
    <@data-title base64>Formulario de Login</@data-title>
    <@submerge base64>form:/templates/forms-parts/</@submerge>
    <@multiple json>
        [
            ["input.html", {"name":"user", "label":"Nombre de Usuario"}],
            ["password.html", {"name":"pass", "label":"Contraseña"}]
        ]
    </@multiple>
</rind:mergefile>

# contenido de index.html
<body>
    Bienvenido!<br />
    <br />
    <form action="login">
        {%form%}
        <div><input type="submit" /></div>
    </form>
</body>
```

#### JSON externo
Este caso es igual al anterior, solo que los datos del argumento **multiple** provienen de un archivo JSON.  
```php
# Comando
<rind:mergefile>
    <@source>/templates/index.html</@source>
    <@data-title base64>Formulario de Login</@data-title>
    <@submerge base64>form:/templates/forms-parts/</@submerge>
    <@multiple json>form.json</@multiple>
</rind:mergefile>

# contenido de index.html
<body>
    Bienvenido!<br />
    <br />
    <form action="login">
        {%form%}
        <div><input type="submit" /></div>
    </form>
</body>

# contenido de form.json
[
    ["input.html", {"name":"user", "label":"Nombre de Usuario"}],
    ["password.html", {"name":"pass", "label":"Contraseña"}]
]
```

#### Multiple en argumento content <a name="mergefile-fail"></a>
No es posible declarar un **mergefile** multiple dentro del argumento **content** de otro **mergefile**.
```php
# Comando
<rind:mergefile>
    <@source>/templates/index.html</@source>
    <@data-title base64>Formulario de Login</@data-title>
    <@content>
        <h2>Login</h2>
        <rind:mergefile>
            <@source>/snippets/forms/</@source>
            <@multiple json>
                [
                    ["input.html", {"name":"user", "label":"Nombre de Usuario"}],
                    ["password.html", {"name":"pass", "label":"Contraseña"}]
                ]
            </@multiple>
        </rind:mergefile>
    </@content>
</rind:mergefile>
```

**SOLUCION** 
```php
# Comando
<rind:mergefile>
    <@source>/templates/index.html</@source>
    <@data-title base64>Formulario de Login</@data-title>
    <@data-content base64>
        <h2>Login</h2>
        <rind:mergefile>
            <@source>/snippets/forms/</@source>
            <@multiple json>
                [
                    ["input.html", {"name":"user", "label":"Nombre de Usuario"}],
                    ["password.html", {"name":"pass", "label":"Contraseña"}]
                ]
            </@multiple>
        </rind:mergefile>
    </@data-content>
</rind:mergefile>
```

&nbsp;
___
&nbsp;

## rtn
Por razones de seguridad todas las expresiones utilizadas en los comandos son entrecomilladas por el sistema.
Este comando prepara una expresión para ser utilizada dentro de otro, eliminando las comillas de apertura y cierre.  


|Parámetro|Descripción|
|---|---|
|**content**|Expresión u otro comando.|
|**print**|Determina si el resultado se imprime o retorna. Default TRUE|

### Ejemplos  
#### expresión matemática  
```php
<rind:set>
    <@name>total</@name>
    <@value>
        <rind:ceil>
            <rind:rtn>{$nPrice}/{$nProducts}</rind:rtn>
        </rind:ceil>
    </@value>
</rind:set>
```

#### dump a variable  
```php
<rind:set>
    <@name>environment</@name>
    <@value>
        <rind:rtn>
            <rind:dump>
                <@content>{$ENV}</@content>
                <@print>0</@print>
            </rind:dump>
        </rind:rtn>
    </@value>
</rind:set>
{$_SET.environment}
```

&nbsp;
___
&nbsp;

## set
Este comando permite setear variables en el ambito de las plantillas.
Todos los valores seteados por medio de **RindCommands::set** serán alamcenados como indices de la variable **$_SET** y podrán ser accedidos utilizando las sintaxis:
**{\$_SET.name}** o **{\$_SET["name"]}** para variables simples
**{\$_SET.name.foo.bar}** o **{\$_SET["name"]["foo"]["bar"]}** para arrays

Para el caso en que se necesite setear una variable en modo condicional, es decir, que el valor a asignar dependa de tal o cual factor/es, se podrá establecer como **value** el resultado del comando **RindCommands::ifcase** con la opción **setmode** activa. Para mas detalles ver **RindCommands::ifcase**
Los nombres de variables deberán matchear con el patrón **[a-z0-9_{}%]**, cualquier caracter fuera del patrón será eliminado.

Nota: Como **RindCommands::set** almacena en la variable todo lo que se encuentre entre &lt;@value&gt; y &lt;/@value&gt;, se debe tener cuidado con las tabulaciones y saltos de linea.  

|Parámetro|Descripción|
|---|---|
|**index**|Valor númerico que indica la posición del array que se desea obtener cuando se utiliza el **method** es **element** ó nombre del indice del sub-array que hará de clave en el método **vector**|
|**filter**|Condición evaluada en el método **filter**, donde **$v** será el valor actual de item y **$k** el nombre de su clave. Por ejemplo: ```{$v.age}>17 && {$v.gender}=="M"``` |
|**keys**|Lista de valores separados por coma (,) utilizados cuando se emplea el método **chkeys**|
|**linebreak**|Caracter utilizado como salto de línea para cuando **method** es **explode**|
|**method**|Establece el/los método/s con el/los que será tratado el valor de **value** antes de almacenarlo. <br />Se pueden especificar varios métodos separandos por coma (,) en este caso los métodos se aplicarán en secuencia.<br />Métodos disponibles:<ul><li>**<i>N</i>** = el contenido alamcenado contendrá al elemento **<i>N</i>** del conjunto **value**, ya sea el valor de un índice cuando sea un array o un caracter cuando sea una cadena.<br />**<i>N</i>** comienza a contar desde 1, cuando el valor sea negativo se contará desde el final del conjunto.<br />Cuando el resultado sea un array bidimensional de un único subindice, este será retornado como vector. Para conseguir un array de vectores se deberá anteponer el método **each**</li><li>**<i>N:L</i>** = el contenido alamcenado será la secuencia de elementos del conjunto **value** tal y como se especifiquen los valores **<i>N</i>** y **<i>L</i>**. Donde el primero es la posición del primer elemento y el segundo la cantidad de elementos a capturar.<br />**<i>N</i>** comienza a contar desde 1, cuando el valor sea negativo se contará desde el final del conjunto.</li><li>**<i>N\|N1\|N2\|Nn</i>** = el contenido alamcenado será la secuencia de elementos del conjunto **value** tal y como se especifiquen los valores **<i>N</i>**. Donde cada **N** es la posición del elemento a capturar. **<i>N</i>** puede ser un valor entero o una cadena de texto para el caso de arrays asociativos.<br />Los números enteros comienza a contar desde 1 y deben ser siempre mayores a 0.</li><li>**\[ _keyword_ \]** = obtiene el índice **keyword** del conjunto **value**</li><li>**base64dec** = antes de ser almacenado el contenido será tratado con **base64_decode**</li><li>**base64enc** = antes de ser almacenado el contenido será tratado con **base64_encode**</li><li>**chkeys** = cuando el contenido sea un array, sus claves serán reemplazadas por los valores de la cadena <@keys></li><li>**each** = cuando el contenido sea un array bidimensional, indica que el siguiente método deberá aplicarse de manera recursiva sobre todos sus elementos</li><li>**element** = en caso de existir, el contenido alamcenado será el resultado de ejecutar el método **get** del objeto **value**</li><li>**elements** = en caso de existir, el contenido alamcenado será el resultado de ejecutar el método **getall** del objeto **value**</li><li>**explode** = antes de ser almacenado el contenido será explotado utilizando **splitter** como separador<br />Opcionalmente se podrá especificar el parámetro **linebreak** como salto de línea, consiguiendo asi un array multidimensional</li><li>**file** = el valor será tratado como un path de archivo y se intentará obtener su contenido. Cuando se especifiquen mas de un método, sólo el primero de ellos podrá ser **file**.</li><li>**filter** = trabaja sobre un array de datos el cual es tratado con **array_filter**, la condición evaluada será la de argumento <@filter> y debe repetar la sintáxsis del comando [ifcase](#ifcase) en su modalidad **inline** sin los paréntesis. En cada iteración se pasarán al método 2 valores: <ul><li>**$k** clave de la vuelta actual</li><li>**$v** datos de la vuelta</li></ul></li><li>**group** = agrupa los valores según la estructura **structure** Cuando no se proporcione una estructura, los valores únicos de cada columna formarán subgrupos</li><li>**implode** = antes de ser almacenado el contenido será tratado con **implode** utilizando **splitter** como caracter de unión</li><li>**jsondec** = antes de ser almacenado el contenido será tratado con **json_decode** y convertido en un array asociativo</li><li>**jsonenc** = antes de ser almacenado el contenido será tratado con **json_encode** con los flags:JSON_HEX_TAG \| JSON_NUMERIC_CHECK \| JSON_HEX_APOS \| JSON_HEX_QUOT \| JSON_HEX_AMP \| JSON_UNESCAPED_UNICODE</li><li>**keys** = trabaja sobre un array de datos el cual es tratado con **array_keys**, es decir, que retorna un vector con las claves del valor de origen</li><li>**length**/**len** = calcula el largo del valor, utilizando **count** y **strlen**</li><li>**nest** = trabaja sobre un array, anidando sus elementos convirtiendose asi en un árbol. Para ello se deberá definir una relación de dos indices (padre,hijo) en el argumento <@relation></li><li>**number** = se eliminarán del contenido todos los caracteres NO numéricos antes almacenarlo. Si el resultado de la limpieza fuese un valor NO numérico, el valor será 0</li><li>**object** = antes de ser almacenado el contenido será convertido en un array de manera recursiva</li><li>**querydec** = antes de ser almacenado el contenido será tratado con **parse_str** y convertido en un array</li><li>**queryenc** = antes de ser almacenado el contenido será tratado con **http_build_query**</li><li>**rawurldec** = antes de ser almacenado el contenido será tratado con **rawurldecode**</li><li>**rawurlenc** = antes de ser almacenado el contenido será tratado con **rawurlencode**</li><li>**serialenc** = antes de ser almacenado el contenido será tratado con **serialize**</li><li>**serialdec** = antes de ser almacenado el contenido será tratado con **unserialize** y convertido en un array</li><li>**urldec** = antes de ser almacenado el contenido será tratado con **urldecode**</li><li>**urlenc** = antes de ser almacenado el contenido será tratado con **urlencode**</li><li>**vector** = genera un vector clave/valor a partir de un array bidimensional, con **index** como clave y **column** como valor. Si **column** no es especificado, tomará el primer índice del sub-array</li><li>**xml** = antes de ser almacenado el contenido será convertido en un array</li></ul>|
|**name**|Nombre de la variable a setear|
|**operator**|Tipo de operador. Por defecto se utilizará el operador **=**, para dar inicio a la variable. Si el operador es diferente de **=**, el valor de la variable será alterado segun el caso.<br />Se espera: ( = \| . \| + \| - \| * \| / \| % ).</i><ul><li>**.** = concatena el nuevo valor al valor actual. Unicamente válido para cadenas.</li><li>**+** = suma el nuevo valor al valor actual.</li><li>**-** = resta el nuevo valor al valor actual.</li><li>***** = multiplica el valor actual por el nuevo.</li><li>**/** = divide el valor actual por el nuevo.</li><li>**%** = modulo el valor actual y el nuevo.</li></ul>|
|**relation**|Par valores separados por coma (,) utilizados cuando se emplea el método **nest**. El orden es, **id,id_parent**|
|**splitter**|Caracter utilizado como separador en **explode**, y caracter de concatenación en **implode**. Por defecto , (coma)|
|**structure**|Cadena JSON que detemina la estructura de agrupamiento del método **group**<br />{<blockquote> "MAIN": [ <i>nodo principal</i> <blockquote> "id", [ <i>id del nodo</i> <blockquote> "campo 1", <i>campos del nodo</i><br /> "campo N"<br /> </blockquote> ] </blockquote> ],<br /> "id_secundario": [ <i>nodo de agrupamiento</i> <blockquote> "campo 5", [ <i>id del nodo</i><br /> <blockquote> "campo 1", <i>campos del nodo</i><br /> "campo N"<br /> </blockquote> ] </blockquote> ] </blockquote>}<br />|
|**userpwd**|Utilizado cuando **file** requiera autenticación HTTP ó en WS del tipo REST<br />En ocaciones el valor debe ser pasado con el attributo **quotes**<ul><li>**HTTP** = username:password</li><li>**basic** = basic username:password</li><li>**bearer** = bearer token</li><li>**alvin** = alvin alvintoken</li></ul><br />|
|**body**|Cadena de datos que se enviará en el BODY en caso de solicitudes REST **file**. En ocaciones el valor debe ser pasado con el attributo **json**|
|**columns**|Nombre del índice el sub-array en el método **vector**|
|**value**|Nombre del índice el sub-array|

### Ejemplos  
#### Ejemplo Simple  
```php
# Inicio de variable
<rind:set>
    <@name>counter</@name>
    <@value>1</@value>
    <@operator>=</@operator>
</rind:set>

# Impresión
{$_SET.counter} => 1


# Incremento
<rind:set>
    <@name>counter</@name>
    <@value>1</@value>
    <@operator>+</@operator>
</rind:set>
    
# Impresión
{$_SET.counter} => 2
```

#### Set Mode  
```php
<rind:set>
    <@name>puesto</@name>
    <@value>
        <rind:ifcase>
            <@setmode>1</@setmode>

            <@iff:a>{$nPosition}==1</@iff:a>
            <@then:a>Primero</@then:a>

            <@iff:b>{$nPosition}==2</@iff:b>
            <@then:b>Segundo</@then:b>

            <@iff:c>{$nPosition}==3</@iff:c>
            <@then:c>Tercero</@then:c>

            <@else>Otro</@else>
        </rind:ifcase>
    </@value>
</rind:set>
```

#### Comandos  
```php
#explode
<rind:set>
    <@name>person</@name>
    <@value>John;Smith;35;Male</@value>
    <@method>explode</@method>
    <@splitter>;</@splitter>
    <@keys>name;surname;age;gender</@keys>
</rind:set>

{$_SET.person.name} => John
{$_SET.person.age} => 35


#queryenc
<rind:set>
    <@name>query</@name>
    <@value>{$_SET.person}</@value>
    <@method>queryenc</@method>
</rind:set>
<rind:dump>{$_SET.query}</rind:dump>
```

#### Aplicando varios comandos  
```php
<rind:set>
    <@name>owl2json</@name>
    <@value>
        <rind:nut.owl.getall>
            <@object>SOMEOBJECT</@object>
            <@filter json>
                {
                    "where":[
                        ["nombre_comun","like","(%{$_SET.q}%)"],
                        "OR",
                        ["espacio_verde","like","(%{$_SET.q}%)"],
                        "OR",
                        ["ubicacion","like","(%{$_SET.q}%)"]
                    ]
                }
            </@filter>
            <@offset>{$_REQUEST.values.pager}</@offset>
            <@state>1</@state>
            <@limit>{$ENV.pager_limit}</@limit>
        </rind:nut.owl.getall>
    </@value>
    <@method>object,jsonenc</@method>
</rind:set>
<rind:dump>{$_SET.owl2json}</rind:dump>
```

#### Metodo Group SIN estructura  
```php
# array PHP
$aSource = array(
    array("sale"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>1,"quantity"=>15,"price"=>20),
    array("sale"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>2,"quantity"=>10,"price"=>16),
    array("sale"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>3,"quantity"=>20,"price"=>20),
    array("sale"=>2,"date"=>"2015-11-24","name"=>"Ravelli SA","cuit"=>"33-58796321-8","product"=>2,"quantity"=>13,"price"=>16),
    array("sale"=>2,"date"=>"2015-11-24","name"=>"Ravelli SA","cuit"=>"33-58796321-8","product"=>3,"quantity"=>8,"price"=>20)
);

# comando
<rind:set>
    <@name>test</@name>
    <@value>{$data}</@value>
    <@method>group</@method>
</rind:set>

# salida
array(
    [1] => array(
            [sale] => 1
            [date] => 2015-11-23
            [name] => Castro Hnos SRL
            [cuit] => 30-36251478-9
            [product] => array(
                [0] => 1
                [1] => 2
                [2] => 3
            )
            [quantity] => array(
                [0] => 15
                [1] => 10
                [2] => 20
            )
            [price] => array(
                [0] => 20
                [1] => 16
            )
        )
    [2] => array(
            [sale] => 2
            [date] => 2015-11-24
            [name] => Ravelli SA
            [cuit] => 33-58796321-8
            [product] => array(
                [0] => 2
                [1] => 3
            )
            [quantity] => array(
                [0] => 13
                [1] => 8
            )
            [price] => array(
                [0] => 16
                [1] => 20
            )
        )
    )
)
```

#### Metodo Group CON estructura (1)  
```php
# array PHP
$data = array(
    array("sale"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>1,"quantity"=>15,"price"=>20),
    array("sale"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>2,"quantity"=>10,"price"=>16),
    array("sale"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>3,"quantity"=>20,"price"=>20),
    array("sale"=>2,"date"=>"2015-11-24","name"=>"Ravelli SA","cuit"=>"33-58796321-8","product"=>2,"quantity"=>13,"price"=>16),
    array("sale"=>2,"date"=>"2015-11-24","name"=>"Ravelli SA","cuit"=>"33-58796321-8","product"=>3,"quantity"=>8,"price"=>20)
);

# comando
<rind:set>
    <@name>test</@name>
    <@value>{$data}</@value>
    <@method>group</@method>
    <@structure json>
        {
            "MAIN":["sale", [
                    "sale",
                    "name",
                    "cuit",
                    "date"
                ]
            ],
            "details": [
                "product", [
                    "quantity",
                    "price"
                ]
            ]
        }
    </@structure>
</rind:set>

# salida
array(
    [1] => array(
        [sale] => 1
        [name] => Castro Hnos SRL
        [cuit] => 30-36251478-9
        [date] => 2015-11-23
        [details] => array(
            [1] => array(
                [quantity] => 15
                [price] => 20
            )
            [2] => array(
                [quantity] => 10
                [price] => 16
            )
            [3] => array(
                [quantity] => 20
                [price] => 20
            )
        )
    )
    [2] => array(
        [sale] => 2
        [name] => Ravelli SA
        [cuit] => 33-58796321-8
        [date] => 2015-11-24
        [details] => array(
            [2] => array(
                [quantity] => 13
                [price] => 16
            )
            [3] => array(
                [quantity] => 8
                [price] => 20
            )
        )
    )
)
```

#### Metodo Group CON estructura (2)  
```php
# array PHP
$data = array(
    array("post"=>1,"title"=>"Que es Nogal","content"=>"Lorem ipsum dolor sit amet consectetur adipiscing elit.","image"=>1,"filename"=>"nogal_1.jpg","caption"=>"auctor risus sodales"),
    array("post"=>1,"title"=>"Que es Nogal","content"=>"Lorem ipsum dolor sit amet consectetur adipiscing elit.","image"=>2,"filename"=>"nogal_2.jpg","caption"=>"lorem dignissim metus"),
    array("post"=>2,"title"=>"Instalación","content"=>"Aliquam erat volutpat. Aliquam facilisis ante in eros ullamcorper","image"=>3,"filename"=>"step_1.jpg","caption"=>"risus sodales"),
    array("post"=>2,"title"=>"Instalación","content"=>"Aliquam erat volutpat. Aliquam facilisis ante in eros ullamcorper","image"=>4,"filename"=>"step_2.jpg","caption"=>"tincidunt aliquam"),
    array("post"=>2,"title"=>"Instalación","content"=>"Aliquam erat volutpat. Aliquam facilisis ante in eros ullamcorper","image"=>5,"filename"=>"step_3.jpg","caption"=>"imperdiet ante")
);

# comando
<rind:set>
    <@name>test</@name>
    <@value>{$data}</@value>
    <@method>group,element,2</@method>
    <@structure json>
        {
            "MAIN":["post", [
                    "post",
                    "title",
                    "content"
                ]
            ],
            "images": [
                "image", [
                    "filename",
                    "caption"
                ]
            ]
        }
    </@structure>
</rind:set>

# salida
array(
    [post] => 2
    [title] => Instalación
    [content] => Aliquam erat volutpat. Aliquam facilisis ante in eros ullamcorper
    [details] => array(
        [3] => array(
            [filename] => step_1.jpg
            [caption] => risus sodales
        )
        [4] => array(
            [filename] => step_2.jpg
            [caption] => tincidunt aliquam
        )
        [5] => array(
            [filename] => step_3.jpg
            [caption] => imperdiet ante
        )
    )
)
```

#### Metodo Filter 
```php
# array PHP
$data = array(
    array("sale"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>1,"quantity"=>15,"price"=>20),
    array("sale"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>2,"quantity"=>10,"price"=>16),
    array("sale"=>1,"date"=>"2015-11-23","name"=>"Castro Hnos SRL","cuit"=>"30-36251478-9","product"=>3,"quantity"=>20,"price"=>20),
    array("sale"=>2,"date"=>"2015-11-24","name"=>"Ravelli SA","cuit"=>"33-58796321-8","product"=>2,"quantity"=>13,"price"=>16),
    array("sale"=>2,"date"=>"2015-11-24","name"=>"Ravelli SA","cuit"=>"33-58796321-8","product"=>3,"quantity"=>8,"price"=>20)
);

# comando
<rind:set>
    <@name>test</@name>
    <@value>{$data}</@value>
    <@method>filter</@method>
    <@filter>{$v.price}<20</@filter>
</rind:set>
```

#### Convertir un array a Base64  
```php
# array PHP
$data = array("name"=>"John", "lastname"=>"Smith", "age"=>35);


# array a json
<rind:set>
    <@name>json2b64</@name>
    <@value>{$data}</@value>
    <@method>jsonenc,base64enc</@method>
</rind:set>
<rind:dump>{$_SET.json2b64}</rind:dump>
```

#### Crear un array a partir de una cadena  
```php
<rind:set>
	<@name>tests</@name>
	<@value json>[
        {"name":"Castro Hnos SRL", "cuit":"30-36251478-9"},
        {"name":"Ravelli SA", "cuit":"33-58796321-8"}
    ]</@value>
</rind:set>
```

#### Convertir un array de PHP a JS  
```php
<rind:set>
    <@name>envjson</@name>
    <@value>{$ENV}</@value>
    <@method>jsonenc</@method>
</rind:set>
var ENV = eval({$_SET.envjson});
```

#### Lista de valores  
```php
# Comando
<rind:set>
    <@name>days</@name>
    <@value>lunes;martes;miércoles;jueves;viernes;sábado;domingo</@value>
    <@method>explode</@method>
    <@splitter>;</@splitter>
</rind:set>

# Resultado
Array (
    [0] => lunes
    [1] => martes
    [2] => miercoles
    [3] => jueves
    [4] => viernes
    [5] => sabado
    [6] => domingo
)
```

#### sub-arrays / each  
```php
# array PHP
$data = array(
    array("name"=>"John", "surname"=>"Smith", "age"=>"35", "gender"=>"M"),
    array("name"=>"Susan", "surname"=>"Johnson", "age"=>"29", "gender"=>"F"),
    array("name"=>"Carl", "surname"=>"Williams", "age"=>"43", "gender"=>"M")
);

# Comando
<rind:set>
  <@name>foo</@name>
  <@value>{$data}</@value>
  <@method>1</@method>
</rind:set>

# Resultado
Array (
    [name] => John
    [surname] => Smith
    [age] => 35
    [gender] => M
)

# Comando
<rind:set>
  <@name>foo</@name>
  <@value>{$data}</@value>
  <@method>each,1</@method>
</rind:set>

# Resultado
Array (
    [0] => Array ( [name] => John )
    [1] => Array ( [name] => Susan )
    [2] => Array ( [name] =>  Carl )
)
```

#### Ejemplo de subcadena  
```php
# Comando
<rind:set>
  <@name>foo</@name>
  <@value>ABCDEFGHI</@value>
  <@method>3:4</@method>
</rind:set>

# Resultado
CDEF
```

#### Indice alfanumérico  
```php
# array PHP
$data = array("name"=>"John", "lastname"=>"Smith", "age"=>35);


# indice
<rind:set>
    <@name>bykey</@name>
    <@value>{$data}</@value>
    <@method>jsonenc,[lastname]</@method>
</rind:set>
<rind:dump>{$_SET.bykey}</rind:dump>

# Resultado
Smith
```

#### Vector de valores  
```php
# array PHP
$data = array(
    array("name"=>"John", "surname"=>"Smith", "age"=>"35", "gender"=>"M"),
    array("name"=>"Susan", "surname"=>"Johnson", "age"=>"29", "gender"=>"F"),
    array("name"=>"Carl", "surname"=>"Williams", "age"=>"43", "gender"=>"M")
);

# Comando
<rind:set>
  <@name>foo</@name>
  <@value>{$data}</@value>
  <@method>vector</@method>
  <@column>surname</@column>
  <@index>name</@index>
</rind:set>

# Resultado
Array (
    [John] => Smith
    [Susan] => Johnson
    [Carl] => Williams
)
```

#### Set Mode (ver RindCommands::ifcase)  
```php
# Establece el valor de la variable en función del resultado del ifcase
<rind:set>
    <@name>puesto</@name>
    <@value>
        <rind:ifcase>
            <@setmode>1</@setmode>

            <@iff:a>{$nPosition}==1</@iff:a>
            <@then:a>Primero</@then:a>

            <@iff:b>{$nPosition}==2</@iff:b>
            <@then:b>Segundo</@then:b>

            <@iff:c>{$nPosition}==3</@iff:c>
            <@then:c>Tercero</@then:c>

            <@else>Otro</@else>
        </rind:ifcase>
    </@value>
</rind:set>
```

#### Web Service REST
con autenticación basic y body en formato JSON
```php
# Comando
<rind:set>
	<@name>test</@name>
	<@value>https://dominio.io/ws/run</@value>
	<@userpwd>basic foo:barpass</@userpwd>
	<@body json>{"method":"list", "limit":"{$_GET.limit}"}</@body>
	<@method>file,jsondec</@method>
</rind:set>

# Resultado
(JSON de resultados)
```

&nbsp;
___
&nbsp;

## unique
Genera un código único.  


|Parámetro|Descripción|
|---|---|
|**content**|Longuitud del código|
### Ejemplos  
#### example  
```php
#Comando
<rind:unique>8</rind:unique>

#Salida
kE7XDb1k
```

&nbsp;
___
&nbsp;

## skip
Aborta la ejecución de la actual vuelta y continúa con la siguiente. Disponible sólo dentro de un **RindCommands::loop**.
Es un comando simple (inline), que no posee parámetros.  

 *inline* function ( );
  
### Ejemplos  
#### example  
```php
# Datos
Para mostrar únicamente los usuarios de USA del siguiente Array: 

$aUsers = array(
    array("name"=>"John", "surname"=>"Smith", "age"=>"35", "country"=>"USA"),
    array("name"=>"Susan", "surname"=>"Johnson", "age"=>"29", "country"=>"Canadá"),
    array("name"=>"Carl", "surname"=>"Williams", "age"=>"43", "country"=>"USA")
);

# Comando
<rind:loop>
    <@name>users</@name>
    <@source>{$aUsers}</@source>
    <@type>array</@type>
    <@content>
        <rind:ifcase>({country}!="USA")<rind:skip /></rind:ifcase>
        {name} {surname} vive en {country}
    </@content>
</rind:loop>
```

&nbsp;
___
&nbsp;

## unset
Permite desetear variables seteadas por medio del comando **RindCommands::set**. Los nombres de variables deberán matchear con el patrón **[a-z0-9_]**, cualquier caracter fuera del patrón será eliminado.  


|Parámetro|Descripción|
|---|---|
|**content**|Nombre de variable a desetear.|
### Ejemplos  
#### Deseteo  
```php
# Seteo
<rind:set>
    <@name>foobar</@name>
    <@value>hola</@value>
</rind:set>

# Impresión
{$_SET.foobar}

# Deseteo
<rind:unset>foobar</rind:unset>

# Impresión fallida
{$_SET.foobar}
```

&nbsp;
___
&nbsp;

## /*...*
**Rind** cuenta con una manera de añadir comentarios al código fuente.
Los comentarios son del tipo bloque, y su sintáxis es:

**&lt;rind:/&#42;**...**&#42;/&gt;**

donde **...** ejemplifica el texto que desea ser comentado.  

 *inline* function ( );
  
### Ejemplos  
#### Linea  
```php
<rind:/* Inicio del chequeo */>
```
#### Multilinea  
```php
<rind:/*;
    <rind:ifcase>
        <@iff>{gender}=="M"</@iff>
        <@then>Sr.</@then>
        <@else>Sra.</@else>
    </rind:ifcase>
*/>;
```

&nbsp;
___
&nbsp;

# Normalización de Datos
A todos los parámetros se les puede establecer por medio de un atributo si deben o no ser tratados por un normalizador de datos.
Supongamos que tenemos el parámetro <@value>, entonces:

**&lt;@value base64&gt;...&lt;/@value&gt;**  

el valor RAW del argumento es tratado con base64_encode antes de ser utilizado
con valor RAW nos referimos al código crudo (exacto) hallado en la plantilla, antes de ser procesado por el sistema, entonces:

```php
<@value>foobar is a test string</@value>
retornará: foobar is a test string

<@value base64>foobar is a test string</@value>
retornará: Zm9vYmFyIGlzIGEgdGVzdCBzdHJpbmc=
```

## Métodos de Normalización
|Método|Descripción|
|---|---|
|quotes|Englobla el contenido de una variable en el formato HEREDOC|
|base64|trata al valor como un cadena y la codifica en base64|
|join|Aplica **implode** sobre el valor, usando **NGL_STRING_SPLITTER** como pegamento, antes de ser utilizado|
|json|Trata al valor como un cadena json y la convierte en un array|
|split|Aplica **explode** sobre el valor, usando **NGL_STRING_SPLITTER** como separador, antes de ser utilizado|

```html
<rind:loop>
    <@name>data</@name>
    <@source json>
		[
			{
				"id": 1,
				"name": "Coca-Cola 3lts",
				"price": 22
			},
			{
				"id": 1,
				"name": "Coca-Cola 2lts",
				"price": 18
			}
		]
    </@source>
    <@content>
        {name} ${price}<br />
    </@content>
</rind:loop>
```

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2021 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br /> 
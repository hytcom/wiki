# nogal::rind - Clase 1 - Conceptos Básicos
Plantillas HTML
___

# Contenidos
- [Estructura de Archivos](#estructura-de-archivos)
- [Variables](#variables)
- [Comandos](#comandos)
- [Variables Anidadas](#variables-anidadas)
- [Comandos Anidados](#comandos-anidados)

&nbsp;

## Estructura de Archivos
### Concepto de plantilla
En este momento no vamos a adentrarnos mucho en este tema, sólo debemos saber que **rind** trabaja con 3 archivos:

- **HTML** es donde vamos a trabajar con los comandos
- **PHP** es donde el programador PHP prepara los datos e invoca al archivo HTML
- **RIND** es el resultado que arroja el sistema luego de procesar los archivos HTML y PHP
<br />

Rind combina los datos del archivo PHP con las declaraciones e interfaz gráfica del archivo HTML. El resultado de este proceso es un archivo RIND, que no es mas que un archivo PHP avanzado.
<br />

### El clásico "Hola Mundo!"
**index.php**
``` php
<?php
require_once("config.php");

$myvar = "Mundo";
echo $ngl("rind")->stamp();
?>
```
<br />

**index.html**
``` html
<html>
    <head>
        <title>demo</title>
    </head>
    <body>
        Hola {$myvar}!
    </body>
</html>
```
<br />

**index.html.rind**
``` php
<?php /*rind-ac2c9f4756518fe726709b2758e58e52-N9uaFH-20170115223049*/ ?>
<html>
    <head>
    	<title>demo</title>
    </head>
    <body>
	    Hola <?php echo $GLOBALS["myvar"];?>!
    </body>
</html>
```
<br />


**Navegador**

Hola Mundo!

<br />
<br />

## Variables
### Definición de variable
En programación, una variable está formada por un espacio en memoria y un nombre simbólico asociado a dicho espacio. Ese espacio contiene una cantidad o información conocida o desconocida, es decir un valor.
Imaginemos una caja, ahora supongamos que en esa caja vamos guardando diferentes objetos, uno a la vez. Siempre que hagamos referencia a la "caja" estaremos hablando de un objeto específico, sin importar lo que sea. Este es el concepto de variable.
En programación se utilizan variables cuando se desconoce un valor, pero se conoce que se debe hacer con él. Por ejemplo, si un usuario inicia sesión en nuestro sitio y queremos saludarlo, sabemos que inició sesión y sabemos que tiene un nombre de usuario, pero no sabemos cual es ese nombre. Para saludarlo usamos una variable:

```
<h1>Bienvenido {$username}</h1>
```

En **PHP** un nombre de variable tiene que empezar con una letra o un carácter de subrayado (underscore), seguido de cualquier número de letras, números y caracteres de subrayado; todo esto precedido por un signo **$**

Algunos ejemplos:
- $my_variable
- $part06
- $sUserName
- $4day (no es un nombre válido, pues comienza con un número)

Para aprender más sobre las variables en PHP hacé [click](http://php.net/language.variables.basics)
<br />

### Imprimir variables
Para imprimir una variable proveniente del entorno PHP en la plantilla, se deberá encerrar a la misma entre llaves. A su vez existen dos maneras de escribirlas, utilizando la sintáxis de **PHP** o la sintáxis abreviada **Rind**
La sintáxis **Rind** es como la de javascript, se utilizan puntos para separar en índices y subíndices

**index.php**
``` php
<?php
require_once("config.php");

$foo = "variable simple"; 
$bar = array(); 
$bar["foo"] = "índice de un array"; 
$bar["bar"]["foo"] = "índice de un array bidimensional"; 
$foobar = new stdClass();
$foobar->bar = "variable del objeto"; 
$foobar->foo["bar"] = "índice de un array del objeto"; 

echo $ngl("rind")->stamp();
?>
```
<br />

**index.html**
``` html
<b>Impresión de variables con la sintáxis tradicional</b>
{$foo} <br />
{$bar["foo"]} <br />
{$bar["bar"]["foo"]} <br />
{$foobar->bar} <br />
{$foobar->foo["bar"]} <br />

<b>Impresión de variables con la sintáxis Rind</b>
{$foo} <br />
{$bar.foo} <br />
{$bar.bar.foo} <br />
{$foobar->bar} <br />
{$foobar->foo.bar} <br />
```
<br />
En caso de que querramos utilizar un texto que represente una variable, y que éste no sea reemplazado por su valor, debemos utilizar llaves dobles a modo de escape. Esto mismo aplica para las palabras claves dentro de los loops.

``` html
{$foo} = imprime el valor de la variable $foo<br />
{{$foo}} = imprime el texto literal {$foo}
```

Existen otros tipos de datos predefinidos en las plantillas con sus propios atajos para ser impresos, pero los iremos viendo más adelante, en la medida en que avancemos.
<br />
<br />

## Comandos
### Definición de comando
Los comandos en **Rind** se decodifican como métodos PHP en el **archivo.html.rind**, ahora bien, que es un método? En programación un método es un fragmento de código que puede ser llamado o invocado por el programa principal o por otro método para realizar alguna tarea específica.
Supongamos que tenemos el método **licuar** con 3 parámetros: **fruta**, **líquido** y **endulzante**. Cada vez que lo invoquemos, sin importar el tipo de fruta, si usamos agua ó leche, ó si le ponemos o no azúcar, siempre vamos a optener es un licuado.
En **Rind** los comandos tienen la misma estructura de las listas en HTML (`<ul>` y `<ol>`),
donde en lugar de **UL** u **OL** usamos el nombre del comando y donde cada `<li>` es un parámetro.

Los nombres de los comandos están precedidos por la etiqueta **rind:** y cada parámetro por una **@**:

``` html
<rind:NOMBRE_DEL_COMANDO>
    <@parametro1>...</@parametro1>
    <@parametro2>...</@parametro2>
    ...
    <@parametroN>...</@parametroN>
</rind:NOMBRE_DEL_COMANDO> 
```
**IMPORTANTE:**

Por razones de seguridad todos los valores pasados en los parámetros son entrecomillados por el sistema.
<br />

### Otras declaraciones de comandos
Es posible declarar comandos con un sólo parámetro sin especificar el nombre del mismo.
En estos casos se asume que el valor pasado corresponde al parámetro `<@content>`

``` html
<rind:NOMBRE_DEL_COMANDO>
    <@content>...</@content>
</rind:NOMBRE_DEL_COMANDO>

<!-- es equivalente a -->

<rind:NOMBRE_DEL_COMANDO>...</rind:NOMBRE_DEL_COMANDO>
```

También existen determinados comandos no requieren del uso de parámetros, en esos casos su declaración es igual a la de los `<br />` o `<img />`

``` html
<rind:NOMBRE_DEL_COMANDO />
```
<br />

### Normalizadores de datos
El sistema cuenta con normalizadores de datos que pueden ser aplicados a los parámetros. Estos normalizadores preparan los datos para que puedan ser utilizados por los comandos. No siempre son necesarios, todo dependerá del comando.
- **json** trata al valor como un JSON y lo convierte en un array
- **base64** codifica el valor en base64 antes de utilizarlo
- **quotes** permite el uso de comillas dobles
- **math** prepara una expresión para ser utilizada como valor numérico en otro comando
<br />

El modo en el que se aplican los normalizadores es:
``` html
<rind:NOMBRE_DEL_COMANDO>
    <@parametro1 json>...</@parametro1>
    <@parametro1 base64>...</@parametro1>
    <@parametro2 quotes>...</@parametro2>
    <@parametro3 math>...</@parametro3>
</rind:NOMBRE_DEL_COMANDO>
```
<br />

### Tipos de comandos
Existen 3 tipos de comandos en Rind:
- **nativos** son los comandos específicos de Rind
- **nuts** son un conjunto de métodos declarados en PHP por el programador del proyecto
- **php** son las funciones nativas de PHP declaradas en el argumento **php_functions** del objeto **Rind**
<br />

Todos los tipos de comandos comparten la misma estructura, sin embargo hay una pequeña variación en el nombre de los mismos a la hora de invocarlos; esta variación es la que permite al sistema saber de que tipo de comando se trata.Otro punto a tener en cuenta es que tanto en los **nativos** como en los **nuts** es importante respetar el nombre de los parámetros. 
Esto no es así en los comandos **php**, donde sólo es importante el orden en el que se declaran, ya que ese es el orden en el que se pasarán al método final.

**Nativo**
``` html
<rind:NOMBRE_DEL_COMANDO>
    <@parametro1>...</@parametro1>
    <@parametro2>...</@parametro2>
</rind:NOMBRE_DEL_COMANDO>
```

**Nut**
``` html
<rind:nut.NOMBRE_DEL_NUT.NOMBRE_DEL_METODO>
    <@parametro1>...</@parametro1>
    <@parametro2>...</@parametro2>
</rind:nut.NOMBRE_DEL_NUT.NOMBRE_DEL_METODO>

<!-- declaración cuando el nombre del método coincide con el nombre del nut -->
<rind:nut.NOMBRE_DEL_NUT>
    <@parametro1>...</@parametro1>
    <@parametro2>...</@parametro2>
</rind:nut.NOMBRE_DEL_NUT>
```

**php**
``` html
<rind:php.FUNCION_PHP>
    <@parametro1>...</@parametro1>
    <@parametro2>...</@parametro2>
</rind:php.FUNCION_PHP>
```
<br />
<br />

## Variables Anidadas
### Referenciando arrays con variables
Este sea quizás un tema avanzado si recién estás empezando a programar, pero no podemos dejar explicarlo.
De todas maneras, el uso de esta práctica no es tan frecuente en el ámbito de las plantillas, pero es importante que sepas que **Rind** cuenta con esta funcionalidad.
A la hora de utilizar una variable como clave de otra se pueden utilizar la sintáxis tradicional de PHP, 
sintáxis la Rind ó la combinación de ambas.
En lo que se refiere a la sintáxis Rind, se utiliza un único par de llaves **{}** para englobar toda la variable, luego, cada variable "clave" deberá estar entre paréntesis **()**
Supongamos que tenemos por un lado un array con la definición RGB de algunos colores y por el otro uno con las preferencias de color del usuario para el texto y el fondo de su entorno. También pondremos en juego una variable que determina si estamos trabajando con el texto o con el fondo *(esta variable carece de relevancia en 
un entorno de producción, pero es útil a los efectos de este ejemplo)*

**index.php**
``` php
<?php
require_once("config.php");

// definición de colores
$colors = array();
$colors["black"] 	= array("R"=>"00", "G"=>"00", "B"=>"0");
$colors["blue"] 	= array("R"=>"00", "G"=>"00", "B"=>"FF");
$colors["green"] 	= array("R"=>"00", "G"=>"FF", "B"=>"0");
$colors["orange"] 	= array("R"=>"FF", "G"=>"99", "B"=>"00");
$colors["red"] 		= array("R"=>"FF", "G"=>"00", "B"=>"00");
$colors["white"] 	= array("R"=>"FF", "G"=>"FF", "B"=>"FF");
$colors["yellow"] 	= array("R"=>"FF", "G"=>"FF", "B"=>"00");

// preferencias del usuario
$preferences = array();
$preferences["text"] = "red";
$preferences["back"] = "yellow";

// indice actual
$current = "text"; 

echo $ngl("rind")->stamp();
?>
```
<br />

**index.html**
``` html
- Sintáxis tradicional, se respeta la estructura de corchetes y comillas de PHP, cada variable está encerrada entre llaves

{$preferences[{$current}]} <br />
{$colors[{$preferences["back"]}]["R"]} <br />
{$colors[{$preferences[{$current}]}]["B"]} <br />


- Sintáxis Rind, los indices estan separados por puntos, como en javascript, cada nueva variable invocada está entre paréntesis

{$preferences.($current)} <br />
{$colors.($preferences.back).R} <br />
{$colors.($preferences.($current)).B} <br />


- Sintáxis Mixta

{$colors[{$preferences.back}]["R"]} <br />
{$colors[{$preferences.($current)}]["B"]} <br /> 
```
<br />
<br />

## Comandos Anidados
### Como utilizar un comando como parámetro de otro
Al anidar comandos es conveniente declarar todos los parámetros para evitar una lectura erronea.Si tenemos dos comandos que tienen definido un parámetro `<@name></@name>` y los anidamos de la siguiente manera:

``` html
<rind:COMANDO1>
    <rind:COMANDO2>
        <@type>...</@type>
        <@name>...</@name>
        <@value>...</@value>
    </rind:COMANDO2>
</rind:COMANDO1>
```

Rind ignorará todos los parámetros del **COMANDO2** hasta llegar a `<@name></@name>`, y asumirá que corresponde a **COMANDO1**. La correcta manera de declararlos para evitar errores es:
Rind ignorará todos los parámetros del **COMANDO2** hasta llegar a `<@name></@name>`, y asumirá que corresponde a **COMANDO1**. La correcta manera de declararlos para evitar errores es:

``` html
<rind:COMANDO1>
    <@name>
        <rind:COMANDO2>
            <@type>...</@type>
            <@name>...</@name>
            <@value>...</@value>
        </rind:COMANDO2>
    </@name>
</rind:COMANDO1>
```

___
- [Indice](readme.md)
- [Clase 2 - Condicional IF](clase-02.md)

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
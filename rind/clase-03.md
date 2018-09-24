# Clase 3 - Bucles
- [Clase 3 - Bucles](#clase-3---bucles)
    - [Concepto General](#concepto-general)
    - [Comando LOOP](#comando-loop)
    - [LOOP Indicadores](#loop-indicadores)
    - [Indices](#indices)
    - [Anidamiento de Bucles](#anidamiento-de-bucles)
    - [Referencias entre Bucles](#referencias-entre-bucles)
    - [Ejemplo de Bucle Complejo](#ejemplo-de-bucle-complejo)
<br />
<br />

## Concepto General
Un bucle es una rutina que utilizado para repetir una acción sin tener que escribir varias veces el mismo código, lo que ahorra tiempo, procesos y deja el código más claro. Supongamos que queremos mostrar en pantalla un listado de colores:

``` php
*** index.php ***

<?php
require_once("config.php");

// definición de colores
$colors     = array();
$colors[0]     = "black";
$colors[1]     = "blue";
$colors[2]     = "green";
$colors[3]     = "orange";
$colors[4]     = "red";
$colors[5]     = "white";
$colors[6]     = "yellow";

echo $ngl("rind")->stamp();
?>
```

``` php
*** index.html ***

{$colors.0}<br />
{$colors.1}<br />
{$colors.2}<br />
{$colors.3}<br />
{$colors.4}<br />
{$colors.5}<br />
{$colors.6}<br />
```

Pero que pasaría si el listado tuviese 256 colores ó si desconocieramos la cantidad?
Es en estos casos es en donde se utilizan los bucles. Un bucle nos permite recorrer el origen de los datos, aplicando un conjunto de acciones en cada vuelta.


## Comando LOOP
Este método puede ser utilizado para recorrer todo tipo de arrays, objetos de datos, objetos OWL y sucesiones númericas. LOOP dará una vuelta por cada índice, fila o incremento que encuentre en el origen de datos **<@source>**, y en cada vuelta ejecutará el contenido del parámetro **<@content>**. Opcionalmente, en caso de que el origen de datos estuviese vacío, se ejecutará el código de **<@empty>**.

Siguiendo con el ejemplo de los colores:
``` html
*** index.html ***

<rind:loop>
    <@source>{$colors}</@source>
    <@content>
        {#current}<br />
    </@content>
</rind:loop>
```
<br />
<br />

## LOOP Indicadores
Sin importar el tipo de origen de datos, en cada vuelta dada por **rind::loop** estarán disponibles una serie  de indicadores. Estos indicadores serán invocados con la sintáxis **{#nombre_indicador}** o **{nombre_bucle.#nombre_indicador}** para el caso de los bucles anidados.

Listado de indicadores:
- **data** datos recogidos en la vuelta actual
- **current** primer elemento de los datos de la vuelta actual, en ocasiones puede coincidir con el valor de **data**
- **first** retorna **1** sólo en la primer vuelta
- **from** inicio del loop (comienza en 0)
- **key** nombre de la clave activa (solo en arrays)
- **numrows** número total de vueltas
- **last** retorna **1** sólo en la última vuelta
- **limit** cantidad de vueltas del loop
- **line** número de la vuelta actual, comenzando por el 1
- **lines** alias de **numrows**
- **numrow** número de la vuelta actual, comenzando por el 0
- **odd** retorna **0** si la vuelta es par y **1** si es impar
- **parity** retorna **even** si la vuelta es par y **odd** si es impar
<br />

Seguimos con el caso de los colores
``` html
*** index.html ***

<style type="text/css">
    .bg-even { background-color: #FFFFFF; }
    .bg-odd { background-color: #BBBBBB; }
</style>

<rind:loop>
    <@source>{$colors}</@source>
    <@content>
        <div class="bg-{#parity}">{#current} es el color {#line} de {#lines}</div>
    </@content>
</rind:loop>
```
<br />
<br />

## Indices
Cuando trabajamos con bucles, la información del origen de datos correspondiente a la vuelta activa, es convertida en un array asociativo y alamcenada en el indicador **{#data}**.
Luego, para invocar los indices deberemos colocarlos entre llaves: **{nombre_indice}**

Tomemos como ejemplo las puntuaciones alcanzadas por un grupo de usuarios en un juego.

``` php
*** index.php ***

<?php
require_once("config.php");

// puntajes
$users     = array();
$users[] = array("top"=>1, "username"=>"B-L-A-K-E", "score"=>872195);
$users[] = array("top"=>2, "username"=>"DrWho", "score"=>814751);
$users[] = array("top"=>3, "username"=>"Th3Ev1l", "score"=>759725);
$users[] = array("top"=>4, "username"=>"Mountain666", "score"=>679134);
$users[] = array("top"=>5, "username"=>"ElBarto", "score"=>541455);

echo $ngl("rind")->stamp();
?>
```

``` html
*** index.html ***

<style type="text/css">
    .bg-even { padding:8px; background-color: #EEEEEE; }
    .bg-odd { padding:8px; background-color: #BBBBBB; }
</style>

<rind:loop>
    <@source>{$users}</@source>
    <@content>
        <div class="bg-{#parity">{#line} - {score} - {username}</div>
    </@content>
</rind:loop>
```
<br />
<br />

## Anidamiento de Bucles
Los bucles cuentan con un parámetro `<@name>`, que si bien no es necesario cuando declaramos un bucle simple, 
es util a la hora de anidar bucles, ya que se puede utilizar para identificar el origen de los índices.
Si no se especifica un nombre de bucle, el sistema asumirá que se trata del bucle principal.
Siguiendo con el ejemplo de los puntajes, supongamos que tenemos un indice con los nombres de los ultimos 3 personajes con los que jugó el usuario.

``` php
*** index.php ***

<?php
require_once("config.php");

// puntajes
$users     = array();
$users[] = array("top"=>1, "username"=>"B-L-A-K-E", "score"=>872195, "characters"=>array("RYU","RYU","RYU"));
$users[] = array("top"=>2, "username"=>"DrWho", "score"=>814751, "characters"=>array("KEN","RYU","KEN"));
$users[] = array("top"=>3, "username"=>"Th3Ev1l", "score"=>759725, "characters"=>array("BLANKA","ZANGIEF","HONDA"));
$users[] = array("top"=>4, "username"=>"Mountain666", "score"=>679134, "characters"=>array("ZANGIEF","ZANGIEF","ZANGIEF"));
$users[] = array("top"=>5, "username"=>"ElBarto", "score"=>541455, "characters"=>array("GUILE","GUILE","KEN"));

echo $ngl("rind")->stamp();
?>
```

``` html
*** index.html ***

<style type="text/css">
    .bg-even { padding:8px; background-color: #EEEEEE; }
    .bg-odd { padding:8px; background-color: #BBBBBB; }
</style>

<rind:loop>
    <@source>{$users}</@source>
    <@content>
        <div class="bg-{#parity}">
            <b>{username}</b>
            <ul>
                <rind:loop>
                    <@name>chars</@name>
                    <@source>{characters}</@source>
                    <@content>
                        <li>{chars.#current}
                    </@content>
                </rind:loop>
            </ul>
        </div>
    </@content>
</rind:loop>
```
<br />
<br />

## Referencias entre Bucles
También es posible referenciar los bucles utilizando las palabras claves **parent** y **self**. Donde la primera referencia al bucle inmediatamente superior y la segunda al bucle actual.
Supongamos que en 3 bucles anidados y en todos existe un campo llamado **name**, si nos paramos en el bucle mas anidado, las referencias serian:

- **{name}** = campo name del bucle superior
- **{parent.name}** = campo name del bucle del medio
- **{self.name}** = campo name del bucle inferior

En el siguiente ejemplo supone un nivel superior a los usuarios, donde se agrupan los mismos por juego.
Y se busca imprimir el último personaje con el que jugó.

``` php
*** index.php ***

<?php
require_once("config.php");

// puntajes
$aGames		= array();
$aGames[]	= array("game"=>"Street Fighter II",
					"users" => array(
						array("top"=>1, "username"=>"B-L-A-K-E", "score"=>872195, "characters"=>array("KEN","RYU","RYU")),
						array("top"=>2, "username"=>"DrWho", "score"=>814751, "characters"=>array("KEN","RYU","KEN")),
						array("top"=>3, "username"=>"Th3Ev1l", "score"=>759725, "characters"=>array("BLANKA","ZANGIEF","HONDA"))
					)
				);

$aGames[]	= array("game"=>"Street Fighter III",
				"users" => array(
					array("top"=>1, "username"=>"DrWho", "score"=>814751, "characters"=>array("KEN","RYU","KEN")),
					array("top"=>2, "username"=>"Mountain666", "score"=>679134, "characters"=>array("ZANGIEF","RYU","ZANGIEF")),
					array("top"=>3, "username"=>"ElBarto", "score"=>541455, "characters"=>array("GUILE","GUILE","KEN"))
				)
			);

echo $ngl("rind")->stamp();
?>
```

``` html
*** index.html ***

<rind:loop>
    <@source>{$aGames}</@source>
    <@content>
        <rind:loop>
            <@source>{users}</@source>
            <@content>
                <rind:loop>
                    <@source>{self.characters}</@source>
                    <@limit>1</@limit>
                    <@content>
                        <div>
                            Juego {game}<br />
                            Usuario: {parent.username}<br />
                            Personaje: {self.#current}<br />
                        </div>
                    </@content>
                </rind:loop>
            </@content>
        </rind:loop>
    </@content>
</rind:loop>
```

&nbsp;

## Ejemplo de Bucle Complejo
Este caso utiliza los datos del ejemplo anterior, pero el bucle utiliza más niveles de anidamiento.

``` php
*** index.php ***

<?php
require_once("config.php");

// puntajes
$aGames		= array();
$aGames[]	= array("game"=>"Street Fighter II",
					"users" => array(
						array("top"=>1, "username"=>"B-L-A-K-E", "score"=>872195, "characters"=>array("KEN","RYU","RYU")),
						array("top"=>2, "username"=>"DrWho", "score"=>814751, "characters"=>array("KEN","RYU","KEN")),
						array("top"=>3, "username"=>"Th3Ev1l", "score"=>759725, "characters"=>array("BLANKA","ZANGIEF","HONDA"))
					)
				);

$aGames[]	= array("game"=>"Street Fighter III",
				"users" => array(
					array("top"=>1, "username"=>"DrWho", "score"=>814751, "characters"=>array("KEN","RYU","KEN")),
					array("top"=>2, "username"=>"Mountain666", "score"=>679134, "characters"=>array("ZANGIEF","RYU","ZANGIEF")),
					array("top"=>3, "username"=>"ElBarto", "score"=>541455, "characters"=>array("GUILE","GUILE","KEN"))
				)
			);

echo $ngl("rind")->stamp();
?>
```

``` html
*** index.html ***

<rind:loop>
    <@source>{$aGames}</@source>
    <@content>
        <rind:loop>
            <@source>{users}</@source>
            <@content>
                <rind:loop>
                    <@source>{self.characters}</@source>
                    <@limit>1</@limit>
                    <@content>
                        <div>
                            Juego {game}<br />
                            Usuario: {parent.username}<br />
                            Personaje: {self.#current}<br />
                        </div>
                    </@content>
                </rind:loop>
            </@content>
        </rind:loop>
    </@content>
</rind:loop>
```
<br />
<br />
<br />

Para ver la descripción completa del comando ver la [ayuda](commands.md#loop)

---
- [Clase 2 - Condicional IF](clase-02.md)
- [Clase 4 - Seteo de Variables](clase-04.md)
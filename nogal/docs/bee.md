# Bee

[Conceptos y Fundamentos](#conceptos-y-fundamentos)  
[Sintásis](#sintaxis)  
[Comandos](#comandos)  
[Ayuda en Terminal](#ayuda-en-terminal)  
[Ejemplos de Uso](#ejemplos-de-uso)  


## Conceptos y Fundamentos
**Bee** es el interprete de **nogal**, y nos da acceso a todos métodos públicos de cualquier objetos del framework, utilizando cadenas de texto plano. Además cuenta con algunas sentencias básicas de programación que ayudan a ejecutar secuencias de códigos. Al poder almacenar el código **bee** en una cadena de texto, este puede estar guardado en una variable, un archivo ó en una base de datos, y podemos utilizarlo almacenar rutinas de mantenimiento o instaladores.

**bee** se puede ejecutar desde un archivo **php** o desde la misma línea de comandos. Para ejectuarlo desde **php** la sintaxis es:
```php
<?php
require_once("config.php");
echo $ngl("bee")->bzzz("fn imya");

// que sería equivalente a 

require_once("config.php");
echo $ngl()->imya();

?>
```

Para ejecutarlo desde la línea de comandos se puede utilizar el archivo **bee** (sin extención) disponible el el directorio raíz de **nogal** o dentro de sus estructuras de ejemplo (nogal/assets/templates/structures/)

```bash
$ php bee fn imya
```
&nbsp;
___

## Sintásis
La ejecución de sentencias es secuencial, cada comando y/o llamada a un objeto deberá realizarse en una nueva línea. El llamado de objetos, tiene la siguiente sintáxis:
> &nbsp;
> **objeto** **_metodo_** _[argumentos]_
> o
> **objeto**:**_metodo_** _[argumentos]_
> &nbsp;

Donde **objeto** es un objeto de **nogal**, **metodo** uno de los métodos de ese objeto y los argumentos una cadena literal, un **array** en formato **JSON** o múltiples valores separados por un espacio.

Ademas **bee** soporta el uso de variables, constantes y algunas estructuras de control básicas.
- **variables** = {$nombre_de_variable}
- **constantes** = {@NOMBRE_DE_CONSTANTE}
- **-\$:** = variable de sistema en la que se almacena el resultado de la última sentencia ejecutada. 
&nbsp;
___

## Comandos
|Comando|Descripción|
|---|---|
|**@clear**|Limpia el buffer de salida|
|**@exit**|Aborta la excusión en curso|
|**@if**|Permite la ejecuión condicional de sentencias cuando el resultado del mismo es **true**. El bloque de sentencias finaliza con **endif**.<br />Limitaciones: <ul><li>no se cuenta con bloque **else**</li><li>cuando las variables y/o constantes sean cadenas, deberán ser entrecomilladas</ul>|
|**@ifnot**|Idéntico **@if**, sólo que las sentencias se ejecutan cuando el condicional retorna **false**|
|**@get**|Obtiene el valor de una variable. Principalmente útil en el caso de arrays|
|**@launch**|Redirige a la URL pasada. Sólo disponible en la versión web|
|**@loop**|Iterador de código (bucle). Se comporta como un foreach de php.<br />Origenes de datos<ul><li>arrays</li>|
|**@php**|Permite la ejecución de una función php|
|**@print**|Imprime una expresión en pantalla|
|**@splitter**|Setea el separador de cadenas. Por defecto: \\t|
|**@set**|Setea una variable. Si el valor|

Al ejecutar **bee** en la línea de comandos existen 2 modos, ejecución lineal y en bloque.  
El modo directo se ejecuta al presionar **ENTER**. Opcionalmente puede ejecutarse en modo silencioso y tambien multiples comandos entrecomillados y separados por uno o varios caracteres.  
**IMPORTANTE:** el separador no debe repetirse dentro de la cadena de comandos, mas que para separar a estos entre sí.  

El modo en bloque se inicia al ejecutar
```bash
$ php bee
```
y finaliza con la línea 
```bash
bzzz
```
seguida de un **ENTER**
&nbsp;
___
&nbsp;

## Ayuda en Terminal
Si se ejecuta **bee** con el flag -h, se mostrará una ayuda rápida en pantalla:
```bash
$ php bee -h

--------------------------------------------------------------------------------
 nogal bee -S:
--------------------------------------------------------------------------------
Uso:
	$ php bee [OPTIONS] <COMMAND>
	$ php bee [OPTIONS] -m"<COMMAND><SEPARATOR><COMMAND>"
	$ php bee [OPTIONS] (modo consola. para finalizar, usar bzzz)

Comandos:
	Las sintaxis de los comandos pueden ser:
		<OBJECT> <METHOD> [<ARGUMENT> ... <ARGUMENT>]
		<OBJECT>:<METHOD> [<ARGUMENT> ... <ARGUMENT>]

Opciones:
	-e    variable de entorno key=value
	-h    ayuda
	-m    permite ejecutar multiples comandos en línea
	-m@   multiples comandos con un separador distinto a |, en este caso, @
	-r    retorna el valor crudo de la respusts (raw)
	-f    carga el código a ejecutar, desde un archivo
	-s    modo silencioso, no tiene salida
	-v    nogal versión

Ejemplo de ejecución
  # en línea
	$ php bee fn:imya


  # en bloque
	$ php bee
	file load https://cdn.upps.cloud/json/material-design-colors.json
	-$: read
	shift convert ["-$:", "json-ttable"]
	bzzz
```
&nbsp;
___

## Ejemplos de Uso
A continuación se ponen algunos ejemplos prácticos.

**El siguiente ejemplo es una muestra del uso de todos los comandos**
```bash
# se setea un objeto con datos
@set data [{"name":"bart", "age":"10", "gender":"M"}, {"name":"lisa", "age":"8", "gender":"F"}, {"name":"homero", "age":"34", "gender":"M"}]

# impresión en pantalla
@print ""
@print "--------------------"
@print " Lista de Invitados "
@print "--------------------"

# iteración del objeto de datos
@loop {$data}
	# guardamos el primer elemento del LOOP en la variable ROW
	@set row -$:

	# obtenemos el valor de un indice y lo guardamos en una variable
	@get row name
	@php strtoupper -$:
	@set name -$:

	@get row age
	@set age -$:

	# uso de condiconales
	@if -$: < 13
		@get row gender
		@set gender -$:

		# condicionales anidados
		@if "-$:" == "M"
			@print "{$name} es un niño de {$age} años"
		endif
		@ifnot "{$gender}" == "M"
			@print "{$name} es una niña de {$age} años"
		endif
	endif

	@if {$age} > 13
		@print ""
		@print "------------------------------------"
		@print " Alerta!! hay un adulto en el grupo"
		@print " Se llama {$name} y tiene {$age} años"
		# se aborta el proceso
		@exit "------------------------------------"
	endif

endloop

# limpieza de buffer
@clear
```
&nbsp;

**Ejemplo completo de un instalador**  
[https://github.com/hytcom/bithive/blob/master/install.bee](https://github.com/hytcom/bithive/blob/master/install.bee)

&nbsp;

**Lectura de base de datos a formato CSV (php)**
```php
<?php

require_once("config.php");

echo $ngl("bee")->bzzz(<<<BEE
mysql args [{"host":"mariadb", "base":"test", "user":"myuser", "pass":"mypass"}]
-$: connect
-$: query "SELECT * FROM sometable"
-$: getall
shift convert ["-$:", "array-csv"]
BEE
);
```
&nbsp;

**lista de archivos del directorio actual**
```bash
$ php bee files ls . :null: info
```
&nbsp;

**Impresión de un JSON remoto como tabla de texto**
```bash
$ php bee
file load https://cdn.upps.cloud/json/material-design-colors.json
-$: read
shift convert ["-$:", "json-ttable"]
bzzz
```
&nbsp;

**Verifica si un archivo existe**
```bash
$ php bee
@php file_exists foobar.txt
@set file -$:
@if {$file}
	@print "file exists"
endif
@ifnot {$file}
	@print "file not exists"
endif
bzzz
```
&nbsp;

**Iteraciones anidadas**
```bash
$ php bee
@loop 1:5
	@print "ROW -$:"
	@loop ["A","B","C"]
		@print "  COL -$:"
	endloop
endloop
bzzz
```
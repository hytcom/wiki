<img src="https://cdn.abcontenidos.com/images/nogal/nogal.svg" width="150" alt="nogal"/><br />
## Indice de contenidos

- [Introducción](#introducción) 
- [Instalación](#instalación) 
- [Primeros Pasos](#primeros-pasos)
- [Secuencia de Inicio](#secuencia-de-inicio)
- [Objecto $ngl](#objeto-ngl)
- [Objetos](objects.md)
- [Ejemplo Práctico](#ejemplo-práctico)
- [Bee](#bee)
- [Rind](#rind)
- [Licencia](#licencia)
&nbsp;

## Introducción
**nogal** es un framework PHP de código abierto pensado para que todos puedan desarrollar aplicaciones sin necesidad de tener grandes conocimientos de programación (the most simple PHP Framework)  

Su estructura está pensada para que todo aquel que tenga conocimientos básicos de programación, como que es una **variable**, un **if** y un **bucle**, pueda desarrollar aplicaciones sin mayores dificultades. Esto es gracias, entre otras cosas, a su poderoso sistema de templates llamado [rind](../rind) que permite al desarrollador comunicarse con cualquiera de los objetos del framework desde el HTML, utilizando una sintáxis basada en XML, JS y JSON.

A pesar de ser fácil de utilizar y de tener una curva de aprendizaje muy rápida, **nogal** es también una poderosa herramienta para el desarrollador experimentado, ya que respeta los estandares del modelo MVC, tiene una sintáxsis muy limpia en el uso de PHP y un interprete denominado [Bee](#bee) que le permitirá ejecutar cualquier objeto del framework desde una terminal.

&nbsp;

## Instalación
En el siguiente enlace encontrarás los pasos para la instalación de **nogal** [Como instalar nogal](install/)

Para realizar la instalación es necesario tener instalado PHP 5.6 o superior, con los siguientes módulos:
||||||
|-|-|-|-|-|
|bcmath|Core|ctype|curl|date|
|dom|fileinfo|filter|ftp|gd|
|hash|iconv|json|libxml|mbstring|
|mysqli|mysqlnd|openssl|pcre|Phar|
|posix|readline|Reflection|session|SimpleXML|
|sodium|SPL|sqlite3|standard|tokenizer|
|xml|xmlreader|xmlwriter|zip|zlib|

&nbsp;

## Primeros Pasos
Para arrancar el framework unicamente hace falta incluir el archivo **nogal.php** ubicado en la carpeta **nogal**.  
Hay distintas maneras de hacer esta inclusión, puede ser:
- **directo** = donde se asumirán todas las configuraciones por defecto.
- **customizado** = utilizando un archivo **config.php** en donde se declaran las [contantes](docs/constants.md) previa a la inclusión de **nogal.php**
- **prickout** = que permite no sólo el uso de un **config.php**, sino que además habilita que los archivos del cógido fuente estén fuera de la carpeta pública. Ver **re-prickout** para hacer redireccionamientos.

### Uso directo
Sólo hace falta incluir el archivo **nogal.php**. En este caso, el framework arrancará con todas las configuraciones por default.  
Podemos ver los valores adoptados por las constantes **NGL...** utilizando el método **constants**
```php
<?php

require_once("/var/nogal/nogal.php");
print_r($ngl()->constants());

?>
```
---
### Arranque customizado
Para poder configurar el framework antes de su arranque, podemos utilizar un archivo en donde definir las constantes **NGL**
```php
<?php

require_once("config.php");
print_r($ngl()->constants());

?>
```  
---
### Uso de prickout.php
El uso de **prickout** permite que los archivos del cógido fuente estén fuera de la carpeta pública, agregando asi una capa de seguridad.  
Para ello es necesario definir en la constante **NGL_PATH_PRICKOUT** el path de la carpeta principal que alberga los archivos fuentes y utilizar redirecciones en el servidor, como por ejemplo, usando rewrite mode en .htaccess  
Cuando se utilice **prickout**, **nogal** intentará ejecutar el **path** siguiendo estas directivas:
- si no se encuentra ```$_SERVER["REDIRECT_URL"]``` o ```$_SERVER["REDIRECT_SCRIPT_URL"]```, arrojará error 403
- si el **path** es de un archivo **.php** intentará ejecutarlo
- si el **path** termina en **/** intentará ejecutar un archivo **index.php**
- se intentará enviar los datos del **path** al archivo **\_\_container.php** de la ruta del **path**
- se intentará enviar los datos del **path** al archivo **\_\_container.php** de la carpeta **NGL_PATH_PRICKOUT**
- se intentará cargar el **path**
- arrojará error 404

```sh
# .htaccess

<IfModule mod_rewrite.c>
	RewriteEngine On
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteRule ^(.*)$ index.php [QSA,L]
</IfModule>
```

```php
# index.php

<?php

require_once("config.php");
require_once(NGL_PATH_FRAMEWORK."/prickout.php");

?>
```

Cuando se utilice el **prickout** no es necesario incluir los archivos de configuración y arranque en de cada del proyecto, ya que estos vienen heredados por **prickout.php**. Sin embargo es aconsegable utilizar la sintaxis: 

```php
# foobar.php

<?php defined("NGL_SOWED") || exit();

print_r($ngl()->constants());

?>
```
Que verifica por medio de la existencia de la constante **NGL_SOWED** que el archivo esté siendo ejecutado dentro del entorno de **nogal** y no de manera directa. 

---
### Uso de re-prickout.php
También es posible hacer redirecciones atraves de **prickout** utilizando el arvhivo **re-prickout.php** de la carpeta **NGL_PATH_PROJECT**.

```php
# re-prickout.php

<?php defined("NGL_SOWED") || exit();

switch(true) {
	// http://www.dominio.com/foo/bar.php => http://www.dominio.com/index.php?id=foobar
	case ($_SERVER["REDIRECT_URL"] == "/foo/bar.php"):
		$NGL_REPRICKOUT_URL = "/index.php?id=foobar";
		break;

	// http://www.dominio.com/foo/1234 => http://www.dominio.com/foo.php?id=1234
	case (substr($_SERVER["REDIRECT_URL"], 0, 5)=="/foo/"):
		$NGL_REPRICKOUT_URL = explode("/", $_SERVER["REDIRECT_URL"], 3);
		$NGL_REPRICKOUT_URL = "/foo.php?id=".$NGL_REPRICKOUT_URL[2];
		break;
	
	// http://www.dominio.com/ws/getinfo => http://www.dominio.com/ws/ws/webservice.php con datos POST
	case ($_SERVER["REDIRECT_URL"] == "/ws/getinfo"):
		$NGL_REPRICKOUT_REQUEST	= array("method"=>"getinfo", "body"=>file_get_contents("php://input"));
		$NGL_REPRICKOUT_URL		= NGL_URL."/ws/webservice.php";
		break;
}

if(isset($NGL_REPRICKOUT_URL)) {
	if(isset($NGL_REPRICKOUT_REQUEST)) {
		$req = $ngl("file")->load($NGL_REPRICKOUT_URL);
		$options = array(
			"CURLOPT_CUSTOMREQUEST" => "POST",
			"CURLOPT_POST" => 1,
			"CURLOPT_POSTFIELDS" => $NGL_REPRICKOUT_REQUEST
		);

		echo $req->read(null, $options);
	} else {
		header("location:".$NGL_REPRICKOUT_URL);
	}
	
	exit();
}

?>
```

&nbsp;

## Secuencia de Inicio
En el proceso de carga de **nogal.php**, sin importar el método empleado, se intentarán cagar los siguientes archivos, que si bien estos archivos no son obligatorios para el arranque y uso del framework, es importante tener presente su orden de carga para comprender donde deben configurarse las distintas instancias.

- **NGL_PATH_PROJECT/session.php** = Define el tipo de manejo que se le dará las sessiones.
- **NGL_PATH_PROJECT/settings.php** = Carga las configuraciones necesarias para el funcionamiento del proyecto, como variables de entorno ó las directivas de seguridad [alvin](docs/alvin.md) 
- **NGL_PATH_FRAMEWORK/alvin.php** = Si se cumple la condicion: ```(NGL_ALVIN!==null || NGL_AUTHORIZED_IPS!==null)``` se carga el objeto [alvin](docs/alvin.md), encargado de la seguridad en todos los ambitos del framework.

&nbsp;

## Objeto $ngl
Una vez cargado el framework todos sus objetos serán accesibles desde la variable **\$ngl**, y su utilización es realmente fácil de comprender. Todos los objetos del **nogal** se invocan dentro de la variable-función **\$ngl()**. Esta invocación retorna el objeto solicitado, de manera que todos sus métodos quedan accesibles inmediatamente utilizando el operador **->**.  
Existen dos tipos de objeto:  

- **feeder** = son extensiones del objeto principal, realizan operaciones particulares, que no necesitan un instanciamiento. Funcionan como librerías de herramientas.
- **branch** = son aquellos que administran grupos concretos de datos y pueden requerir varias instancias simultaneas

Por ejemplo
- **nglFiles** = lista y ejecuta acciones entre lotes de archivos o rutas. *(feeder)*
- **nglFile** = contiene la información de un archivo específico y ejecuta acciones sobre el mismo. *(branch)*

Para ver un listado completo de los objetos disponibles se debe ejecutar el método **availables**.

```php
<?php

require_once("config.php");
$ngl()->dumphtml($ngl()->availables());

?>
```
---
### Objetos Feeder
- Se invocan utilizando unicamente su nombre
- Siempre retornan un resultado concreto (string, array, boolean, int)
- Los argumentos a cada uno de sus métodos deberán se pasados al momento de la invocación

```php
<?php
$ngl("OBJETO")->METODO($ARG1, $ARG2, ..., $ARN);

# por ejemplo
$ls = $ngl("files")->ls("public_html/files", "*.jpg", "info");
print_r($ls);

?>
```
---
### Objetos Branch
- Al igual a los feeders, se pueden invocar sólo con su nombre. Sin embargo, para cuando sea necesario identificar las instancias de un mismo objeto, existe la posibilidad de añardirles un identificador, separando a este por un punto. También puede utilizarse el punto sólo, en cuyo caso el sistema asignará uno aleatorio, que luego podrá ser obtenido mediante el método **\_\_me\_\_** del objeto. 
- Mayormente retornarán una referencia al propio objeto, lo que permite ejecutar multiples métodos en una sola llamada. Aunque dependiendo del caso, podrían retornan un resultado concreto (string, array, boolean, int)
- Estos objetos cuentan con argumentos y atributos. Los atributos son de sólo lectura y van siendo seteados por el objeto en función de los argumentos pasados y/o de los métodos ejecutados. Por otro lado, los argumentos permiten configurar el objeto y ejecutar sus métodos. Existen **MULTIPLES** maneras de configurar y ejecturar los métodos de un objeto **branch**:

  - **default** = son los configurados directamente en la clase
  - **archivo.conf** = cuando exista un archivo **OBJETO.conf**\* en la carpeta **NGL_PATH_CONF**, el objeto se iniciará con dicha configuración.
  - **archivo.conf específico** = igual al anterior, con la diferencia es que el nombre del archivo deberá ser pasado por referencia a la hora de iniciar el objeto, separandolo del nombre utilizando un pipe **|**. Esto es util cuando, por ejemplo, se trabaja con el objeto **mysql** en dos o más bases de datos diferentes.
  - **function syntax** = ya en tiempo de ejecución se llama al argumento como si fuese un método y se pasa el valor como un único argumento.
  - **variable syntax** = ya en tiempo de ejecución se asigna el valor al argumento utilizando la sintaxis de asignación de variables.
  - **método args** = por medio de este método puede pasarse la configuración de varios argumentos simultanemente, en forma de un array asociativo.

\* para generar un archivo de configuración deberá ejecutarse el siguiente método
```php
<?php

$ngl("<OBJETO>")->__configfile__();

?>
```

Veamos unos ejemplos aplicados sobre el objeto **mysql**

```php
<?php

# default / archivo.conf
$db = $ngl("mysql")->query("SELECT * FROM `tablename`");

?>
```

```php
<?php

# Usando dos Instancias
## archivo conf
$db1 = $ngl("mysql.local")->query("SELECT * FROM `tablename`");

## archivo conf específico
$db2 = $ngl("mysql.remote|mysql-slave.conf")->query("SELECT * FROM `tablename`");

?>
```

```php
<?php

# archivo.conf
$db = $ngl("mysql");

# function syntax
$db->base("test1");
$db->connect();

# variable syntax
$db->sql= "SELECT * FROM `tablename`";
$db->query();

# método args
$db->args(
	"table" => "tablename",
	"values" => $aData
);
$db->insert();

?>
```

```php
<?php

# ejemplo completo
$db = $ngl("mysql")
	->base("test1")
	->user("root")
	->pass("asd123")
	->connect()
	->query("SELECT * FROM `tablename`")
;

# lo que seria equivalente a
$db = $ngl("mysql")
$db->base = "test1";
$db->user = "root";
$db->pass = "asd123";
$db->sql = "SELECT * FROM `tablename`";
$db->connect()->query();

?>
```
&nbsp;

Para conocer la configuración actual de un objeto se puede utilizar el método **\_\_info\_\_**. O tambien **\_\_whoami\_\_**, que no sólo retornará los argumentos y atributos, sino que también mostrará los métodos disponibles.

```php
<?php

var_export($ngl("mysql")->__info__());

var_export($ngl("mysql")->__whoami__());

?>
```

&nbsp;

## Ejemplo Práctico
En el siguiente ejemplo muestra como extraer inforamción de una base de datos para luego enviarla por correo, siempre que el resultado de la consulta no sea *vacío*.  
Los pasos a seguir son:
- Extraer un conjunto de datos de una base mysql. Objeto [mysql](docs/mysql.md)
- Guardar el resultado de esa consulta en un archivo CSV. Objeto [shift](docs/shift.md)
- Enviar ese archivo por e-mail, como archivo adjunto. Objeto [mail](docs/mail.md)

Se asume que los objetos del ejemplo toman los datos de configuración de sus respectivos **archivos.conf**

```php
<?php

require_once("config.php");

// consulta
$data = $ngl("mysql")->query("SELECT * FROM `ventas_diarias` WHERE MONTH(`fecha`) = MONTH(NOW())");

if($data->rows()) {
	// salida como CSV
	$sCSV = $ngl("shift")->convert($data->getall(), "array-csv");

	// envio de correo
	$ngl("mail")->connect()->login()
		->subject("Informe de ventas por día")
		->attachContent($sCSV, "informe.csv")
		->message("Se adjunta el informe del mes")
		->send("foo@foobar.com")
	;
	echo "El informe se envió con éxito";
} else {
	echo "No se hallaron datos para enviar";
}

?>
```
**NOTA:** Si la distribución cuenta con las librerías **grafts** instaladas, en lugar de generar un archivo **csv** se podrían generar archivos **xlsx**, **pdf**, etc. 

&nbsp;

## Bee
[bee](docs/bee.md) nos brinda acceso a todos métodos públicos de cualquier objetos del framework, utilizando cadenas de texto plano.

```php
<?php

require_once("config.php");
echo $ngl("bee")->bzzz("fn imya");

?>
```

es equivalente a 

```php
<?php

require_once("config.php");
echo $ngl()->imya();

?>
```

Pero entoces, por que usarlo?  

La respuesta es simple, porque con [bee](docs/bee.md) se pueden ejecutar multiples sentencias almacenadas en una cadena de texto, ya sea en una variable, un archivo ó en una base de datos. De esta manera podemos almacenar rutinas ó bien darle al usuario final la posibilidad de crear las que necesite. Eso si, tomando recaudos.  

La estructura de las sentencias es siempre la misma:  

**objeto** **_metodo_** _[argumentos]_

Los argumentos pueden ser una cadena literal o un **array** en formato **JSON**  
En cada sentecia la variable **-$:** contendrá el resultado de la sentencia inmediatamente anterior, sin importar el tipo de dato.

El siquiente ejemplo lee datos de la base y los exporta en formato CSV. 

```php
<?php

require_once("config.php");

echo $ngl("bee")->bzzz(<<<BEE
mysql args [{"host":"172.17.0.3", "base":"test", "user":"root", "pass":"rootoor"}]
-$: connect
-$: query select * from ventas
-$: getall
shift convert ["-$:", "array-csv"]
BEE
);

?>
```
&nbsp;

En la carpeta raíz del framework se encuentra un archivo llamado *bee*, sin extención. Ese archivo es en realidad un archivo *.php* que permite utilizar **bee** en la linea de comandos. Para utilizarlo sólo hay que configurar el archivo de [arranque](#primeros-pasos) de **nogal**; por defecto viene configurado *nogal.php* que se encuentra en la misma carpeta, pero si se copia *bee* en otra ubicación habrá que reapuntarlo.  

[bee](docs/bee.md) en linea de comandos tiene 2 modos de ejecución, lineal y en bloque.  
El modo lineal se ejecuta al presionar **ENTER** y sólo permite una única sentencia

```bash
$ php bee files ls [\".\",null,\"info\"]
```

El modo bloque se inicia con la llamada **php bee** y finaliza con la sentencia **bzzz** seguida de **ENTER**.  

```bash
$ php bee
bee: file load https://cdn.abcontenidos.com/json/material-design-colors.json
bee: -$: read
bee: shift convert ["-$:", "json-ttable"]
bee: bzzz
```

&nbsp;

## Rind
Rind es el sistemas de templates de nogal y está pensado para diseñadores web que tengan algunos conocimientos básicos de programación en javascript o mejor aún en PHP, aunque esto no es excluyente. Está conformado por un conjunto de etiquetas HTML que al ser procesadas son decodificadas en código PHP, lo que permite trabajar con PHP desde HTML, haciendo que el código resultante sea más claro y limpio. 

[Guía completa con los primeros pasos](../rind)  
[Indice de comandos](../rind/commands.md)

&nbsp;

## Licencia
The MIT License (MIT) - Copyright (c) 2016 [hytcom.net](https://hytcom.net/nogal)

Permission is hereby granted, free of charge, to any person obtaining a copy of 
this software and associated documentation files (the "Software"), to deal in 
the Software without restriction, including without limitation the rights to 
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies 
of the Software, and to permit persons to whom the Software is furnished to do 
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all 
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR 
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE 
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER 
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, 
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE 
SOFTWARE.

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2020 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br /> 
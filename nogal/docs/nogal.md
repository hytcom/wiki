# Nogal - Introducción


- [Primeros Pasos](#primeros-pasos)
- [Objecto $ngl](#objeto-ngl)
- [Ejemplo Práctico](#ejemplo-practico)
- [Bee](#bee)

## Primeros Pasos
### Uso directo
Sólo hace falta incluir el archivo **nogal.php**. En este caso, el framework arrancará con todas las configuraciones por default. Podemos ver los valores adoptados por las constantes **NGL...** utilizando el método **constants**
```php
<?php

require_once("/var/nogal/nogal.php");
print_r($ngl()->constants());

?>
```

&nbsp;

### Arranque customizado
Para poder configurar el framework antes de su arranque, podemos utilizar un archivo en donde definir las constantes **NGL**
```php
<?php

require_once("config.php");
print_r($ngl()->constants());

?>
```  

&nbsp;

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

&nbsp;

## Objeto $ngl
Una vez cargado el framework todos sus objetos serán accesibles desde la variable **\$ngl**, y su utilización es realmente fácil de comprender. Todos los objetos del **nogal** se invocan dentro de la variable-función **\$ngl()**. Esta invocación retorna el objeto solicitado, de manera que todos sus métodos quedan accesibles inmediatamente utilizando el operador **->**.  
Existen dos tipos de objeto:  

- **principales** = son extensiones del objeto principal, realizan operaciones particulares, que no necesitan un instanciamiento. Funcionan como librerías de herramientas.
- **instanciables** = son aquellos que administran grupos concretos de datos y pueden requerir varias instancias simultaneas

Por ejemplo
- **nglFiles** = lista y ejecuta acciones entre lotes de archivos o rutas. *(principal)*
- **nglFile** = contiene la información de un archivo específico y ejecuta acciones sobre el mismo. *(instanciable)*

Para ver un listado completo de los objetos disponibles se debe ejecutar el método **availables**.

```php
<?php

require_once("config.php");
$ngl()->dumphtml($ngl()->availables());

?>
```

&nbsp;

### Objetos Principales
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

&nbsp;

### Objetos Instanciables
- Al igual a los principales, se pueden invocar sólo con su nombre. Sin embargo, para cuando sea necesario identificar las instancias de un mismo objeto, existe la posibilidad de añardirles un identificador, separando a este por un punto. También puede utilizarse el punto sólo, en cuyo caso el sistema asignará uno aleatorio, que luego podrá ser obtenido mediante el método **\_\_me\_\_** del objeto. 
- Mayormente retornarán una referencia al propio objeto, lo que permite ejecutar multiples métodos en una sola llamada. Aunque dependiendo del caso, podrían retornan un resultado concreto (string, array, boolean, int)
- Estos objetos cuentan con argumentos y atributos. Los atributos son de sólo lectura y van siendo seteados por el objeto en función de los argumentos pasados y/o de los métodos ejecutados. Por otro lado, los argumentos permiten configurar el objeto y ejecutar sus métodos. Existen **MULTIPLES** maneras de configurar y ejecturar los métedos de un objeto instanciable:

  - **default** = son los configurados directamente en la clase
  - **archivo.conf** = cuando exista un archivo **OBJETO.conf** en la carpeta **NGL_PATH_CONF**, el objeto se iniciará con dicha configuración
  - **archivo.conf específico** = igual al anterior, con la diferencia es que el nombre del archivo deberá ser pasado por referencia a la hora de iniciar el objeto, separandolo del nombre utilizando un pipe **|**. Esto es util cuando, por ejemplo, se trabaja con el objeto **mysql** en dos o más bases de datos diferentes.
  - **function syntax** = ya en tiempo de ejecución se llama al argumento como si fuese un método y se pasa el valor como un único argumento.
  - **variable syntax** = ya en tiempo de ejecución se asigna el valor al argumento utilizando la sintaxis de asignación de variables.
  - **método args** = por medio de este método puede pasarse la configuración de varios argumentos simultanemente, en forma de un array asociativo.

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
```

files ls [".","nogal.*","info"]
bzzz



file load https://cdn.bithive.cloud/json/material-design-colors.json
-$: read
shift convert ["-$:", "json-array"]
@get -$: pink
bzzz


file load https://cdn.bithive.cloud/json/material-design-colors.json
-$: read
bzzz



mysql host 172.17.0.3
-$: base test
-$: user root
-$: pass rootoor
-$: connect
-$: query show tables
-$: getall
shift convert ["-$:", "array-text"]
bzzz


mysql host 172.17.0.3
-$: base test
-$: user root
-$: pass rootoor
-$: connect
-$: query select * from __ngl_owl_log__
-$: getall
shift convert ["-$:", "array-csv"]
bzzz

```


___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2020 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
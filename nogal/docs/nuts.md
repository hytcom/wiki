# Cómo crear NUTS
Los [nuts](https://github.com/arielbottero/wiki/blob/master/nogal/docs/nut.md) son clases php cuyo objetivo es agrupar procedimientos que pueden o no invocar a otros objetos del **nogal**<br />
Estas clases son las *vistas* del modelo, por lo que los nuts no deberían incluir operaciones de escritura, para ello están los [tutores](https://github.com/arielbottero/wiki/blob/master/nogal/docs/tutor.md)

## Declaración
Para declarar un nut  hay que tener presente sólo 2 reglas
- Todos los nuts son heredados del objeto principal
- Los nuts admiten únicamente métodos *protected* y *private*. La declaración de un método *public* anula al nut
- Los métodos accesibles desde el método [run]() del objeto principal son únicamente los **protected**
- Al momento de cargar el nut se ejecuta, de existir, el método **init**, utilizado generalmente para configuraciones iniciales

Otro factor a tener en cuenta, es que a diferencia de los otros objetos, los nuts pueden ser utilizados dentro de las plantillas [rind](https://github.com/arielbottero/wiki/blob/master/nogal/docs/rind.md) como origen de datos, pero por seguridad sólo pueden ejecutarse los métodos listados en el array **$safemethods**

La sintáxsis básica de los nuts es:

```php
<?php

namespace nogal;

class nutNOMBRE_DEL_NUT extends nglNut {

	protected function init() {
		$this->SafeMethods();
	}

	protected function NOMBRE_DEL_NUT($aArguments) {
	}
	.
	.
	.
}
?>
```

Para invocar a otros objetos de **nogal** dentro del nut se debe utilizar
```php
$this->ngl("OBJETO");
```
Estos objetos se comportarán como si los estuvises ejecutando fuera del nut, por lo que tomarán todas las configuraciones del los archivos *.conf* disponibles

### Lectura de información de una base de datos en forma de Array
```php
<?php

namespace nogal;

class nutWeb extends nglNut {

	protected function subcategorias($aArguments) {
		$db = $this->ngl("mysql");
		$nParent = (int)$aArguments["parent"];
		$data = $db->query("SELECT * FROM `categorias` WHERE `parent`='".$nParent."'");
		if($data->rows()) {
			return $data->getall();
		}
		return array();
	}
}
?>
```
Ejecución desde PHP
```php 
<?php defined("NGL_SOWED") || exit();

print_r($ngl("nut.web")->run("subcategorias", array("parent"=>2)));

?>
```

Para ejecutar este método desde las plantillas es necesario que se lo defina como un método seguro
```php
<?php

namespace nogal;

class nutWeb extends nglNut {

	protected function init() {
		$this->SafeMethods("subcategorias", "query");
	}

	protected function subcategorias($aArguments) {
		$db = $this->ngl("mysql");
		$nParent = (int)$aArguments["parent"];
		$data = $db->query("SELECT * FROM `categorias` WHERE `parent`='".$nParent."'");
		if($data->rows()) {
			return $data->getall();
		}
		return array();
	}

	protected function query($aArguments) {
		$db = $this->ngl("mysql");
		$sTable = preg_replace("/[^a-z0-9_]/i", "", $aArguments["table"]);
		$aWhere = $db->escape($aArguments["DATA"]);
		$data = $db->query("SELECT * FROM `".$sTable."` WHERE `sucursal`='".$aWhere["sucursal"]."' AND `estado`='".$aWhere["estado"]."'");
		if($data->rows()) {
			return $data->getall();
		}
		return array();
	}
}
?>
```

Llamada desde plantillas
```html 
<rind:nut.web.subcategorias>
	<@parent>2</@parent>
</rind:nut.web.subcategorias>
```

Si el nut esperase argumentos del tipo DATA
```html 
<rind:nut.web.query>
	<@tabla>sventas</@tabla>
	<@data-sucursal>Central</@data-sucursal>
	<@data-estado>pagada</@data-estado>
</rind:nut.web.query>
```

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
# tutor
Como ya digimos, los tutores son controlados por el objeto [tutor](tutor.md), pero para que esto funcione debe existir un repositorio de tutores donde el objeto principal vaya a buscarlos.
Es repositorio puede ser cualquier carpeta dentro del sistema, y su ruta deberá estar definida en la constante **NGL_PATH_TUTORS**

___

## Creando Tutores
Los **tutores** son clases php cuyo objetivo es controlar la escritura de datos dentro del sistema, ya sea en base de datos, en el sistema de archivo (copiando, modificando y borrando archivos) o enviando correos.<br />
Estas clases son los *controladores* del modelo, por lo que los **tutores** no deberían incluir operaciones de lectura, para ello están los [nuts](nut.md), más que las necesarias para poder realizar las de escritura.

## Declaración
Para declarar un **tutor** hay que tener presente:
- Todos los **tutores** son heredados del objeto principal
- Los **tutores** admiten únicamente métodos *protected* y *private*. La declaración de un método *public* anula al **tutor**
- Los métodos accesibles desde el método [run](tutor.md#run) del objeto principal son únicamente los **protected**
- Al momento de cargar el tutor se ejecuta, de existir, el método **init**, utilizado generalmente para configuraciones iniciales
- Al comienzo de cada archivo de tutor y antes de la declaración del mismo, se deberá colocar la línea ```php $this->TutorCaller(self::requirer());``` Esto evita que la clase pueda ser incluída por cualquier otro archivo que no sea el objeto principal [tutor](tutor.md).

La sintáxsis básica de los **tutores** es:

```php
<?php

namespace nogal;
$this->TutorCaller(self::requirer());

class tutorOBJECTNAME extends nglTutor {

	private $foo;
	private $bar;
	
	protected function init($aArguments=array()) {
		.
		.
		.
	}

	protected function METODO($aArguments=array()) {
		.
		.
		.
	}
}
?>
```

Para invocar a otros objetos de **nogal** dentro del **tutor** se debe utilizar
```php
self::call("OBJETO");
```
Estos objetos se comportarán como si los estuvises ejecutando fuera del **tutor**, por lo que tomarán todas las configuraciones del los archivos *.conf* disponibles.  


### Escritura de información de una base de datos utilizando [owl](owl.md)
```php
<?php

namespace nogal;
$this->TutorCaller(self::requirer());

class tutorWeb extends nglTutor {

	private $db;
	private $owl;
	
	protected function init($aArguments=array()) {
		$this->db = self::call("mysql");
		$this->owl = self::call("owl");
		$this->owl->connect($this->db);
		$this->owl->select("objectname");
	}

	protected function save($aArguments=array()) {
		if(!count($aArguments)) { return false; }

		$this->alvin();

		# si desescapamos esta línea el tutor entra en modo debug y retorna el valor de la variable $aArguments
		# esto es util para verificar que datos le están llegando al método
		// return $this->debug($aArguments);

		$insert = $this->owl->insert($aArguments);
		return ($insert!==false) ? $insert : $this->owl->validate;
	}
}
```

Ejecución desde PHP
```php 
<?php defined("NGL_SOWED") || exit();

$tutor = $ngl("tutor")->load("web");
$response = $tutor->run("save", $_POST);

# si el tutor estuviera en modo debug, esta línea se encargará de mostrar la salida de datos
if($tutor->debugging()) { exit($response); }

?>
```


### Envío de correos
```php
<?php

namespace nogal;
$this->TutorCaller(self::requirer());

class tutorMailer extends nglTutor {

	private $mail;

	protected function init($aArguments=array()) {
		$this->mail = self::call("mail")
			->server("smtp")
			->host("tls://smtp.gmail.com")
			->secure("tls")
			->port(587)
			->user("foobar@gmail.com")
			->pass("YourP@ssw0rd")
			->charset("iso-8859-1")
			->connect()
			->login()
			->from("Foo Bar<foobar@gmail.com>")
		;
	}

	protected function send($aArguments=array()) {
		if(!count($aArguments)) { return false; }
		$this->alvin();

		if(isset($aArguments["cc"])) { $this->mail->cc($aArguments["cc"]); }
		$this->mail
			->subject($aArguments["subject"])
			->message($aArguments["message"])
			->send($aArguments["to"])
		;

		return $this->mail->log;
	}
}
```

Ejecución desde PHP
```php 
<?php defined("NGL_SOWED") || exit();

$tutor = $ngl("tutor")->load("mailer");
echo $tutor->run("send", array(
	"subject" => "Prueba",
	"message" => "Esta es una prueba de correo",
	"to" => "someguy@gmail.com",
));

?>
```

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2019 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
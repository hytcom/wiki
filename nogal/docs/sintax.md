
# Nogal
Pensado para aquellos que tienen una pequeña idea de los conceptos básicos de programación, **nogal** intenta ser el framework de PHP con la curva de aprendisaje mas rápida.  
En sus comienzos, **nogal** nace como un sistema de plantillas y simplificación para maquetadores web, sin embargo con el tiempo ha ido creciendo hasta convertirse en una herramienta muy completa para cualquier seniority de desarrollador  

Por la naturaleza de su origen, y teniendo en cuenta que la mayoría de los maquetadores conocen jQuery, **nogal** centraliza todas sus herramientas en un único objeto, que se encarga llamar e inicializar a todos sus componentes a demanda. Este objeto es la variable **\$ngl**  

Existen dos tipos de componentes dentro de **nogal**, los instanciables y los no instanciables.  
Los no instanciables son aquellos que realizan operaciones particulares y concretas, que no requieren copias de si mismos. Son librerias de herramientas.  
Por el contrario, los componentes instanciables son aquellos que gestionan entidades o grupos de datos, como las conexiones a bases de datos o archivos.  

Un claro ejemplo de esto son los componentes:
- [files](files.md) que lista y ejecuta acciones entre archivos
- [file](file.md) contiene la información de un archivo específico y ejecuta acciones sobre el mismo

&nbsp;

## Variable $ngl
Una vez finalizada la carga del framework todos sus componentes serán accesibles desde la variable-función **\$ngl**. Este es en sí el objeto principal del sistema, y el encargado de autocargar las clases e iniciar las instancias.    
Para llamar a un componente sólo se debe pasar el nombre del mismo como argumento del objeto **\$ngl**
``` php
$ngl("NOMBRE_COMPONENTE");
```
Cuando no se proporcione un nombre de componente el sistema asumirá que se trata de una llamada al objeto principal o al componente [fn](fn.md). Para ver una lista de los componentes disponibles podrá ejecutarse: 
``` php
print_r($ngl()->availables());
```

&nbsp;

## Ejecución de Métodos
Para ejecutar un método de un componente, 
``` php
$ngl("files");
$ngl("file");
```
En los objetos no instanciables se invocará a la clase y se retornará el objeto.  
En los objetos instaciables se invocará la clase, si no ha sido previamente invocada, y se retornará una instancia de ella.  
**nogal** guarda la referencia de los objetos llamados, por lo que si volvemos llamar a un objecto el sistema nos retornará la versión previamente ejecutada. Esto plantea la obvia pregunta: *cómo podemos identificar a instancias especificas?*

En el caso de los objetos no instanciables es claro que esto no hace falta, ya que justamente no se instancian, asi como tampoco guardan configuraciones de las ejecuciones previas.  
Para los objetos instanciables existe una manera de identificarlos, y es asignandoles un identificador a la hora de llamarlos. Este identificador se coloca dentro de la llamada **\$ngl()**, separándolo del tipo de objeto por medio de un punto (.)
``` php
$ngl("file.foo");
```
Quiere decir que cada vez que llamemos a `$ngl("file.foo");` estaremos llamando a la instancia **foo** del objeto **file**  
En cualquiera de los casos es posible asignar el objeto a una variable, y eso es lo que se suele hacer por costumbre. Para ello simplemente ponemos:
``` php
$file = $ngl("file");
```
Debemos tener en cuenta que **nogal** creará una nueva instancia de los objetos instanciables cada vez que estos sean llamados sin un identificador.

&nbsp;

## Objetos Instanciables
Además de tener un identificador, los objetos instanciables cuentan con argumentos y atributos  
Los argumentos permiten configurar el objeto y ejecutar sus métodos sin necesidad de pasarle parámetros en tiempo de ejecución. Los atributos axilian al sistema para no tener que recalcular ciertos valores.


ACA 


- Ejecución de un instanciable con un archivo de configuración específico
	**$ngl**("mysql.foobar|base_alter")->METODO();
	
 por ej:
	
	$foo = **$ngl**("file.foo")->load("/public_html/datos/list.txt");
	$foo->write("hola mundo"); #escribe hola mundo en el archivo una sola vez
	$foo->fill("hola mundo", 1024); #escribe hola mundo en el archivo hasta llegar a los 1024 bytes


	es equivalente a 

	$foo = **$ngl**("file.foo");
	$foo->filepath = "/public_html/datos/list.txt";
	$foo->content = "hola mundo";
	$foo->length = 1024;

	**$ngl**("file.foo")->load();
	**$ngl**("file.foo")->write();
	**$ngl**("file.foo")->fill();
	
Basicamente la ventaja de los argumentos es la reutilización de los mismos en otros métodos
Por otro lado, los atributos son seteados por la clase y son de sólo lectura, como por ejemplo:

	$foo = **$ngl**("file.foo")->load("/public_html/datos/list.txt");
	echo $foo->path;
	echo $foo->basename;
	echo $foo->date;



/** FUNCTION {
	"name" : "[3]",
	"type" : "explain",
	"description" : "Hola Mundo! como empezar a trabajar",
	"examples" : {
		"Listando un directorio" : "
			# incluir el archivo config
			require_once("config.php");
			
			# se llama al objeto files y se lista el directorio public_html
			$ls = **$ngl**("files")->ls("public_html", null, "info");
			
			# impresion de los datos
			foreach($ls as $file) {
				echo $file["basename"]."<br />";
			}
		",

		"Cargando un CSV en una tabla de la base de datos" : "
			# incluir el archivo config
			require_once("config.php");
			
			# se lee el contenido del archivo CSV, donde su primer fila contiene los nombres de las columnas
			$data = **$ngl**("file.foo")->load("datos.csv")->read();

			# paso del csv a un array
			$info = **$ngl**("shift")->convert($data, "csv-array", array("use_colnames"=>true));

			# carga en la base de datos, donde los campos de la tablas se llaman igual a las columnas de CSV
			foreach($info as $row) {
				**$ngl**("mysql.bar")->insert("mytable", $row);
			}

		"
	}
} **/



/** FUNCTION {
	"name" : "[4]",
	"type" : "explain",
	"description" : "Como pasar argumentos en objetos instanciables",
	"examples" : {
		"Como array de datos en la llamada del objeto" : "
			$conf array();
			$conf["server"] = "smtp";
			$conf["host"] = "smtp.gmail.com";
			$conf["secure"] = "ssl";
			$conf["port"] = 465;
			$conf["user"] = "js@mydomain.com";
			$conf["pass"] = "Pass1234?";
			$conf["name"] = "John Smith";
			$conf["from"] = "js@mydomain.com";
			$conf["subject"] = "Test";
			$conf["message"] = "Lorem ipsum dolor sit amet consectetur adipiscing elit";
			$conf["to"] = "you@yourdomain.com";

			**$ngl**("mail.", $conf)->connect()->login()->send();
		",

		"Como array de datos utilizando el método <strong>args</strong>" : "
			$conf array();
			$conf["server"] = "smtp";
			$conf["host"] = "smtp.gmail.com";
			$conf["secure"] = "ssl";
			$conf["port"] = 465;
			$conf["user"] = "js@mydomain.com";
			$conf["pass"] = "Pass1234?";
			$conf["name"] = "John Smith";
			$conf["from"] = "js@mydomain.com";
			$conf["subject"] = "Test";
			$conf["message"] = "Lorem ipsum dolor sit amet consectetur adipiscing elit";
			$conf["to"] = "you@yourdomain.com";

			**$ngl**("mail.")
				->args($conf)
				->connect()
				->login()
				->send();
		",

		"Como variables del objeto" : "
			$mail = **$ngl**("mail.");

			$mail->server = "smtp";
			$mail->host = "smtp.gmail.com";
			$mail->secure = "ssl";
			$mail->port = 465;
			$mail->user = "js@mydomain.com";
			$mail->pass = "Pass1234?";
			$mail->name = "John Smith";
			$mail->from = "js@mydomain.com";
			$mail->subject = "Test";
			$mail->message = "Lorem ipsum dolor sit amet consectetur adipiscing elit";
			$mail->to = "you@yourdomain.com";

			$mail->connect();
			$mail->login();
			$mail->send();
		",

		"Como setters del objeto" : "
			**$ngl**("mail.")
				->server("smtp")
				->host("smtp.gmail.com")
				->secure("ssl")
				->port(465)
				->user("js@mydomain.com")
				->pass("Pass1234?")
				->name("John Smith")
				->from("js@mydomain.com")
				->subject("Test")
				->message("Lorem ipsum dolor sit amet consectetur adipiscing elit")
				->to("you@yourdomain.com")
				->connect()
				->login()
				->send()
			;
		",

		"Mixto" : "
			# datos de login en la llamada del objeto
			$login array();
			$login["server"] = "smtp";
			$login["host"] = "smtp.gmail.com";
			$login["secure"] = "ssl";
			$login["port"] = 465;
			$login["user"] = "js@mydomain.com";
			$login["pass"] = "Pass1234?";
			$mail = **$ngl**("mail.", $login)->connect()->login();
			
			# algunos datos del mail utilizando ARGS
			$conf array();
			$conf["name"] = "John Smith";
			$conf["from"] = "js@mydomain.com";
			$conf["subject"] = "Test";
			$mail->args($conf);
			
			# mensaje usando el setter
			$mail->message("hola!!!!!!!!  probandooooo!!!");
			
			# copia seteada como variable del objeto
			$mail->cc = "backup@yourdomain.com";
			
			# to pasado directamente al metodo SEND
			$mail->send("you@yourdomain.com");

		"
	}
} **/

# owl Paso a Paso
Tutorial sobre como crear y administra una estrucutra [owl](owl.md) utilizando el objeto [nest](nest.md)
___

## Indice
- [Nest](#nest)
	- [Objetivo](#objetivo)
	- [Conexión a MySQL](#conexion-a-mysql)
	- [Iniciando Nest](#iniciando-nest)
	- [Creando Objetos Predefinidos](#creando-objetos-predefinidos)
	- [Creando Objetos](#creando-objetos)
	- [Relaciones](#relaciones)
	- [Objetos Dependientes](#objetos-dependientes)
	- [Generar Estructura](#generar-estructura)
	- [Modificando Objetos](#modificando-objetos)
- [OWL](#owl)
	- [Conectando](#conectando)
	- [Selección de Objeto](#seleccion-de-objeto)
	- [Guardando Datos](#guardando-datos)
	- [Recuperar Listado](#recuperar-listado)
	- [Recuperar Registro](#recuperar-registro)
	- [Actualización de Datos](#actualizacion-de-datos)
	- [Registros Dependientes](#registros-dependientes)

&nbsp;

## Nest
### Objetivo
A lo largo de este documento vamos a crear una estructura para un pequeño sistema de reuniones interno. Los objetos que usaremos son:  
- areas
- estadocivil
- personal
- reuniones
- comentarios

Como el objetivo es aprender a usar la herramienta y no ahondar en un análisis funcional, mantendremos el ejemplo lo más básico posible, por lo que pasaremos por alto muchos campos que sabemos que deberían tener algunos de los objetos.
  
&nbsp;

### Conexión a MySQL
El primer paso es crear el objeto de gestión de MySQL ([mysql](mysql.md))
``` php
$db = $ngl("mysql");
$db->args(array(
	"host" => "localhost",
	"user" => "dev002",
	"pass" => "asd123",
	"base" => "test"
))->connect();
```

&nbsp;

### Iniciando Nest
Iniciamos el objeto [nest](nest.md) con una estructura vacía.
``` php
$nest = $ngl("nest");
$nest->load(null, $db);
```
también podríamos usar
```php
$nest = $ngl("nest");
$nest->db($db)->load();
```

&nbsp;

### Creando Objetos Predefinidos
Creamos los objetos **areas** y **estadocivil**, que en nuestro sistema son simplemente listas desplegables
``` php 
$nest->preset("basic", "areas", "Areas");
$nest->preset("basic", "estadocivil", "Estado Civil");
```

&nbsp;

### Creando Objetos
Ahora crearemos los objetos **personal** y **reuniones**
``` php 
$nest->create("personal", 
	array(
		array("nombre", "name"),
		array("apellido", "name"),
		array("interno", "phone"),
		array("correo", "email"),
		array("fecha_nacimiento", "date"),
		array("hijos", array("type"=>"tint", "length"=>2))
	)
	"Personal"
);

$nest->create("reuniones", 
	array(
		array("fecha", "date"),
		array("inicio", "time"),
		array("fin", "time"),
		array("asunto", "varchar"),
		array("detalle", "text")
	)
	"Reuniones"
);
```

&nbsp;

### Relaciones
En este paso relacionaremos los objetos **areas** y **estadocivil** con el objeto **personal**.  
Para ellos crearemos dos nuevos campos en **personal** y los referenciaremos a los otros objetos. 
``` php 
$nest->select("personal")
	->add("area", "@areas")
	->add("estado_civil", "@estadocivil")
;
```

&nbsp;

### Objetos Dependientes
Sólo nos resta crear el objeto **comentarios**, que dependerá de **reuniones** ya que una reunión podrá ser comentada por muchas personas, para ello hacemos uso del campo **pid** que crea una relación **padre-hijo**.  
También generaremos la relación **comentario-persona**
``` php 
$nest->create("comentarios")
	array(
		array("pid", "@reuniones"),
		array("emisor", "@personal:comentarios_personas"),
		array("fecha", "datetime"),
		array("mensage", "varchar")
	)
	"Comentarios"
);
```

&nbsp;

### Generar Estructura
A la hora de generar la estructura podemos verla antes de ejecutarla, o ejecutarla directamente en la base (no se aconseja).  
Para ello nos valemos del único argumento que tiene el método: 
- **true** = ejecuta la estructura y retornará la cadena **RUNNED**
- **false** = retorna la estructura en formato cadena. FALSE es el valor por defecto
``` php 
echo $nest->generate();
```
``` php 
echo $nest->generate(true);
```

&nbsp;

### Modificando Objetos
Supongamos que la estructura ya fue ejecutada y cargada en la base de datos. 
Cuando generamos el objeto **comentarios** escribimos mal el nombre del campo **mensaje**, ahora vamos a corregir eso, vamos a darle mas capacidad de almacenamiento al campo y renombraremos el objeto como **minutas**.  
Para ello primero debemos recargar la estrucura actual (el registro *owl* de la tabla *__ngl_sentences__*) en un objeto **nest** nuevo y luego efectuar los cambios.
``` php 
$nest = $ngl("nest");
$nest
	->db($db)
	->load("owl")
	->select("comentarios")
	->rename("minutas", "Minutas")
	->alter("mensage", array("name"=>"mensaje", "type"=>"text")
	->generate()
;
```

&nbsp;

## OWL
### Conectando
La carga del objeto [owl](owl.md) es similiar a la del objeto [nest](nest.md), la diferencia es que se deberá pasar un único argumento con el conector a la base de datos con la que se trabajará. En este caso, [MySQL](mysql.md).
``` php
$db = $ngl("mysql");
$owl = $ngl("owl")->load($db);
```

&nbsp;

### Selección de Objeto
Una vez cargado el **owl** debemos seleccionar el objeto con el cual queremos trabajar, para eso usamos
``` php
$db = $ngl("mysql");
$owl = $ngl("owl")->load($db);
$owl->select("personal");
```

&nbsp;

### Guardando Datos
Una de las maneras de enviar los datos al objeto **owl** es en forma de array asociativo. No siempre hace falta enviar todos los campos del objeto, eso dependerá de la estructura del mismo. Y el array también puede contener datos que el objeto no use, estos serán ignorados.
``` php
$db = $ngl("mysql");
$owl = $ngl("owl")->load($db);
$owl->select("personal")->insert(array(
	"nombre" => "Joe",
	"apellido" => "Smith",
	"interno" => "245",
	"correo" => "jsmith@domain.com",
	"fecha_nacimiento" => "1983-07-14",
	"hijos" => 1
));
```
  
&nbsp;

### Recuperar Listado
Para recuperar el grupo completo de registros de un objeto basta con ejecutar el método [getall](owl.md#getall). Como este método retorna un objeto del tipo **result** del objeto de base de datos, es necesario ejecutar los métodos **get** ó **getall** de este último para acceder a los datos.
``` php
$db = $ngl("mysql");
$owl = $ngl("owl")->load($db);
$list = $owl->select("personal")->getall();

if($list->rows()) {
	while($row = $list->get()) {
		echo "Nombre: ".$row["nombre"]." ".$row["apellido"]."<br />";
		echo "e-mail: ".$row["correo"]."<br />";
		echo "<hr />";
	}
}
```

A la hora de utilizar [getall](owl.md#getall) es posible aplicar un filtro, este deberá estar en formato [jsql](#jsql.md). El siguiente listado muestra a todas las personas nacidas desdes la 1990 agrupadas por la cantidad de hijos que tienen.
``` php
$db = $ngl("mysql");
$owl = $ngl("owl")->load($db);
$list = $owl->select("personal")->getall('{
	"where": [
		["fecha_nacimiento","gteq","(1990-01-01)"]
	]
}');
print_r($list->getall("#hijos"));
```
  
&nbsp;

### Recuperar Registro
Para recuperar un registro en particular se utiliza el método [get](#owl.md#get), especificando su **id** o **imya** por el método [id](#owl.md#id) o directamente como primer argumento del método **get**. Este método también retorna un objeto del tipo **result** del objeto de base de datos.
``` php
$db = $ngl("mysql");
$owl = $ngl("owl")->load($db);
$data = $owl->select("personal")->get("URI75pbceMb7ca85acLD7160Ze40Kb94");
$reg = $data->get();
echo "Hola ".$reg["nombre"].", tu correo sigue siendo: ".$reg["correo"]."?";
```
  
&nbsp;

### Actualización de Datos
Para actualizar un registro en particular se utiliza el método [update](#owl.md#update), especificando su **id** o **imya** por el método [id](#owl.md#id). Este método espera como argumento uno o mas arrays.
``` php
# este ejemplo simula una petición POST

# validación de IMYA
$imya = $ngl()->imya($_POST["imya"]);

# objeto OWL
$db = $ngl("mysql");
$owl = $ngl("owl")->load($db);
$owl->select("personal")->id($imya);

// actualización
$data = array();
$data["correo"] = preg_replace("/[a-z0-9@\_\-\.]/is", "", $_POST["email"]);
$data["hijos"] = (int)$_POST["children"];
$owl->update(array("correo"=>"jsmith@mydomain.net"));
```
  
&nbsp;

### Registros Dependientes
En este ejemplo vemos como obtener todos las minutas del usuario Joe Smith
``` php
# owl
$db = $ngl("mysql");
$owl = $ngl("owl")->load($db);

# seleccion del objeto y registro principal
$owl->select("personal")->id("URI75pbceMb7ca85acLD7160Ze40Kb94");

# obtener todas las minutas del usuario seleccionado
$data = $owl->child("minutas")->getAll();
print_r($data->getall());
```

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2020 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br /> 
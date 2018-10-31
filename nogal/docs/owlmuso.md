# Owlm Paso a Paso
Tutorial sobre como crear y administra una estrucutra [owl](https://github.com/arielbottero/wiki/blob/master/nogal/docs/owl.md) utilizando el objeto [owlm](https://github.com/arielbottero/wiki/blob/master/nogal/docs/owlm.md)
___

## Indice
- [Objetivo](#objetivo)
- [Conexión a MySQL](#conexion-a-mysql)
- [Iniciando OWLM](#iniciando-owlm)
- [Creando Objetos Predefinidos](#creando-objetos-predefinidos)
- [Creando Objetos](#creando-objetos)
- [Relaciones](#relaciones)
- [Objetos Dependientes](#objetos-dependientes)
- [Generar Estructura](#generar-estructura)
- [Modificando Objetos](#modificando-objetos)
  
&nbsp;

## Objetivo
A lo largo de este documento vamos a crear una estructura para un pequeño sistema de reuniones interno. Los objetos que usaremos son:  
- areas
- estadocivil
- personal
- reuniones
- comentarios

Como el objetivo es aprender a usar la herramienta y no ahondar en un análisis funcional, mantendremos el ejemplo lo más básico posible, por lo que pasaremos por alto muchos campos que sabemos que deberían tener algunos de los objetos.
  
&nbsp;

## Conexión a MySQL
El primer paso es crear el objeto de gestión de MySQL ([mysql](https://github.com/arielbottero/wiki/blob/master/nogal/docs/mysql.md))
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

## Iniciando OWLM
Iniciamos el objeto [owlm](https://github.com/arielbottero/wiki/blob/master/nogal/docs/owlm.md) con una estructura vacía.
``` php
$owlm = $ngl("owlm");
$owlm->load(null, $db);
```
también podríamos usar
```php
$owlm = $ngl("owlm");
$owlm->db($db)->load();
```

## Creando Objetos Predefinidos
Creamos los objetos **areas** y **estadocivil**, que en nuestro sistema son simplemente listas desplegables
``` php 
$owlm->preset("basic", "areas", "Areas");
$owlm->preset("basic", "estadocivil", "Estado Civil");
```

&nbsp;

## Creando Objetos
Ahora crearemos los objetos **personal** y **reuniones**
``` php 
$owlm->create("personal", 
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

$owlm->create("reuniones", 
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

## Relaciones
En este paso relacionaremos los objetos **areas** y **estadocivil** con el objeto **personal**.  
Para ellos crearemos dos nuevos campos en **personal** y los referenciaremos a los otros objetos. 
``` php 
$owlm->select("personal")
	->add("area", "@areas")
	->add("estado_civil", "@estadocivil")
;
```

&nbsp;

## Objetos Dependientes
Sólo nos resta crear el objeto **comentarios**, que dependerá de **reuniones** ya que una reunión podrá ser comentada por muchas personas, para ello hacemos uso del campo **pid** que crea una relación **padre-hijo**.  
También generaremos la relación **comentario-persona**
``` php 
$owlm->create("comentarios")
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

## Generar Estructura
A la hora de generar la estructura podemos verla antes de ejecutarla, o ejecutarla directamente en la base (no se aconseja).  
Para ello nos valemos del único argumento que tiene el método: 
- **true** = ejecuta la estructura y retornará la cadena **RUNNED**
- **false** = retorna la estructura en formato cadena. FALSE es el valor por defecto
``` php 
echo $owlm->generate();
```
``` php 
echo $owlm->generate(true);
```

&nbsp;

## Modificando Objetos
Supongamos que la estructura ya fue ejecutada y cargada en la base de datos. 
Cuando generamos el objeto **comentarios** escribimos mal el nombre del campo **mensaje**, ahora vamos a corregir eso, vamos a darle mas capacidad de almacenamiento al campo y renombraremos el objeto como **minutas**.  
Para ello primero debemos recargar la estrucura actual (el registro *owl* de la tabla *__ngl_sentences__*) en un objeto **owlm** nuevo y luego efectuar los cambios.
``` php 
$owlm = $ngl("owlm");
$owlm
	->db($db)
	->load("owl")
	->select("comentarios")
	->rename("minutas", "Minutas")
	->alter("mensage", array("name"=>"mensaje", "type"=>"text")
	->generate()
;
```

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
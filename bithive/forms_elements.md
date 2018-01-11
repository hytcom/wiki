## Consideraciones Generales
### Listado de Elementos soportados
- [attacher](#attacher)
- [autocomplete](#autocomplete)
- [break](#break)
- [checkbox](#checkbox)
- [color](#color)
- [cols](#cols)
- [date](#date)
- [date-range](#date)
- [divider](#divider)
- [file](#file)
- [hidden](#hidden)
- [html](#html)
- [input](#input)
- [modalform](#modalform)
- [number](#number)
- owl-checkbox ( obsoleto, utilizar [checkbox](#checkbox) )
- owl-radio ( obsoleto, utilizar [radio](#radio) )
- owl-select ( obsoleto, utilizar [select](#select) )
- [password](#password)
- [radio](#radio)
- [readonly](#readonly)
- [section](#section)
- [select](#select)
- [slider](#slider)
- [subform](#subform)
- [switch](#switch)
- [tags](#tags)
- [textarea](#textarea)
&nbsp;

### Atributos comunes a todos los Elementos
- **class** = clase css del campo
- **groupclass** = clase css que se añadirá al *DIV* form-group
- **labelclass** = clase css que se añadirá al *LABEL* form-label
- **label** = etiqueta (exepto en el elemento hidden)
- **name** = nombre del campo
- **value** = valor previo del campo (opcionalmente). En el caso de valores multiples (selects, checkboxs y radios), utilizar una cadena de valores separados por ;
- **notes** = text/html que se colocará despues del campo (exepto hidden)
- **attribs** = json con atributos extras:

```json
	"attribs": {
		"atributo1": "valor1",
		"atributo2": "valor2"
	}
```
&nbsp;

### Formato de datos para los campos de selección *(combos)*
- autocomplete
- checkbox
- radio
- select
&nbsp;

#### Simple {label,value}
```json
[
	{"label":"..", "value":".."},
	{"label":"..", "value":".."},
	{"label":"..", "value":".."}
]
```

#### Grupos {group,label,value} (sólo disponible para selects)
```json
[
	{"group":...,"label":"..", "value":".."},
	{"group":...,"label":"..", "value":".."},
	{"group":...,"label":"..", "value":".."}
]
```
&nbsp;

### Datos dinámicos (URLs)
Cuando se quieran cargar datos dinámicos desde una URL se podra utilizar la sintaxis {:JQUERY_SELECTOR:} para hacer referencia al valor de otro elemento, por ejemplo:

Aquí se condiciona un autocomplete en funcion del valor seleccionado en otro

```json
["autocomplete", {
	"label": "Localidad",
	"name": "town",
	"source": "../../__bigknot?imya=qm4SAG5VSiuUxhNzvSJpLej14BeZdSOA",
	"notes": "Comience a escribir el nombre del Localidad"
}],
["autocomplete", {
	"label": "Barrio",
	"name": "neighborhood",
	"source": "__knot?imya=uEdVChLkoZkKbHsYQtiiulHQNwnihLxd&town={:[name='town']:}",
	"notes": "Comience a escribir el nombre del Barrio"
}]
```
&nbsp;

### Combos relacionados
Es posible relacionar 2 ó mas combos, para ello se deberan utilizar los siguientes atributos en el elemento de **origen**
#### Atributos
- **lnk-source** = URL que se ejecutará al seleccionar un valor en el *select* o *radio* de **origen** y a la que se le añadirá el valor seleccionado *(en el ejemplo, variable q)*. Los elementos *checkbox* no pueden ser utilizados como elementos de **origen**
- **lnk-target** = jquery selector del combo de **destino**, *select*, *radio** o *checkbox*

``` json	
["select", {
	"name":"source", 
	"label":"Empresa", 
	"source":"__knot?imya=agHra496ft4LhAQZcg7rlefxW54prX6d",
	"lnk-source":"__knot?imya=Uh06krPfToxH08qeX5z4uokRAsEzrhQU&q=",
	"lnk-target":"#cc"
}],
["checkbox", {
	"attribs":{"id":"contact"},
	"name":"contact", 
	"label":"Contactos", 
}]
```
&nbsp;

### Validación "On The Fly"
Para validar un dato contra el servidor, en tiempo real, se pueden utilizar los sub-atributos: *data-checker* y *data-checker-text* dentro del atributo **attribs**
- **data-checker** = URL del script verificador, al cual se le pasará la variable **q**  con el valor actual del campo. La respuesta deberá ser retornada en formato JSON y deberá tener la siguiente estructura: &nbsp; `{"value": "1", "message": "@motto"}`
- **data-checker-text** = mensaje que se mostrará cuando *data-checker* retorne **1** en el campo **value**. Se puede utilizar *** para imprimir el valor retornado en el campo **message**

``` json	
"attribs": {
	"data-checker": "__knot?imya=fUyqGMP1vTnvECn2zRHCtjj5RIHVtgdz",
	"data-checker-text": "El nombre de usuario *** ya está siendo utilizado"
}
```
&nbsp;
&nbsp;

---
&nbsp;

# Elementos
En la sección **Atributos** de cada elemento enumeran los atributos particulares y opcionales del elemento que se suman a los atributos comunes.
&nbsp;

## attacher
Zona para drag a drop para adjuntar archivos
### Variantes
- **attacher** es la clásica zona
- **attacher-name** adjunta los archivos sin generar preview
- **attacher-image** acepta un único archivo del tipo imagen, se usa para fotos de perfil

### Atributos
- **text** = texto de la zona de drop
#### Opcionales
- **types** = tipos de archivos separados por coma (,)
- **max** = maximo de archivos permitidos
- **autohide** = define si se debe ocultar la zona drop despues de adjuntar: true o false
- **value** =  opcional, url con la ruta del archivo actual

```json
["attacher", {
	"name": "images", 
	"label": "Fotos", 
	"text": "haga click o arrastre las imágenes aquí para adjuntar",
	"max": "5",
	"types": "jpg,jpeg,png"
}]
```
&nbsp;

## autocomplete
Lista autocompletable de opciones
### Atributos
- **source** = url o json con datos, formato {label,value}
#### Opcionales
- **addtext** = texto que se mostrara cuando no haya resultados y que dará la opcion de **agregar nuevos valores** abriendo el formulario *addsource*
- **addsource** = url del formulario para agregar datos al autocomplete

```json
["autocomplete", {
	"name": "town",
	"label": "Barrio",
	"source": "__knot?imya=uEdVChLkoZkKbHsYQtiiulHQNwnihLxd",
	"notes": "Comience a tipear el nombre del barrio"
}]
```
*con la opción de agregar valores a la lista*

```json
["autocomplete", {
	"name": "town",
	"label": "Barrio",
	"source": "__knot?imya=uEdVChLkoZkKbHsYQtiiulHQNwnihLxd",
	"addtext": "Agregar Nuevo Barrio",
	"addsource": "add.html"
}]
```
&nbsp;

## break
Añade un espacio vertical. Este elemento unicamente soporta el atributo *class* y de manera opcional

```json
["break"]
```
&nbsp;

## checkbox
Grupo de opciones checkbox
### Atributos
- **source** = opciones en formato {label,value} donde *label* será el texto a mostrar y *value* el valor del checkbox
#### Opcionales
- **class** = se pueden especificar clases de columnas Bootstrap para una mejor representación *col-lg-4 col-md-6 col-xs-12*
- **empty** = texto que se mostrará cuando no se encuentren opciones en *source*, utilizado con *checkbox* dinámicos

```json
["checkbox", {
	"name":"name",
	"label":"Barrio",
	"source":"__knot?imya=uEdVChLkoZkKbHsYQtiiulHQNwnihLxd",
	"class": "col-lg-4 col-md-6 col-xs-12"
}]
```
&nbsp;

## color
Selector de color hexadecimal

```json
["color", {"name":"color", "label":"Color"}]
```
&nbsp;

## cols
Agrupa los campos de 2 a 5 columnas en resoluciones mayores a **992px**
En resoluciones menores, los bloques de 4 y 5 columnas se transformarán en 2 columnas, y los bloques de 2 y 3 en 1 columna.
El bloque de columnas se inicia con la llamada de un elemento *cols* y se cierra con la llamada de otro elemento *cols* que tenga presente el atributo *close*
### Atributos
- **cols** = cantidad de columnas 2 a 5
#### opcionales
- **close** = indica el cierre de columnas, se puede utilizar 1 o true

```json
["cols", {"cols":"3"}],
["input", {"name":"name", "label":"Label"}],
["input", {"name":"name", "label":"Label"}],
["input", {"name":"name", "label":"Label"}],
["cols", {"cols":"2", "close":"1"}],
["input", {"name":"name", "label":"Label"}],
["input", {"name":"name", "label":"Label"}],
["cols", {"close":"1"}]
```
&nbsp;

## date
Selector de fecha y/u hora
### Variantes
- **date** selector simple
- **date-range** selector de rangos
### Atributos
- **name2** = nombre del campo de fin, sólo en *date-range*
#### opcionales
- **value2** = valor del campo de fin, sólo en *date-range*
- **type** = tipo de selector, por default *datepicker*
	- datepicker
	- monthpicker
	- yearpicker
	- hourpicker
	- timepicker
	- datetimepicker
	- datetime (máscaras)
	- date (máscaras)
	- hour (máscaras)
	- time (máscaras)
- **attribs**
	- **data-min** = menor valor posible, sólo para los tipo *picker*
	- **data-max** = máximo valor posible, sólo para los tipo *picker*

```json
["date", {"type":"datepicker", "name":"name", "label":"Fecha"}]
```

```json
["date-range", {
	"type": "monthpicker",
	"name": "name",
	"name2": "name2",
	"label": "Fechas",
	"attribs": {
		"data-min": "2018-01-01",
		"data-max": "2018-12-01"
	}
}]
```
&nbsp;

## divider
Añade un divisor horizontal `<hr>`. Este elemento sólo acepta el atributo **class** de manera opcional

```json
["divider"]
```
&nbsp;

## file
Clásico input file

```json
["file", {"name":"document", "label":"Documento"}]
```
&nbsp;

## hidden
Clásico input hidden. Por su característica de oculto, este elemento no soporta el atributo **label**

```json
["hidden", {"name":"id"}]
```
&nbsp;

## html
En si mismo, este elemento no añade contenido alguno. Es una herramienta para añadir todo tipo de código HTML al formulario.
### Atributos
- **code** = código HTML, único atributo soportado por el elemento. En el valor del atributo deberán considerarse los caracteres especiales para no romper la estructura JSON.

```json
["html", {"code":"
    <div id='demo'>
        <table class='table table-bordered'>
            <thead>
                <tr>
			        <th>#</th>
			        <th>Nombre</th>
			        <th>Apellido</th>
			        <th>Usuario</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <th>1</th>
                    <td>Mark</td>
                    <td>Otto</td>
                    <td>@motto</td>
                </tr>
            </tbody>
    	</table>
    </div>
"}]
```
&nbsp;

## input
Clásico input text

```json
["input", {"name":"name", "label":"Nombre"}]
```
&nbsp;

## modalform
Este elemento genera un botón que al presionarlo abre un formulario emergente. Utilizado generalmente en formularios de edición, ya que es ideal para vincular registros satélites a un registro maestro, como por ejemplo los contactos de una empresa a la misma.
### Atributos
- **button** = texto que se mostrará en el botón
- **source** = URL del formulario. Puede contener variables, esto permite, por ejemplo, pasar el **id** de un registro maestro
#### opcionales
- **value** = URL de un documento que contiene los valores previamentes cargados (ej: tabla con contactos). Este documento se cargará en *target* al inicio (excepto con *skipfirst =* **true**) y después de cada envio satisfactorio
- **target** = selector jquery de la zona en donde se cargará *value*
- **skipfirst** = evita el primer llamado al domumento *value*, por defecto es **false**
- **size** = tamaño del dialogo (hg xl lg md sm xs)

```json
["modalform", {
	"button": "nuevo contacto",
	"class": "btn btn-primary",
	"source": "modalform_form?id=1234", 
	"value": "modalform_table?id=1234",
	"target": "#demo",
	"skipfirst": "true",
	"size": "md"
}],
```
&nbsp;

## number
Selector de valores numéricos.
### Atributos
#### opcionales
- **min** = menor valor posible
- **max** = maximo valor posible
- **step** = incremento

``` json
["number", {"name":"number", "label":"number", "min":"0", "max":"100", "step":"10"}]
```
&nbsp;

## password
Campo para contraseñas que cuenta con la opción de mostrar el valor introducido

```json
["password", {"name":"password", "label":"password"}]
```
&nbsp;

## radio
Grupo de opciones radio
### Atributos
- **source** = opciones en formato {label,value} donde *label* será el texto a mostrar y *value* el valor del radio
#### Opcionales
- **class** = se pueden especificar clases de columnas Bootstrap para una mejor representación *col-lg-4 col-md-6 col-xs-12*
- **empty** = texto que se mostrará cuando no se encuentren opciones en *source*, utilizado con *radio* dinámicos
- **lnk-source** = URL del origen de datos de un **combo relacionado**, que se ejecutará al seleccionar un valor
- **lnk-target** = jquery selector del combo relacionado, *select*, *radio** o *checkbox*

```json
["radio", {
	"name":"name",
	"label":"Barrio",
	"source":"__knot?imya=uEdVChLkoZkKbHsYQtiiulHQNwnihLxd",
	"class": "col-lg-4 col-md-6 col-xs-12"
}]
```
&nbsp;





section:
	divider: opcional, inserta un HR antes del Hn
	size: tamaño del titulo Hn (1 a 6)
	title: texto del titulo
	
readonly:
	show: valor a mostrar, puede o no ser igual a value

	
select:
	source: url o json en formato {label,value} donde value sera el valor de la opcion y label el texto a mostrar
		utilizar {group,label,value} para agrupar opciones dentro de un select

	attribs
		multiple: true o false
		size: 

slider:
	attribs
		data-min: menor valor posible
		data-max: maximo valor posible
		data-step: incremento


switch:
	source: json en formato {label,value} donde label sera el texto a mostrar y value el valor del checkbox


tag:
	source: opcionalmente una url de un json que retorne registros con el formato:
		[{"label:"..", "value":".."}, {"label:"..", "value":".."}, {"label:"..", "value":".."}]
		si no se declara, el campo será libre
		
		
textarea
	type
		dynamic
		wysiwyg-lite
		wysiwyg
		wysiwyg-full



subform:
	button: texto del boton de agregar
	source: url del subformulario
	default: cantidad de subformularios cargados por default
	
	controles
		los subforms cuentan con algunos controles, que se asignan a los objetos html segun los atributos subform-
			subform-number: pondra el numero de orden del subform dentro del grupo
			subform-up: mueve el subform hacia arriba, colocandolo por encima del subform inmediato anterior
			subform-down: mueve el subform hacia abajo, colocandolo por debajo del subform inmediato posterior
			subform-update:
				cuando existe bloquea la edición de los campos y la eliminacion del subform
				al presionarlo activa la edición y crea un campo oculto que tiene por nombre el valor del atributo 'subform-update' + '_update' y le asigna el valor el almacenado en 'value' para ese indice. Esto actua como FLAG de modificación
			subform-remove:
				remueve el subform y de existir un valor en 'value' para el valor del atributo 'subform-remove' crea un campo oculto llamado como dicho valor + '_remove' y 
				le asigna el valor el almacenado en 'value' para ese indice. Esto actua como FLAG de eliminación
		ej:
			<div class="bg-lighter-grey brd-solid brd-xs brd-light-grey padding-sm margin-md margin-only-bottom">
				<div class="row">
					<div class="col-xs-6 h4"><span class="label label-info">Paso <span class="text-white" subform-number="true"></span></span></div>
					<div class="col-xs-6 text-right">
						<i class="fa fa-chevron-up cursor-pointer" subform-up="true"></i>
						<i class="fa fa-chevron-down cursor-pointer margin-xs" subform-down="true"></i>
						<i class="fa fa-pencil cursor-pointer margin-xs" subform-update="id"></i>
						<i class="fa fa-times cursor-pointer margin-xs" subform-remove="id"></i>
					</div>
				</div>
				<br />
				<rind:set><@name>step</@name><@value><rind:unique/></@value></rind:set>
				<rind:mergefile>
					<@source>forms</@source>
					<@multiple json>
						[
							["input", {"name":"stepname[{*step}]", "label":"Nombre"}],
							["input", {"name":"stepdescription[{*step}]", "label":"Descripción"}],
							["number", {"name":"stepattempts[{*step}]", "label":"Intentos", "value":"1"}],
							["subform", {"button":"agregar opción", "source":"case_add.php?id={*step}", "default":"1"}]
						]
					</@multiple>
				</rind:mergefile>
			</div>



dialog
<div class="form-group">
	<label class="control-label"><button class="btn btn-primary btn-sm margin-sm margin-only-bottom dialog-link" dialog-href="motos_search" dialog-id="motos" dialog-title="Seleccionar Moto">agregar moto</button></label>
	<input id="dominio" value="" class="form-control form-input" style="display:none" />
	<input type="hidden" id="dominioh" name="moto" value="" />
</div>



// -------------------------------------------------------------------------------------------------
Validacion de datos

para validad un dato contra el servidor, on the fly, utilizar los atributos especiales: data-checker y data-checker-text
	"attribs": {
		"data-checker": "url del script chequeador. En la variable q el sistema agregará el valor actual del campo",
		"data-checker-text": "mensaje a mostrar en caso de que el chequeo de positivo. Utilizar *** para imprimir el valor retornado por el archivo chequeador"
	}
	
	
	ej:
	
	"attribs": {
		"data-checker": "__knot?imya=fUyqGMP1vTnvECn2zRHCtjj5RIHVtgdz",
		"data-checker-text": "El correo *** ya se encuentra registrado"
	}
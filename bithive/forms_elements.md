## Consideraciones Generales
### Elementos Básicos
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
- [switch](#switch)
- [tags](#tags)
- [textarea](#textarea)

### Elementos Avanzados
- [relation](#relation)
- [subform](#subform)
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
- switch
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
	{"group":"...","label":"..", "value":".."},
	{"group":"...","label":"..", "value":".."},
	{"group":"...","label":"..", "value":".."}
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
	"lnk-target":"#contact"
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

# Elementos Básicos
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
#### opcionales
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
#### opcionales
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
#### opcionales
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
Un bloque de columnas se inicia con la llamada de un elemento **cols** con el atributo *open* y se cierra con la llamada de otro elemento **cols** que tenga presente el atributo *close*
Cada llamada al elemento que contenga unicamente el atributo *cols*, cerrará el bloque anterior de columnas e iniciará uno nuevo
### Atributos
- **cols** = cantidad de columnas 2 a 5
#### opcionales
- **open** = indica el inicio del encolumnado, se puede utilizar 1 o true
- **close** = indica el cierre de columnas, se puede utilizar 1 o true

```json
El tabulado del ejemplo es unicamente con fines estéticos

["cols", {"cols":"3", "open":"1"}],
	["input", {"name":"name", "label":"Label"}],
	["input", {"name":"name", "label":"Label"}],
	["input", {"name":"name", "label":"Label"}],
["cols", {"cols":"2"}],
	["input", {"name":"name", "label":"Label"}],
	["input", {"name":"name", "label":"Label"}],
["cols", {"cols":"3"}],
	["input", {"name":"name", "label":"Label"}],
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
	<table class='table table-bordered'>
		<tbody>
			<tr><td>7</td><td>8</td><td>9</td></tr>
			<tr><td>4</td><td>5</td><td>6</td></tr>
			<tr><td>1</td><td>2</td><td>3</td></tr>
			<tr><td colspan='2'>0</td><td>.</td></tr>
		</tbody>
	</table>
"}]
```
&nbsp;

## input
Clásico input text

```json
["input", {"name":"name", "label":"Nombre"}]
```
&nbsp;

## number
Selector de valores numéricos.
### Atributos
#### opcionales
- **min** = menor valor posible
- **max** = maximo valor posible
- **step** = incremento
- **attribs**
	- **data-prefix** = texto anterior al campo
	- **data-postfix** = texto posterior al campo

``` json
["number", {
	"name":"number", 
	"label":"number", 
	"min":"0", 
	"max":"100", 
	"step":"10",
	"attribs": {"data-postfix":"%"}
}]
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
#### opcionales
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

## readonly
Emula un campo `<input>` utilizando un `<span>`, la utilidad de es este elemento es poder mostrar un valor en el `<span>` mientras que el campo *hidden* alberga otro
### Atributos
- **show** = valor que se mostrará en el campo falso `<span>`
#### opcionales

```json
["readonly", {
	"name":"name",
	"label":"Responsable",
	"value":"1",
	"show": "Mark Otto (@motto)"
}]
```
&nbsp;

## section
Inserta un título de sección del tipo `<h.>` con la posibilidad de anteponer un `<hr>`
### Atributos
#### opcionales
- **divider** = si tiene seteado cualquier valor, se insertará un `<hr>` antes del título de sección
- **title** = texto del título de la sección
- **size** = tamaño de texto del título, valores de 1 a 6. Por defecto 3
- **dclass** = clase CSS del divider

```json
["section", {
	"divider":"1",
	"title":"Sección"
}]
```
&nbsp;

## select
Lista desplegable de opciones
### Atributos
- **source** = opciones en formato `{label,value}` donde *label* será el texto a mostrar y *value* el valor del option. También se puede utilizar `{group,label,value}` para agrupar opciones
#### opcionales
- **empty** = texto que se mostrará cuando no se encuentren opciones en *source*, utilizado con *selects* dinámicos
- **lnk-source** = URL del origen de datos de un **combo relacionado**, que se ejecutará al seleccionar un valor
- **lnk-target** = jquery selector del combo relacionado, *select*, *radio** o *checkbox*
- **attribs**
	- **multiple** = determina si es un `<select>` de selección multiple, true o false
	- **size** = cantidad de valores visibles en el select tamaño

```json
["select", {
	"name":"name",
	"label":"Barrio",
	"source":"__knot?imya=uEdVChLkoZkKbHsYQtiiulHQNwnihLxd"
}]
```
&nbsp;

## slider
Selector desplazable de valores
### Atributos
#### opcionales
- **min** = menor valor posible
- **max** = maximo valor posible
- **step** = incremento
- **attribs**
	- **data-prefix** = texto anterior al valor
	- **data-postfix** = texto posterior al valor

```json
["select", {
	"name":"name",
	"label":"Barrio",
	"source":"__knot?imya=uEdVChLkoZkKbHsYQtiiulHQNwnihLxd"
}]
```
&nbsp;

## switch
Grupo de opciones **checkbox** en formato de botones switch
### Atributos
- **source** = opciones en formato {label,value} donde *label* será el texto a mostrar y *value* el valor del switch
#### opcionales
- **class** = se pueden especificar clases de columnas Bootstrap para una mejor representación *col-lg-4 col-md-6 col-xs-12*
- **empty** = texto que se mostrará cuando no se encuentren opciones en *source*, utilizado con *switch* dinámicos

```json
["switch", {
	"name":"name",
	"label":"Barrio",
	"source":"__knot?imya=uEdVChLkoZkKbHsYQtiiulHQNwnihLxd",
	"class": "col-lg-4 col-md-6 col-xs-12"
}]
```
&nbsp;

## tabs
Genera un grupo de tabs.
El grupo se inicia con la llamada de un elemento **tabs** con el atributo *open* y se cierra con la llamada de otro elemento **tabs** que tenga presente el atributo *close*
Cada llamada al elemento que contenga unicamente el atributo *title*, cerrará el tab anterior y abrirá uno nuevo
### Atributos
- **title** = título del TAB
- **class** = clase aplicada al contenedor de todo el bloque. Solo tiene efecto cuando se encuentra presente el atributo *open*
#### opcionales
- **open** = indica el inicio del grupo de tabs, se espera *1* o *true*
- **close** = indica el cierre del grupo de tabs, se espera *1* o *true*

```json
```json
El tabulado y las columnas del ejemplo son unicamente con fines ilustrtivos
[            
    ["tabs", {"title":"Datos Personales", "open":"1"}],
        ["cols", {"open":"1", "cols":"3"}],
            ["input", {"name":"name", "label":"input"}],
            ["input", {"name":"name", "label":"input"}],
            ["input", {"name":"name", "label":"input"}],
        ["cols", {"cols":"4"}],
            ["input", {"name":"name", "label":"input"}],
            ["input", {"name":"name", "label":"input"}],
            ["input", {"name":"name", "label":"input"}],
            ["input", {"name":"name", "label":"input"}],
        ["cols", {"cols":"2"}],
            ["input", {"name":"name", "label":"input"}],
            ["input", {"name":"name", "label":"input"}],
        ["cols", {"close":"1"}],
    ["tabs", {"title":"Datos Laborales"}],
        ["input", {"name":"name", "label":"Label"}],
        ["input", {"name":"name", "label":"Label"}],
    ["tabs", {"title":"Datos Bancarios"}],
        ["input", {"name":"name", "label":"Label"}],
        ["input", {"name":"name", "label":"Label"}],
        ["input", {"name":"name", "label":"Label"}],
    ["tabs", {"close":"1"}]
]
```
&nbsp;

## tags
Selector de etiquetas. Estas pueden ser libres y/o provenientes de un autocomplete si se espeficita *source*
### Atributos
- **source** = por convencion las opciones se esperan en formato {label,value} pero el *label* será ignorado, el *value* será el texto a consignar en la etiqueta

```json
["tags", {
	"name":"name",
	"label":"tags",
	"source":"data.json?"
}],
```
&nbsp;


## textarea
Clasico campo `<textarea>` o versiones enriquecidas
### Atributos
- **type** = tipo de `<textarea>`, las opciones son:
	- **dynamic** = dinámico, el campo se agranda automaticamente para su edición pero luego vuelve a ocupar pocas líneas
	- **fullscreen** = añade la posibilidad de llevar el `<textarea>` al 90% del alto y ancho de la ventana
	- **wysiwyg-lite** = editor WYSIWYG con funcionalidades mínimas: `<b><i><u>`
	- **wysiwyg** = editor WYSIWYG estandar: `<b><i><u>` + alineaciones
	- **wysiwyg-full** = editor WYSIWYG completo

```json
["textarea", {
	"type":"wysiwyg",
	"name":"name",
	"label":"textarea",

}],
```
&nbsp;
&nbsp;

---
&nbsp;

# Elementos Avanzados
## relation
El elemento se muestra como un botón que abre un diálogo en el que se pueden cargar diferentes tipos de documentos.
El propósito del elemento es generar vinculos entre el formulario actual y datos adyacentes. Conceptualmente es identico a un elemento combo (select, radios, etc), pero ofrece un mayor control y versatilidad.  
Hay diferentes variantes para este elmento, algunos de sus usos:
- dar de alta un registro *hijo* y vincularlo al *padre*. Ej: datos de la persona responsable de una empresa (ver ejemplo #2)
- seleccionar uno o varios valores de un grupo de resultados. Ej: vincular rubro/s a un producto
- buscar un valor, si no existe darlo de alta, si existe seleccionarlo del grupo de resultados. Ej: seleccionar el cliente en una factura

### Atributos
- **id** = id del objeto del *diaolog* en donde se cargarán los contenidos dinámcicos. **IMPORTANTE** Luego de cargar nuevos contenidos deberá ejecutarse el método `$("#VALOR_ID").trigger("relation");` para que sean parseados. (ver ejemplo #3)
- **button** = texto que se mostrará en el botón
- **source** = URL del formulario. Puede contener variables, esto permite, por ejemplo, pasar el **id** de un registro maestro
#### opcionales
- **value** = URL de un documento que contiene los valores previamentes cargados (ej: tabla con contactos). Este documento se cargará en *target* al inicio (excepto con *skipfirst =* **true**) y después de cada envio satisfactorio
- **target** = selector jquery de la zona en donde se cargará *value*
- **skipfirst** = evita el primer llamado al domumento *value*. Valore aceptados "true" y "false"
- **closebutton** = determina si debe mostrarse el botón "cerrar" en el diálogo, se espera *1* o *true*, por defecto: *false*
- **noform** = determina si el contenido de el diálogo debe presentarse como un formulario, se espera *1* o *true* por defecto: *false*
- **size** = tamaño del dialogo (hg xl lg md sm xs)
#### en los documentos *source*
- **data-relation** = por medio de este atributo se puede asignar funcionalidades a los elementos de los documentos cargados en el diálogo
	- **close** = al hacer click sobre el elemento se cerrará el diálogo
	- **multiple** = añade a *target* el contenido del comentario del elemento (ver ejemplo #2)
	- **once** = setea de manera única el contenido del comentario en el *target* (aplicable al ejemplo #2)
	- **onceclose** = identico al anterior, pero luego de setearlo cierra el diálogo (ver ejemplo #3)

##### [ Ejemplo 1 ]
```json
["html", {"code": "<div id='contzone'></div>"}],
["relation", {
	"button": "nuevo contacto",
	"class": "btn btn-primary",
	"value": "contacts.php?parent=1234",
	"source": "new_contact.php?parent=1234", 
	"target": "#contzone",
	"skipfirst": "true",
	"size": "md"
}]
```
``` html
<!-- Tabla con contactos previos (contacts.php) -->
<table class="table table-bordered">
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
```
``` html
<!-- formulario para agregar nuevos contactos (new_contact.php) -->
<form action="{$ENV.tutor}/contact/insert" method="post">
    <input type="hidden" name="NGL_ONCECODE" value="<rind:once />" />
    <input type="hidden" name="parent" value="{$_GET.parent}" />
    <div class="row margin-md margin-only-bottom">
        <div class="col-xs-12">
            <rind:mergefile>
                <@source>_templates_/forms</@source>
                <@multiple json>
                    [
                        ["input", {"name":"firstname", "label":"Nombre"}],
                        ["input", {"name":"lastname", "label":"Apellido"}],
                        ["input", {"name":"username", "label":"Usuario"}]
                    ]
                </@multiple>
            </rind:mergefile>
        </div>
    </div>
</form>
```

##### [ Ejemplo 2 ]
```json
["html", {"code":"<div id='categories' class='padding-sm brd-solid brd-gray brd-xs brd-round-xs'></div>"}],
["relation", {
    "button": "seleccionar categorías",
    "closebutton": "true",
    "class": "btn btn-primary",
    "source": "categories.php", 
    "target": "#categories",
    "noform": "true",
    "size": "md"
}],
```
``` html
<!-- Tabla de selección de Categorías (categoires.php) -->
<table class='table table-bordered'>
	<tr>
		<td>Deportes</td>
		<td class="text-center"><a class="btn btn-xs btn-primary" data-relation="multiple">seleccionar<!-- <span><input type="hidden" name="categories[]" value="1" />Deportes<br /></span> --></a></td>
	</tr>
	<tr>
		<td>Economía</td>
		<td class="text-center"><a class="btn btn-xs btn-primary" data-relation="multiple">seleccionar<!-- <span><input type="hidden" name="categories[]" value="2" />Economía<br /></span> --></a></td>
	</tr>
	<tr>
		<td>Policiales</td>
		<td class="text-center"><a class="btn btn-xs btn-primary" data-relation="multiple">seleccionar<!-- <span><input type="hidden" name="categories[]" value="3" />Policiales<br /></span> --></a></td>
	</tr>
	<tr>
		<td>Sociales</td>
		<td class="text-center"><a class="btn btn-xs btn-primary" data-relation="multiple">seleccionar<!-- <span><input type="hidden" name="categories[]" value="4" />Sociales<br /></span> --></a></td>
	</tr>
</table>
```

#### [ ejemplo 3 ]
```json
["html", {"code":"<div id='marks' class='padding-sm brd-solid brd-gray brd-xs brd-round-xs'></div>"}],
["relation", {
	"id": "searchrel",
    "button": "seleccionar marca",
    "class": "btn btn-primary",
    "source": "search.php", 
    "target": "#marks",
    "noform": "true",
    "size": "md"
}]
```
```html
<!-- Buscador (search.php) -->
<div>
    <div class="row margin-md margin-only-bottom">
        <div class="col-xs-12">
            <div class="form-group">
                <label class="control-label">Marcas</label>
                <input id="q" name="name" value="" class="form-control form-input ">
            </div>
        </div>
    </div>
    <div class="text-center">
        <input value="Buscar" class="btn btn-primary" type="button" id="searchbtn" />
        <input value="Cerrar" class="btn btn-primary margin-xs margin-only-left" type="button" data-relation="close" />
    </div>
</div>
<div id="search_result" class="margin-md margin-only-top"></div>
<script type="text/javascript">
    $(function(){
        $("#searchbtn").click(function(e) {
            e.preventDefault();
            $("#search_result").load("result?q="+$("#q").val(), function() {
                $("#searchrel").trigger("relation");
            });
        });
    });
</script>
```
```html
<!-- Resultados de búsqueda (result.php) -->
<table class='table table-bordered'>
    <tr>
        <td>Adidas</td>
        <td class="text-center"><a class="btn btn-xs btn-primary" data-relation="onceclose">seleccionar<!-- <span><input type="hidden" name="mark" value="1" />Adidas<br /></span> --></a></td>
    </tr>
    <tr>
        <td>Nike</td>
        <td class="text-center"><a class="btn btn-xs btn-primary" data-relation="onceclose">seleccionar<!-- <span><input type="hidden" name="mark" value="2" />Nike<br /></span> --></a></td>
    </tr>
    <tr>
        <td>Reebok</td>
        <td class="text-center"><a class="btn btn-xs btn-primary" data-relation="onceclose">seleccionar<!-- <span><input type="hidden" name="mark" value="3" />Reebok<br /></span> --></a></td>
    </tr>
</table>
```






&nbsp;

#subform:

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
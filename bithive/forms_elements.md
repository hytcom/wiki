# Bithive
___
  

## Formularios
### Seguridad
Validación de datos por medio del objeto [avlin](#alvin)

### Elementos de Datos
- [attacher](#attacher)
- [autocomplete](#autocomplete)
- [checkbox](#checkbox)
- [buttonsset](#buttonsset) FALTA
- [color](#color)
- [coords](#coords)
- [date](#date)
- [date-range](#date)
- [days](#days)
- [fake](#fake) FALTA
- [find](#find) FALTA
- [file](#file)
- [hidden](#hidden)
- [input](#input)
- [months](#months)
- [number](#number)
- [password](#password)
- [radio](#radio)
- [readonly](#readonly)
- [select](#select)
- [slider](#slider)
- [social](#social)
- [steps](#steps) FALTA
- [switch](#switch)
- [tags](#tags)
- [textarea](#textarea)
- [year](#year) FALTA

### Elementos Avanzados
- [relation](#relation)
- [subform](#subform)
- [subform-number](#subform) FALTA
- [subform-select](#subform) FALTA suffix /extend/

### Elementos GUI
- [break](#break)
- [cols](#cols)
- [colsgroup](#colsgroup)
- [divider](#divider)
- [html](#html)
- [section](#section)
- [steps](#steps)
- [tabs](#tabs)
&nbsp;

## Conceptos Generales
### Atributos comunes a todos los elementos de datos
- **class** = clase css del campo
- **groupclass** = clase css que se añadirá al *DIV* form-group
- **labelclass** = clase css que se añadirá al *LABEL* form-label
- **label** = etiqueta (excepto en el elemento hidden)
- **name** = nombre del campo
- **value** = valor previo del campo (opcionalmente). En el caso de valores multiples (selects, checkboxs y radios), utilizar una cadena de valores separados por ;
- **notes** = text/html que se colocará despues del campo (excepto hidden). Si queremos evitar el salto en blanco dejado por este texto en caso de no usarlo, utilizar la clase **nonote** en el atributo **groupclass**
- **attribs** = json con atributos extras
- **alvin** = json con las validaciones de seguridad ALVIN
- **disabled** = deshabilita el campo
- **readonly** = establece el campo como solo lectura
- **stamp** = mostrará el campo cuando el valor sea *vacio* o sea una expresión lógica que al evaluarla devuelva **true**. En caso contrario excluirá el elemento del formulario. **NO LO OCULTA, LO EXCLUYE DEL DOCUMENTO HTML**

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
- **data-checker** = URL del script verificador, al cual se le pasará la variable **q**  con el valor actual del campo. La respuesta deberá ser retornada en formato JSON y deberá tener la siguiente estructura: &nbsp; `{"success": "0 o 1", "message": "Mensaje de error", "values":{"foo":"bar"}}` Los índices **message** y **values** son opcionales
	- **message** = mostrará un cuadro de diálogo con el mensaje **data-checker-text** 
	- **values** = será tratado como un objeto JSON y se intentará actualizar en el documento los valores de los campos o los contenidos HTML de los elementos, que tengan el valor del atributo **data-id** igual a alguna de las **key** del JSON. Algo similar al [ejemplo #4](#relation---ejemplo-4) del elemento [relation](#relation)
- **data-checker-success** = mensaje que se mostrará cuando *data-checker* retorne **1** en el índice **success** y se encuentre presente el índice **message**. Se puede utilizar *** para imprimir el valor retornado en el campo **message**
- **data-checker-fail** = mensaje que se mostrará cuando *data-checker* retorne **0** en el índice **success** y se encuentre presente el índice **message**. Se puede utilizar *** para imprimir el valor retornado en el campo **message**


``` json	
"attribs": {
	"data-checker": "__knot?imya=fUyqGMP1vTnvECn2zRHCtjj5RIHVtgdz",
	"data-checker-fail": "El nombre de usuario *** ya está siendo utilizado"
}
```
&nbsp;
&nbsp;

---
&nbsp;
# Alvin
Validación de campos antes del envío.
Los argumentos deben ser pasados utilizando los atributos

- **alvin-message** = Mensaje que se le mostrará al usuario en caso de error
- **alvin-required** = Campo obligatorio
- **alvin-minlength** = valor mínimo aceptado
- **alvin-maxlength** = valor máximo aceptado
- **alvin-greaterthan** = el valor debe ser mayor o igual a
- **alvin-lessthan** = el valor debe ser menor o igual a
- **alvin-in** = el valor debe estar en este listado de valores separados por , 
- **alvin-type** = Validación del tipo de dato
  - **noempty** = el campo no puede quedar vacio
  - **all** = acepta todo tipo de caracter
  - **html** = idem **all**
  - **color** = formato hexadecimal
  - **date** = formato YYYY-mm-dd
  - **datetime** = formato YYYY-mm-dd HH:ii:ss
  - **email** = dirección de correo
  - **filename** = path local o url
  - **int** = número entero
  - **imya** = imya
  - **ipv4** = IPV4
  - **ipv6** = IPV6
  - **number** = número entero o decimal, positivo o negativo. Con separadores , y/o .
  - **time** = HH:ii:ss ó HH:ii
  - **url** = URL
  - **alpha** = sólo letras
  - **string** = números y letras
  - **text** = números y letras
  - **symbols** = sólo símbolos

En caso de utilizar la estructura **JSON** para el armado del formulario, los atributos deben declararse dentro del índice **jsalvin** 

### Ejemplo HTML
```html
	<label for="age">Edad (18 a 65)</label>
	<input id="age" type="text" name="age" 
		alvin-required="true" 
		alvin-type="int" 
		alvin-greaterthan="18" 
		alvin-lessthan="65" 
		alvin-message="Valor inválido, rango 18 a 65" 
	/>
```

### Ejemplo JSON
```json
["input", 
	{
		"label": "Edad (18 a 65)",
		"name": "age",
		"jsalvin": {
			"required": "true",
			"type": "int",
			"greaterthan": "18",
			"lessthan": "65",
			"message": "Valor invalido, rango 18 a 65"
		}
	}
]
```
&nbsp;
&nbsp;

---
&nbsp;

# Elementos de Datos
En la sección **Atributos** de cada elemento enumeran los atributos particulares y opcionales del elemento que se suman a los atributos comunes.  

## attacher
Zona para drag a drop para adjuntar archivos
### Variantes
- **attacher** es la clásica zona
- **attacher-name** adjunta los archivos sin generar preview
- **attacher-image** acepta un único archivo del tipo imagen, se usa para fotos de perfil

### Atributos
#### opcionales
- **types** = tipos de archivos separados por coma (,)
- **max** = maximo de archivos permitidos
- **autohide** = define si se debe ocultar la zona drop despues de adjuntar: true o false
- **value** =  opcional, json con archivos precargados `[{"name":"nombre_archivo", "path":"ruta_archivo", "size":"tamaño_bytes"}, {...} ]`

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
**con la opción de agregar valores a la lista**

```json
["autocomplete", {
	"name": "town",
	"label": "Barrio",
	"source": "__knot?imya=uEdVChLkoZkKbHsYQtiiulHQNwnihLxd",
	"addtext": "Agregar Nuevo Barrio",
	"addsource": "add.html"
}]
```

**ejecutando un método después de la selección**
> el método deberá estar previamente declarado y pasado por medio de ```"attribs": {"data-after":"myMethod"}```  
> al método le serán pasado 3 argumentos, el objeto *autocomplete*, los datos del *item* seleccionado y true en caso de ser una carga para edición

```json
["autocomplete", {
	"name": "town",
	"label": "Barrio",
	"source": "__knot?imya=uEdVChLkoZkKbHsYQtiiulHQNwnihLxd",
	"addtext": "Agregar Nuevo Barrio",
	"addsource": "add.html",
	"attribs": {"data-after":"myMethod"}
}]
```
``` js
function myMethod(a) {
	console.debug(a);
}
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
- **selector** = selector de rangos de fechas (sólo disponible en **date-range**) Cadena separada por comas, las opciones son:
	- | = separador
	- today = hoy
	- yesterday = ayer
	- tomorrow = mañana
	- week = semana actual
	- month = mes actual
	- pastWeek = última semana
	- pastFortnight = última quincena
	- pastMonth = último mes
	- pastQuarter = último trimestre
	- pastSemester = último semestre
	- pastYear = último año
	- nextWeek = próxima semana
	- nextFortnight = próxima quincena
	- nextMonth = próximo mes
	- nextQuarter = próximo trimestre
	- nextSemester = próximo semestre
	- nextYear = próximo año

```json
["date", {"type":"datepicker", "name":"name", "label":"Fecha"}]
```

```json
["date-range", {
	"type": "monthpicker",
	"name": "name",
	"name2": "name2",
	"label": "Fechas",
	"selector": "pastWeek,pastMonth,pastSemester,pastYear",
	"attribs": {
		"data-min": "2018-01-01",
		"data-max": "2018-12-01"
	}
}]
```
&nbsp;

## days
Selector de los días de la semana.  
El formato de los datos guardados es una cadena de 7 caracteres donde:
- **D** = Domingo
- **L** = Lunes
- **M** = Martes
- **X** = Miércoles
- **J** = Jueves
- **V** = Viernes
- **S** = Sábado

```json
["days", {"name":"days", "label":"Días de la semana"}]
```
&nbsp;

## file
Clásico input file

```json
["file", {"name":"document", "label":"Documento"}]
```
&nbsp;

## coords
Mapa de Google con un PIN que puede ser arrastrado para obtener un par de coordenadas, las cuales se almacenan en un campo oculto separadas por un ;  
También es posible pegar el link corto que genera Google Maps para compartir una ubicación

```json
["coords", {"label":"Coordenadas", "name":"coords"}]
```
&nbsp;

## hidden
Clásico input hidden. Por su característica de oculto, este elemento no soporta el atributo **label**

```json
["hidden", {"name":"id"}]
```
&nbsp;

## input
Clásico input text
### Atributos
#### opcionales
- **prepend** = antepone al campo una caja para código HTML
- **append** = coloca después al campo una caja para código HTML
- **mask** = máscara de ingreso. Valores esperados
  - **decimal** = 0.00
  - **money** = 0,000,000.00
  - **cuit** = 00-00000000-0
  - **dni** = 00.000.000
  - **ip** = 000.000.000.000
  - **cbu** = 0000000000000000000000
  - **phone** = 0000 0000
  - **phone_area** = (00) 0000 0000
  - **cellphone** = 15 0000 0000
  - **cellphone_area** = (00) 15 0000 0000

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

## select
Lista desplegable de opciones
### Atributos
- **source** = opciones en formato `{label,value}` donde *label* será el texto a mostrar y *value* el valor del option. También se puede utilizar `{group,label,value}` para agrupar opciones
#### opcionales
- **empty** = texto que se mostrará cuando no se encuentren opciones en *source*, utilizado con *selects* dinámicos
- **nodefault** = si tiene seteado cualquier valor, no se mostrará la opción por defecto **--** con valor **0**
- **lnk-source** = URL del origen de datos de un **combo relacionado**, que se ejecutará al seleccionar un valor
- **lnk-target** = jquery selector del combo relacionado, *select*, *radio** o *checkbox*
- **attribs**
	- **multiple** = determina si es un `<select>` de selección multiple, true o false
	- **size** = cantidad de valores visibles en el select tamaño
- **prepend** = antepone al campo una caja para código HTML
- **append** = coloca después al campo una caja para código HTML

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

## Social
Clásico input text con un icono de red social que lo antecede.
### Atributos
- **type** = nombre de la red social
	- behance
	- facebook
	- flickr
	- google-plus
	- instagram
	- linkedin
	- pinterest
	- snapchat
	- twitter
	- youtube

```json
["social", {
	"type":"twitter",
	"name":"name",
	"label":"Usuario de Twitter"
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

## tags
Selector de etiquetas. Estas pueden ser libres y/o provenientes de un autocomplete si se espeficita *source*
### Atributos
- **source** = por convencion las opciones se esperan en formato {label,value} pero el *label* será ignorado, el *value* será el texto a consignar en la etiqueta
- **clear** = expresión regular que determina que caracteres deben eliminarse del tag antes de añadirlo a la lista
- **length** = longitud máxima para cada tag (cantidad de caracteres)

```json
["tags", {
	"name":"name",
	"label":"tags",
	"source":"data.json?",
	"clear": "^0-9"
}],
```
&nbsp;

## textarea
Clasico campo `<textarea>` o versiones enriquecidas
### Atributos
- **type** = tipo de `<textarea>`, las opciones son:
    - **limited** = limita la cantidad de caracteres y añade un contador. El límite por defecto es 255 pero puede especificarse en el atributo **limit**
	- **dynamic** = dinámico, el campo se agranda automaticamente para su edición pero luego vuelve a ocupar pocas líneas
	- **fullscreen** = añade la posibilidad de llevar el `<textarea>` al 90% del alto y ancho de la ventana
	- **fullinput** = igual a **fullinput** con la diferencia que plegado se muestra como un campo **input**
	- **wysiwyg-lite** = editor WYSIWYG con funcionalidades mínimas: `<b><i><u>`
	- **wysiwyg** = editor WYSIWYG estandar: `<b><i><u>` + alineaciones
	- **wysiwyg-full** = editor WYSIWYG completo

- **limit** = límite de caracteres cuando el tipo es **limited**

```json
["textarea", {
	"type":"wysiwyg",
	"name":"name",
	"label":"textarea",

}],
```
&nbsp;

## year
Selector de año. Variante de **date** con `"type":"yearpicker"`
### Atributos
#### opcionales
- **attribs**
	- **data-min** = menor valor posible, sólo para los tipo *picker*
	- **data-max** = máximo valor posible, sólo para los tipo *picker*

```json
["date", {"name":"name", "label":"Año"}]
```
&nbsp;
&nbsp;

---
&nbsp;

# Elementos Avanzados
## relation
El elemento se muestra como un botón que abre un diálogo en el que se pueden cargar diferentes tipos de documentos.
El propósito del elemento es generar vinculos entre el formulario actual y datos adyacentes. Conceptualmente es identico a un elemento combo (select, radios, etc), pero ofrece un mayor control y versatilidad. Los valores seleccionados son pasados al formulario principal por medio de los eventos de los elementos que tengan el atributo **data-relation**, en cuyo caso se agregará a **target** el contenido del comentario del mismo.

Hay diferentes variantes para este elmento, algunos de sus usos:
- dar de alta un registro *hijo* y vincularlo al *padre*. Ej: datos de la persona responsable de una empresa [ver ejemplo #2](#relation---ejemplo-2)
- seleccionar uno o varios valores de un grupo de resultados. Ej: vincular rubro/s a un producto
- buscar un valor, si no existe darlo de alta, si existe seleccionarlo del grupo de resultados. Ej: seleccionar el cliente en una factura

### Atributos
- **id** = id del objeto del *diaolog* en donde se cargarán los contenidos dinámcicos. **IMPORTANTE** Luego de cargar nuevos contenidos deberá ejecutarse el método `$("#VALOR_ID").trigger("relation");` para que sean parseados. [ver ejemplo #3](#relation---ejemplo-3)
- **button** = texto que se mostrará en el botón
- **source** = URL del formulario. Puede contener variables, esto permite, por ejemplo, pasar el **id** de un registro maestro
#### opcionales
- **value** = URL de un documento que contiene los valores previamentes cargados (ej: tabla con contactos). Este documento se cargará en *target* al inicio (excepto con *skipfirst =* **true**) y después de cada envio satisfactorio. Si el valor es **true** la respuesta de *source* será tratada como un JSON y se intentará actualizar en el documento los valores de los campos o los contenidos HTML de los elementos, que dentro de *target*, tengan el valor del atributo **data-id** igual a alguna de las **key** del JSON [ver ejemplo #4](#relation---ejemplo-4)
- **target** = selector jquery de la zona en donde se cargará *value*
- **skipfirst** = evita el primer llamado al domumento *value*. Valore aceptados "true" y "false"
- **closebutton** = determina si debe mostrarse el botón "cerrar" en el diálogo, se espera *1* o *true*, por defecto: *false*
- **isform** = determina si el contenido de el diálogo debe presentarse como un formulario, se espera *0* o *false* por defecto: *true*
- **size** = tamaño del dialogo (hg xl lg md sm xs)
#### en los documentos *source*
- **data-relation** = por medio de este atributo se puede asignar funcionalidades a los elementos de los documentos cargados en el diálogo
	- **close** = al hacer click sobre el elemento se cerrará el diálogo
	- **multiple** = añade a *target* el contenido del comentario del elemento [ver ejemplo #2](#relation---ejemplo-2)
	- **once** = setea de manera única el contenido del comentario en el *target* [aplicable al ejemplo #2](#relation---ejemplo-2)
	- **onceclose** = identico al anterior, pero luego de setearlo cierra el diálogo [ver ejemplo #3](#relation---ejemplo-3)
- **data-relation-tag** = cuando el contenido 


##### [relation - ejemplo 1]
```json
["html", {"code": "<div id='contzone'></div>"}],
["relation", {
	"button": "nuevo contacto",
	"class": "btn btn-primary",
	"value": "contacts.php?parent=1234",
	"source": "new_contact.php?parent=1234", 
	"target": "#contzone",
	"isform": "true",
	"skipfirst": "true",
	"size": "md"
}]
```
``` html
*** Tabla con contactos previos (contacts.php) ***

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
*** formulario para agregar nuevos contactos (new_contact.php) ***

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

##### [relation - ejemplo 2]
```json
["html", {"code":"<div id='categories' class='padding-sm brd-solid brd-gray brd-xs brd-round-xs'></div>"}],
["relation", {
    "button": "seleccionar categorías",
    "closebutton": "true",
    "class": "btn btn-primary",
    "source": "categories.php", 
    "target": "#categories",
    "size": "md"
}],
```
``` html
*** Tabla de selección de Categorías (categoires.php) ***

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

#### [relation - ejemplo 3]
```json
["html", {"code":"<div id='marks' class='padding-sm brd-solid brd-gray brd-xs brd-round-xs'></div>"}],
["relation", {
	"id": "searchrel",
    "button": "seleccionar marca",
    "class": "btn btn-primary",
    "source": "search.php", 
    "target": "#marks",
    "size": "md"
}]
```
```html
*** Buscador (search.php) ***

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
*** Resultados de búsqueda (result.php) ***

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

#### [relation - ejemplo 4]
```html
*** Tabla de datos ***

<table class='table table-bordered'>
	<thead>
		<tr>
			<th>#</th>
			<th>Nombre</th>
			<th>Apellido</th>
			<th>Usuario</th>
			<th>Acciones</th>
		</tr>
	</thead>
	<tbody>
		<tr id='user-1'>
			<th>1</th>
			<td>Mark</td>
			<td>Otto</td>
			<td data-id='username'>@motto</td>
			<td class='text-center'>
				<input type='button' value='editar' class='btn btn-primary btn-sm form-relation' 
					data-target='#user-1' 
					data-source='relation_form_update?id=1234' 
					data-value='true' 
					data-isform='true' 
					data-skipfirst='true' 
				/>
			</td>
		</tr>
	</tbody>
</table>
```
```html
*** Formulario de Edición (result.php) ***

<form action="demo/action" method="post">
	<input type="hidden" name="NGL_ONCECODE" value="<rind:once />" />
	<div class="row margin-md margin-only-bottom">
		<div class="col">
			<rind:mergefile>
				<@source>/bithive/forms</@source>
				<@multiple json>
					[
						["input", {"name":"firstname", "label":"Nombre", "value":"Mark", "attribs": {"readonly":"readonly"}}],
						["input", {"name":"lastname", "label":"Apellido", "value":"Otto", "attribs": {"readonly":"readonly"}}],
						["input", {"name":"username", "label":"Usuario", "value":"@motomoto", "attribs": {"readonly":"readonly"}}]
					]
				</@multiple>
			</rind:mergefile>
		</div>
	</div>
</form>
```

&nbsp;
## subform
El elemento se muestra como un botón que agrega al formulario actual un campo o bloque de ellos.
Al igual que [relation](#relation), el propósito del elemento es generar vinculos entre el formulario actual y datos adyacentes, pero su uso se centraliza en las uniones ***padre/hijo*** 
Ej: ingredientes de una receta

### Atributos
- **button** = texto que se mostrará en el botón. Si se deja vacío, no se mostrará el botón para agregar nuevos sub-formulaios.
- **source** = URL del sub-formulario. Puede contener variables, esto permite, por ejemplo, pasar el **id** de un registro maestro
#### opcionales
- **default** = cantidad de sub-formularios que se cargarán la primera vez
- **value** = Array bidimensional en formato JSON y encodeado como BASE64. Donde cada entrada del Array será un conjunto de claves/valor con los datos de cada sub-formulario. Para conseguir este tipo de dato podemos utilizar, por ejemplo:
	```php
	$aIngredients = base64_encode(json_encode(array(
		array("ingrid"=>1, "ingredient"=>"1", "quantity"=>"300"),
		array("ingrid"=>2, "ingredient"=>"2", "quantity"=>"2"),
		array("ingrid"=>3, "ingredient"=>"3", "quantity"=>"180")
	)));
	```
	O en **rind**, si los ingredientes formaran parte de un array mayor llamado *recipe*
	```html
	<rind:set>
		<@name>ingredients</@name>
		<@value>{$_SET.recipe.ingredients}</@value>
		<@keys>ingrid,ingredient,quantity</@keys>
		<@method>chkeys,jsonenc,base64enc</@method>
	</rind:set>
	```
- **target** = selector jquery de la zona en donde se cargará *source*. En caso de no especificarse el sistema generará un *target* automaticamente
- **limit** = número máximo de **subforms** admitidos en **target**. Dejar vacío o en 0 para ilimitado
- **limit-message** = mensaje de alerta que se mostrará cuando **limit** sea alcanzado
- **afteradd** = nombre de función que se ejecutará luego de añadir un **subform**. El **subform** es pasado como argumento
- **afterrem** = nombre de función que se ejecutará luego de eliminar un **subform**


#### en los documentos *source*
- **data-subform-id** = campo del conjunto de datos **value** que se utilizará para identificar al sub-formulario en caso de *update* y *remove*. 
- **data-subform** = por medio de este atributo se puede asignar funcionalidades a los elementos de los documentos cargados en el diálogo
	- **number** = número de orden del sub-formulario dentro del grupo
	- **up** =  mueve el sub-formulario hacia arriba, colocandolo por encima del sub-formulario inmediato anterior
	- **down** = mueve el sub-formulario hacia abajo, colocandolo por debajo del sub-formulario inmediato posterior
	- **update** = cuando existe bloquea la edición de los campos y la eliminacion del sub-formulario. Al hacer click activa la edición y crea un campo oculto que tiene por nombre el valor del atributo *data-subform-id* + *_update* y le asigna el valor el almacenado en *value* para ese indice. Esto actua como FLAG de modificación. (ver ejemplo)
	- **remove** = remueve el sub-formulario y de existir *data-subform-id* crea un campo oculto que tiene por nombre el valor del atributo + *_update* y le asigna el valor el almacenado en *value* para ese indice. Esto actua como FLAG de eliminación. (ver ejemplo)
- **data-subform-nested** =  URL de un sub-sub-formulario que se anidará debajo del subformulario actual. Si el valor es **true** se anidará el mismo formulario

##### [subform - ejemplo 1]
```json
	["section", {"title":"Ingredientes"}],
	["subform", {"button":"agregar ingrediente", "source":"ingredients.php", "class": "btn btn-primary", "default":"0", "value":"{$aIngredients}"}]
```
```html
*** Sub-Formulario (ingredients.php) ***

<rind:mergefile>
    <@source>_templates_/forms</@source>
    <@multiple json>
        [
            ["hidden", {"name":"ingrid"}],
            ["html", {"code":"
                <div class='text-left'>
                    <b class='text-lg'># <span class='text-lg' data-subform='number'></span></b>
                    <span class='pull-right'>
                        <span class='btn btn-primary btn-xs margin-xs' data-subform='update' data-subform-id='ingrid'>habilitar edición</span>
                        <span class='btn btn-danger btn-xs margin-xs margin-none-left' data-subform='remove' data-subform-id='ingrid'>eliminar</span>
                        <i class='btn btn-default btn-sm fa fa-chevron-up cursor-pointer' data-subform='up'></i>
                        <i class='btn btn-default btn-sm fa fa-chevron-down cursor-pointer margin-xs' data-subform='down'></i>
                    </span>
                </div>
            "}],
            ["cols", {"cols":"2", "open":"1"}],
                ["select", {
                    "label": "Ingrediente",
                    "name": "ingredient",
                    "source": "ingredients.json",
                    "groupclass": "margin-none-bottom"
                }], 
                ["input", {"name":"quantity", "label":"Cantidad"}],
            ["cols", {"close":"true"}],
            ["divider", {"class":"brd-gray"}]
        ]
    </@multiple>
</rind:mergefile>
```
```json
*** ingredients.json ***

[
    {"value": "1", "label": "Harina"},
    {"value": "2", "label": "Huevo"},
    {"value": "3", "label": "Manteca"},
    {"value": "4", "label": "Sal"},
    {"value": "5", "label": "Azúcar"},
    {"value": "6", "label": "Leche"}
]
```
&nbsp;
&nbsp;

---
&nbsp;

# Elementos GUI
## break
Añade un espacio vertical. Este elemento unicamente soporta el atributo *class* y de manera opcional

```json
["break"]
```
&nbsp;

## cols
Agrupa los campos de 2 a 5 columnas en resoluciones mayores a **992px**, salvo cuando el valor de cols es 1, en cuyo caso se comportará como una fila y donde el ancho de las columnas deberá ser especificado en el atributo **groupclass** de los elementos
En resoluciones menores, los bloques de 4 y 5 columnas se transformarán en 2 columnas, y los bloques de 2 y 3 en 1 columna.
Un bloque de columnas se inicia con la llamada de un elemento **cols** con el atributo *open* y se cierra con la llamada de otro elemento **cols** que tenga presente el atributo *close*
Cada llamada al elemento que contenga unicamente el atributo *cols*, cerrará el bloque anterior de columnas e iniciará uno nuevo
### Atributos
- **cols** = cantidad de columnas 1 a 5
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
["cols", {"cols":"1"}],
	["input", {"groupclass":"2", "name":"name", "label":"Label"}],
	["input", {"groupclass":"2", "name":"name", "label":"Label"}],
	["input", {"groupclass":"8", "name":"name", "label":"Label"}],
["cols", {"close":"1"}]
```
&nbsp;

## colsgroup
Permite generar bloques con grupos de campos dentro del elemento **cols**
Un grupo se inicia con la llamada de un elemento **colsgroup** con el atributo *open* y se cierra con la llamada de otro elemento **cols** que tenga presente el atributo *close*
Cada llamada al elemento que contenga unicamente el atributo *colsgroup*, cerrará el bloque anterior de columnas e iniciará uno nuevo
### Atributos
#### opcionales
- **open** = indica el inicio del grupo, se puede utilizar 1 o true
- **close** = indica el cierre del grupo, se puede utilizar 1 o true
- **class** = clase css del grupo

```json
El tabulado del ejemplo es unicamente con fines estéticos

["cols", {"cols":"3", "open":"1"}],
	["colsgroup", {"open":"1"}],
		["input", {"name":"name", "label":"Label"}],
		["input", {"name":"name", "label":"Label"}],
		["input", {"name":"name", "label":"Label"}],
	["colsgroup"],
		["input", {"name":"name", "label":"Label"}],
		["input", {"name":"name", "label":"Label"}],
	["colsgroup"],
		["input", {"name":"name", "label":"Label"}],
		["input", {"name":"name", "label":"Label"}],
		["input", {"name":"name", "label":"Label"}],
	["colsgroup", {"close":"1"}],
["cols", {"close":"1"}]
```
&nbsp;

## divider
Añade un divisor horizontal `<hr>`. Este elemento sólo acepta el atributo **class** de manera opcional

```json
["divider"]
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

## section
Inserta una sección dentro de un formulario, la cuál puede llevar un título del tipo `<h.>`. Sino se especifica **open**, el bloque se abrirá y cerrará automaticamente.
### Atributos
#### opcionales
- **open** = inicia el bloque de sección
- **close** = finaliza el bloque de sección
- **sectiontitle** = texto del título de la sección
- **size** = tamaño de texto del título, valores de 1 a 6. Por defecto 3
- **groupclass** = clase CSS del bloque
- **class** = clase CSS del `<h.>`

```json
["section", {
	"divider":"1",
	"sectiontitle":"Sección"
}]
```
&nbsp;

## steps
Genera slides dentro de un formulario.
El grupo se inicia con la llamada de un elemento **steps** con el atributo *open* y se cierra con la llamada de otro elemento **steps** que tenga presente el atributo *close*
Cada llamada al elemento que contenga unicamente el atributo *title*, cerrará el paso anterior y abrirá uno nuevo
### Atributos
- **title** = título del Paso (slide)
- **open** = indica el inicio del grupo de pasos, se espera *1* o *true*
- **close** = indica el cierre del grupo de pasos, se espera *1* o *true*
- **buttons** = botones que se mostrarán debajo del contenido del Paso. Los botones cuentan a su vez con atributos propios
  - **label** = texto del botón
  - **class** = clase CSS
  - **action** = acción que se ejecutará al hacer click. Los valores esperados son:
    - *prev* = paso anterior
	- *next* = paso siguente
	- *#xx* = donde *xx* es un número de paso
- **before** = nombre de una función JS que se ejecutará antes de **action**
- **after** = nombre de una función JS que se ejecutará después de **action**


```json
```json
El tabulado y las columnas del ejemplo son unicamente con fines ilustrtivos
[            
	["steps", {
		"open": "1",
		"title": "Paso 1",
		"buttons": [
			{"label": "Siguiente", "action":"next"}
		]
	}],
		["input", {"name":"name", "label":"input"}],
		["input", {"name":"name", "label":"input"}]

	["steps", {
		"title": "Paso 2",
		"buttons": [
			{"label": "Anterior", "action":"prev"},
			{"label": "Siguiente", "action":"next"}
			{"label": "Ir Final", "action":"#4", "class":"ml-lg"},
		]
	}],
		["input", {"name":"name", "label":"input"}],
		["input", {"name":"name", "label":"input"}]

	["steps", {
		"title": "Paso 3",
		"buttons": [
			{"label": "Anterior", "action":"prev"},
			{"label": "Siguiente", "action":"next"}
		]
	}],
		["input", {"name":"name", "label":"input"}],
		["input", {"name":"name", "label":"input"}]

	["steps", {
		"title": "Paso 4",
		"buttons": [
			{"label": "Anterior", "action":"prev"},
			{"label": "Enviar", "class":"form-submit"}
		]
	}],
		["input", {"name":"name", "label":"input"}],
		["input", {"name":"name", "label":"input"}]

	["steps", {"close": "1" }
]

```
&nbsp;

## tabs
Genera un grupo de tabs.
El grupo se inicia con la llamada de un elemento **tabs** con el atributo *open* y se cierra con la llamada de otro elemento **tabs** que tenga presente el atributo *close*
Cada llamada al elemento que contenga unicamente el atributo *title*, cerrará el tab anterior y abrirá uno nuevo
### Atributos
- **title** = título del TAB
- **class** = clase aplicada al contenedor de todo el bloque. Solo tiene efecto cuando se encuentra presente el atributo *open*
- **open** = indica el inicio del grupo de tabs, se espera *1* o *true*
- **close** = indica el cierre del grupo de tabs, se espera *1* o *true*
- **id** = valor que se asignará al atributo **ID** del elemento **LI** utilizado como pestaña

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
___
<sub><b>revisión:</b> 2019-10-25 - <b>bithive</b> - <em>maximiza tu tiempo</em></sub><br />
<sup>&copy; 2020 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br /> 
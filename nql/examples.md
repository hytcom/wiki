# generar imya
fn imya

# BUCLES
@loop 1:20
	@print "FILA: "
	@set row {$}
	@loop ["A","B","C"]
		@print "{$}{$row} "
	endloop
	@print "<br>"
endloop
@clear

# importar csv en una nueva tabla de mysql
# primero subir el archivo
mysql import ["path/archivo/foo.csv", "nombre_tabla"]

# exportar a consola
mysql query "select * from TABLE_NAME"
{$} getall
shift html {$}


# exportar a excel
mysql query "select * from TABLE_NAME"
{$} getall
@set content {$}
excel load "text.xls"
{$} set ["A1", "{$content}"]
{$} download


# exportar a pdf
mysql query "select * from TABLE_NAME"
{$} getall
shift html {$}
@set content {$}
pdf create "demo.pdf"
{$} content {$content}
{$} download


# owl getall
mysql connect
owl connect {$}
{$} select OBJECT_NAME
{$} getall
{$} getall


# owl manager generate
mysql connect
owlm base "{$}"
{$} load owl
{$} generate


# owl manager describe all
mysql connect
owlm db {$}
{$} load "owl"
{$} describe


# owl manager describe object
mysql connect
owlm db {$}
{$} load "owl"
{$} select OBJECT_NAME
{$} describe


# bucle con mysql y variables
mysql connect
{$} query "select * from rastrilladores"
{$} getall
@loop {$}
	@set row {$}
	@get row contrasena
	alvin password {$}
	@set pass {$}
	@get row dni
	@set dni {$}
	mysql query "update rastrilladores set password='{$pass}' WHERE dni = '{$dni}'"
endloop

# mas owlm
mysql connect
owlm base "{$}"
{$} load owl

{$} create events_doctypes
{$} add ["code", "code"]
{$} add ["name", "name"]

{$} create events_documents
{$} add ["pid", "@events"]
{$} add ["type", "@events_doctypes"]
{$} add ["file", "@uploads"]
{$} add ["pdf", "boolean"]

{$} generate



# mail
mail server imap
-$: host outlook.office365.com
-$: port 993
-$: secure ssl
-$: user xxx@xxxx.com
-$: pass xxxxxx
-$: connect
-$: login
-$: mailbox INBOX
-$: getall ["ALL", "10"]
shift html -$:



# NQL - Ejemplos

## Web Services a MySQL via OWL
``` php
// conexion mysql
mysql connect

// conexion owl y eleccion de objeto
owl connect {$}
owl select ventas_ws

// loop descendente
@loop 20:1
	// fecha
	@php strtotime "-{$}day"
	@php date ["Y-m-d", "{$}"]
	@set date {$}

	// consulta a ws
	file load "http://35.184.84.25/trofeos/ws/trofeos/listReporteZonaFrancaDataBetweenFechas?fechaDesde={$date}&fechaHasta={$date}"
	{$} read

	// conversion de json en array
	shift convert ["{$}", "json-array"]

	// reasignacion de claves
	fn arrayArrayCombine [["codigo","cantidad","puntoventa"], "{$}"]
	@set data {$}

	// loop sobre los datos
	@loop {$data}
		// se agrega la fecha
		@php array_merge ["{$}", {"fecha":"{$date}"}]

		// insercion en la base de datos
		owl insert {$}
	endloop
endloop
```
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
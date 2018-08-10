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
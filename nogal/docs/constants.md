# nogal::file
Constantes

### NGL_PROJECT
Nombre del proyecto

### NGL_PROJECT_RELEASE
Version del proyecto

## RUTAS
### NGL_DOCUMENT_ROOT
document_root

### NGL_PATH_PUBLIC
Directorio público (public_html)

### NGL_PATH_PROJECT
Directorio del proyecto

### NGL_PATH_CONF
Repositorio de archivos de configuración (.conf)

### NGL_PATH_DATA
Repositorio de datos

### NGL_PATH_NUTS
Repositorio de NUTS

### NGL_PATH_SESSIONS
Repositorio de sesiones para los modos fs o sqlite

### NGL_PATH_TMP
Carpeta temporal

### NGL_PATH_TUTORS
Repositorio de tutores

### NGL_PATH_VALIDATE
Repositorio de las reglas para la validacion de variables

### NGL_PATH_VIEWS
Directorio principal de las vistas

### NGL_PATH_VIEWS_APP
Vistas PHP

### NGL_PATH_VIEWS_GUI
Vistas HTML

### NGL_URL
URL del proyecto

## SEGURIDAD
### NGL_ALVIN
Activa el control de accesos (NULL para desactivar)

### NGL_ONCECODE
Valida la vigencia de un código ONCE

### NGL_ONCECODE_TIMELIFE
Tiempo de vigencia de los códigos ONCE

### NGL_PASSWORD_KEY
Clave AES para encriptar passwords (NULL para desactivar encriptación)

### NGL_REFERER
Valida que el referer sea del mismo dominio 

### NGL_SESSION_INDEX
ID de session


CONTROLADORES ---------------------------------------------------------------
manipulacion de errores (true | false)
### NGL_HANDLING_ERRORS

formato de impresion de errores cuando NGL_HANDLING_ERRORS es true (html | text)
### NGL_HANDLING_ERRORS_FORMAT

tipo de salida de errores cuando NGL_HANDLING_ERRORS es true (boolean | code | die | print | return)
### NGL_HANDLING_ERRORS_MODE

muestra el rastreo del error cuando NGL_HANDLING_ERRORS es true (true | false)
### NGL_HANDLING_ERRORS_BACKTRACE


DIVISORES DE CADENAS --------------------------------------------------------
separador de filas
### NGL_STRING_LINEBREAK

separador de columnas
### NGL_STRING_SPLITTER

separador de números
### NGL_STRING_NUMBERS_SPLITTER


FECHA, HORA E IDIOMA --------------------------------------------------------
zona horaria: http://php.net/manual/es/timezones.php
### NGL_TIMEZONE

nombre de los meses del año
### NGL_DATE_MONTHS

nombre de los días de la semana
### NGL_DATE_DAYS

idiomas aceptados
### NGL_ACCEPTED_LANGUAGES


OTRAS -----------------------------------------------------------------------
inicia el framework ignorando los chequeos de compatibilidad
### NGL_RUN_ANYWAY

activa/desactiva la pantalla de configuración
### NGL_GARDERNER

separador de directorios
### NGL_DIR_SLASH

desarrollo: E_ALL | produccion: E_ERROR | E_WARNING | E_PARSE | E_NOTICE
### NGL_ERROR_REPORTING

indica valor nulo pudiendo ser o no NULL
### NGL_NULL

permisos aplicados a las nuevas carpetas
### NGL_CHMOD_FOLDER

permisos aplicados a los nuevos archivos
### NGL_CHMOD_FILE

google api key
### NGL_GOOGLE_API_KEY


SISTEMA ---------------------------------------------------------------------
if(!empty(NGL_URL)) {
	$NGL_URLPARTS = parse_url(NGL_URL);
	### NGL_URL_PROTOCOL
	### NGL_URL_HOST
	### NGL_URL_ROOT
	unset($NGL_URLPARTS);
}

path del archivo actual desde NGL_URL y REQUEST_URI
if((isset($_SERVER["REQUEST_URI"]))) {
	$NGL_PATH_CURRENT = explode("?
	$NGL_PATH_CURRENT = str_replace(NGL_URL, "
	define("NGL_PATH_CURRENT
	unset($NGL_PATH_CURRENT);
} else {
	define("NGL_PATH_CURRENT
}






&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
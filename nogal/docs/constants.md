# nogal - Constantes
Constantes predefinidas disponibles en cualquier script en ejecucción  

- [Proyecto](#proyecto)
- [Rutas](#rutas)
- [Seguridad](#seguridad)
- [Errores](#errores)
- [Separadores de Cadenas](#separadores-de-cadenas)
- [Fechas e Idioma](#fechas-e-idioma)
- [Otras](#otras)
- [De Sistema](#sistema)

## Proyecto
|Constante|Definición|
|---|---|
|NGL_PROJECT|Nombre del proyecto|
|NGL_PROJECT_RELEASE|Version del proyecto|

&nbsp;

## Rutas
|Constante|Definición|
|---|---|
|NGL_DOCUMENT_ROOT|document_root|
|NGL_PATH_CONF|Repositorio de archivos de configuración (.conf)|
|NGL_PATH_DATA|Repositorio de datos|
|NGL_PATH_FRAMEWORK|Directorio Nogal|
|NGL_PATH_GRAFTS|Repositorio de aplicaciones de terceros dentro del proyecto|
|NGL_PATH_LOGS|Carpeta de logs|
|NGL_PATH_NUTS|Repositorio de NUTS|
|NGL_PATH_PROJECT|Directorio del proyecto|
|NGL_PATH_PUBLIC|Directorio público (public_html)|
|NGL_PATH_PRICKOUT|Directorio que contiene los archivos fuente cuando se utiliza **prickout**|
|NGL_PATH_SESSIONS|Repositorio de sesiones para los modos fs o sqlite|
|NGL_PATH_TMP|Carpeta temporal|
|NGL_PATH_TUTORS|Repositorio de tutores|
|NGL_PATH_VALIDATE|Repositorio de las reglas para la validacion de variables|
|NGL_SANDBOX|Ruta dentro de la cual se establecerá el dominio de los objetos que trabajan con el FileSystem|
|NGL_URL|URL del proyecto|

&nbsp;

## Seguridad
|Constante|Definición|
|---|---|
|NGL_ALVIN|Activa el control de accesos (NULL para desactivar)|
|NGL_ALVIN_MODE|tipo de carga de ALVIN-TOKEN (TOKEN, TOKENUSER ó PROFILE)|
|NGL_AUTHORIZED_IPS|array de IPs autorizadas (NULL para desactivar)|
|NGL_ONCECODE|Valida la vigencia de un código ONCE|
|NGL_ONCECODE_TIMELIFE|Tiempo de vigencia de los códigos ONCE|
|NGL_PASSWORD_KEY|Clave AES para encriptar passwords (NULL para desactivar encriptación)|
|NGL_REFERER|Valida que el referer sea del mismo dominio|
|NGL_SESSION_INDEX|ID de session|

&nbsp;

## Errores
|Constante|Definición|
|---|---|
|NGL_HANDLING_ERRORS|Manipulacion de errores (true o false)|
|NGL_HANDLING_ERRORS_BACKTRACE|Muestra el rastreo del error cuando NGL_HANDLING_ERRORS es true (true o false)|
|NGL_HANDLING_ERRORS_FORMAT|formato de impresion de errores cuando NGL_HANDLING_ERRORS es true (html o text)|
|NGL_HANDLING_ERRORS_MODE|Tipo de salida de errores cuando NGL_HANDLING_ERRORS es true (boolean, code, die, print o return)|

&nbsp;

## Separadores de Cadenas
|Constante|Definición|
|---|---|
|NGL_STRING_LINEBREAK|Separador de filas|
|NGL_STRING_SPLITTER|Separador de columnas|
|NGL_STRING_NUMBERS_SPLITTER|Separador de números|

&nbsp;

## Fechas e Idioma
|Constante|Definición|
|---|---|
|NGL_ACCEPTED_LANGUAGES|Idiomas aceptados separados por coma|
|NGL_DATE_DAYS|Nombre de los días de la semana separados por coma|
|NGL_DATE_MONTHS|Nombre de los meses del año separados por coma|
|NGL_TIMEZONE|Zona horaria [http://php.net/manual/es/timezones.php](http://php.net/manual/es/timezones.php)|

&nbsp;

## Otras
|Constante|Definición|
|---|---|
|NGL_BEE|Activa/desactiva la inclusión de BEE|
|NGL_CHMOD_FILE|Permisos aplicados a los nuevos archivos (ambientes linux)|
|NGL_CHMOD_FOLDER|Permisos aplicados a las nuevas carpetas (linux)|
|NGL_DIR_SLASH|separador de directorios|
|NGL_ERROR_REPORTING|Política de reporte de errores. Ddesarrollo: E_ALL,  Produccion: E_ERROR \| E_WARNING \| E_PARSE \| E_NOTICE|
|NGL_FALLEN|Pone al sistema fuera de linea|
|NGL_GARDENER|activa/desactiva la pantalla de configuración|
|NGL_NULL|Indica valor nulo pudiendo ser o no NULL|
|NGL_RUN_ANYWAY|Inicia el framework ignorando los chequeos de compatibilidad|

&nbsp;

## Sistema
|Constante|Definición|
|---|---|
|NGL_PATH_CURRENT|Path del archivo actual desde NGL_URL y REQUEST_URI|
|NGL_PATH_CURRENT_QUERY|Cola de datos del archivo actual|
|NGL_TERMINAL|Indica si la ejecución actual proviene de la línea de comandos|
|NGL_URL_HOST|Dominio de la URL, en caso de existir|
|NGL_URL_PORT|Puerto de la URL, en caso de existir|
|NGL_URL_PROTOCOL|Protocolo de la URL, en caso de existir|
|NGL_URL_ROOT|Conjunción de NGL_URL_PROTOCOL y NGL_URL_HOST|
|NGL_WINDOWS|Indica si la ejecución actual es en un servidor Windows|

&nbsp;

___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2020 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br />
# Nogal

## Estructura y Sintaxis
|Tema|Resumen|
|---|---|
|[constantes](docs/constants.md)|<ul><li>[Proyecto](docs/constants.md#proyecto)</li><li>[Rutas](docs/constants.md#rutas)</li><li>[Seguridad](docs/constants.md#seguridad)</li><li>[Errores](docs/constants.md#errores)</li><li>[Separadores de Cadenas](docs/constants.md#separadores-de-cadenas)</li><li>[Fechas e Idioma](docs/constants.md#fechas-e-idioma)</li><li>[Otras](docs/constants.md#otras)</li><li>[Opcionales](docs/constants.md#opcionales)</li></ul>|

## **Objetos**
|Nombre|Descripción|
|---|---|
|[alvin](docs/alvin.md)|Alvin es el sistema de seguridad de **nogal**, encargado de gestionar permisos, grupos y perfiles de usuario. Mas que un objeto es un concepto que atraviesa transversalmente todo el framework.<br />Para aprender cómo aplicar Alvin, consultar la guía de [Aplicando Alvin](docs/alvinuso.md)|
|[barcode](docs/barcode.md)|Implementa la clase 'barcode-generator' para generar códigos de barras|
|[crypt](docs/crypt.md)|Implementa la clase 'phpseclib', de algoritmos de encriptación, con soporte para:<br /><ul><li>aes</li><li>blowfish</li><li>des</li><li>tripledes</li><li>rc2</li><li>rc4</li><li>rijndael</li><li>rsa</li><li>twofish</li></ul>|
|[dates](docs/dates.md)|Utilidades para operaciones con fechas y horas. Generación de Calendarios|
|[excel](docs/excel.md)||
|[file](docs/file.md)|Crea un objeto sobre un archivo ó directorio permitiendo acceder a sus propiedades, leer y escribir su contenido.|
|[files](docs/files.md)||
|[fn](docs/fn.md)||
|[ftp](docs/ftp.md)||
|[help](docs/help.md)||
|[image](docs/image.md)||
|[jsql](docs/jsql.md)||
|[mail](docs/mail.md)||
|[md](docs/md.md)||
|[mysql](docs/mysql.md)|Gestor de conexiones con bases de datos MySQL. Las consultas SQL se ejecutan a través del método **query**, que en caso de exito retornarán un objeto [mysqlq](docs/mysqlq.md)|
|[mysqlq](docs/mysqlq.md)|Controla los resultados generados por consultas a la bases de datos MySQL.|
|[nut](docs/nut.md)|Gestor de los **nuts**. Los **nuts** son clases php cuyo objetivo es agrupar procedimientos que pueden o no invocar a otros objetos del **nogal**. Son las **vistas** del modelo. Ver también la guía [NUTS Definidos por el Usuario](docs/nuts.md)|
|[owl](docs/owl.md)|Owl es el ORM de NOGAL y permite ejecutar operaciones sobre distintos objetos de base de datos. Para ver un ejemplo de uso completo ver la guía [owl paso a paso](docs/owluso.md)|
|[owlm](docs/owlm.md)|Owl Manager es la herramienta para crear y mantener la estructura de base de datos del objeto [owl](docs/owl.md), en MySQSL. Para ver un ejemplo de uso completo ver la guía [owl paso a paso](docs/owlmuso.md)|
|[pdf](docs/pdf.md)||
|[pdomysql](docs/pdomysql.md)||
|[qr](docs/qr.md)||
|[rind](docs/rind.md)||
|[sess](docs/sess.md)||
|[set](docs/set.md)||
|[shift](docs/shift.md)||
|[sqlite](docs/sqlite.md)||
|[sqliteq](docs/sqliteq.md)||
|[sysvar](docs/sysvar.md)||
|[todopago](docs/todopago.md)||
|[tree](docs/tree.md)||
|[tutor](docs/tutor.md)|Gestor de los **tutores**. Los **tutores** son los encargados de realizar las tareas de escritura dentro del sistema, son los **controladores** del modelo. Ver también la guía [Declarando Tutores](docs/tutors.md)|
|[unicode](docs/unicode.md)||
|[url](docs/url.md)||
|[validate](docs/validate.md)||

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2019 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
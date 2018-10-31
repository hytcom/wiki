# Nogal

## Estructura y Sintaxis
- [constantes](docs/constants.md)

## **Objetos**
|Nombre|Descripción|
|---|---|
|[alvin](docs/alvin.md)|Alvin es el sistema de seguridad de **nogal**, encargado de gestionar permisos, grupos y perfiles de usuario.<br />Mas que un objeto es un concepto que atraviesa transversalmente todo el framework.<br />Para aprender cómo aplicar Alvin, consultar la guía de [Aplicando Alvin](https://github.com/arielbottero/wiki/blob/master/nogal/docs/alvinuso.md)|
|[barcode](docs/barcode.md)|Implementa la clase 'barcode-generator' para generar códigos de barras|
|[crypt](docs/crypt.md)|Implementa la clase 'phpseclib', de algoritmos de encriptación, con soporte para:<br /><ul><li>aes</li><li>blowfish</li><li>des</li><li>tripledes</li><li>rc2</li><li>rc4</li><li>rijndael</li><li>rsa</li><li>twofish</li></ul>|
|[dates](docs/dates.md)|Utilidades para operaciones con fechas y horas.<br />Generación de Calendarios|
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
|[mysql](docs/mysql.md)|Gestor de conexiones con bases de datos MySQL.<br />Las consultas SQL se ejecutan a través del método **query**, que en caso de exito retornarán un objeto [mysqlq](https://github.com/arielbottero/wiki/blob/master/nogal/docs/mysql.md)|
|[mysqlq](docs/mysqlq.md)|Controla los resultados generados por consultas a la bases de datos MySQL.|
|[nut](docs/nut.md)|Gestor de los **nuts**.<br />Los **nuts** son clases php cuyo objetivo es agrupar procedimientos que pueden o no invocar a otros objetos del **nogal**. Son las *vistas* del modelo.<br />Vea también la guía[NUTS Definidos por el Usuario](https://github.com/arielbottero/wiki/blob/master/nogal/docs/nuts.md)|
|[owl](docs/owl.md)||
|[owlm](docs/owlm.md)|Owl Manager es la herramienta para crear y mantener la estructura de base de datos del objeto [owl](https://github.com/arielbottero/wiki/blob/master/nogal/docs/owl.md), en MySQSL.<br />Para ver un ejemplo de uso completo ver la guía [owlm paso a paso](https://github.com/arielbottero/wiki/blob/master/nogal/docs/owlmuso.md)|
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
|[tutor](docs/tutor.md)||
|[unicode](docs/unicode.md)||
|[url](docs/url.md)||
|[validate](docs/validate.md)||

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
# nogal - Objetos [documento y subdocumentos en desarrollo]
Los objetos en nogal se invocan atraves de la variable **$ngl**, no en necesario incluir librerías ni instanciarlos de manera previa.

```php
# sintáxis
$ngl("<OBJETO>")-><METODO>(<ARGUMENTOS>);

# ejemplo
$ngl("file")->load("/tmp/doc.txt")->read();

```

## Indice de objetos
|Nombre|Tipo|Descripción|
|---|---|---|
|[alvin](docs/alvin.md)|feeder|Alvin es el sistema de seguridad de **nogal**, encargado de gestionar permisos, grupos y perfiles de usuario. Mas que un objeto es un concepto que atraviesa transversalmente todo el framework.<br />Para aprender cómo aplicar Alvin, consultar la guía de [Aplicando Alvin](docs/alvinuso.md)|
|[barcode](docs/barcode.md)|graft|Implementa la clase 'barcode-generator' para generar códigos de barras|
|[bee](docs/bee.md)|feeder||
|[coon](docs/coon.md)|branch||
|[crypt](docs/crypt.md)|graft|Implementa la clase 'phpseclib', de algoritmos de encriptación, con soporte para:<br /><ul><li>aes</li><li>blowfish</li><li>des</li><li>tripledes</li><li>rc2</li><li>rc4</li><li>rijndael</li><li>rsa</li><li>twofish</li></ul>|
|[dates](docs/dates.md)|feeder|Utilidades para operaciones con fechas y horas. Generación de Calendarios|
|[dbase](docs/dbase.md)|branch||
|[excel](docs/excel.md)|graft||
|[file](docs/file.md)|branch|Crea un objeto sobre un archivo ó directorio permitiendo acceder a sus propiedades, leer y escribir su contenido.|
|[files](docs/files.md)|feeder||
|[fn](docs/fn.md)|feeder||
|[ftp](docs/ftp.md)|branch||
|[image](docs/image.md)|branch||
|[jsql](docs/jsql.md)|feeder|JSQL es una sintáxis que busca estandarizar las consultas SQL en un formato JSON. El objeto **jsql** proporciona un conjunto de métodos que posibilita el parseo de dichas cadenas.|
|[mail](docs/mail.md)|branch||
|[md](docs/md.md)|branch||
|[mysql](docs/mysql.md)|branch|Gestor de conexiones con bases de datos MySQL. Las consultas SQL se ejecutan a través del método **query**, que en caso de exito retornarán un objeto [mysqlq](docs/mysqlq.md)|
|[mysqlq](docs/mysqlq.md)|branch|Controla los resultados generados por consultas a la bases de datos MySQL.|
|[nest](docs/nest.md)|branch|Nest es la herramienta para crear y mantener la estructura de base de datos del objeto [owl](docs/owl.md), en MySQSL. Para ver un ejemplo de uso completo ver la guía [owl paso a paso](docs/owluso.md)|
|[nut](docs/nut.md)|feeder|Gestor de los **nuts**. Los **nuts** son clases php cuyo objetivo es agrupar procedimientos que pueden o no invocar a otros objetos del **nogal**. Son las **vistas** del modelo. Ver también la guía [NUTS Definidos por el Usuario](docs/nuts.md)|
|[ocr](docs/pdf.md)|graft||
|[owl](docs/owl.md)|branch|Owl es el ORM de NOGAL y permite ejecutar operaciones sobre distintos objetos de base de datos. Para ver un ejemplo de uso completo ver la guía [owl paso a paso](docs/owluso.md)|
|[pdf](docs/pdf.md)|graft||
|[pdomysql](docs/pdomysql.md)|branch||
|[qparser](docs/qparser.md)|feeder||
|[qr](docs/qr.md)|graft||
|[rind](docs/rind.md)|branch||
|[sess](docs/sess.md)|feeder||
|[set](docs/set.md)|branch||
|[shift](docs/shift.md)|feeder||
|[sqlite](docs/sqlite.md)|branch||
|[sqliteq](docs/sqliteq.md)|branch||
|[sysvar](docs/sysvar.md)|feeder||
|[tree](docs/tree.md)|branch||
|[tutor](docs/tutor.md)|branch|Gestor de los **tutores**. Los **tutores** son los encargados de realizar las tareas de escritura dentro del sistema, son los **controladores** del modelo. Ver también la guía [Declarando Tutores](docs/tutors.md)|
|[unicode](docs/unicode.md)|feeder||
|[url](docs/url.md)|branch||
|[validate](docs/validate.md)|feeder||
|[zip](docs/zip.md)|branch||
 
&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2020 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br />
# rind - Clase 4 - Seteo de Variables
Plantillas HTML
___

# Contenidos
- [Variable $_SET](#variable--set)
- [Comando SET](#comando-set)
- [SET Métodos](#set-metodos)
- [Métodos Múltiples](#metodos-multiples)
<br />

## Variable $_SET
Desde el entorno de las plantillas es posible acceder a todas las variables generadas en el ambito PHP, siempre que no sean bloquedas por el programador de ese entorno.
Pero la variable **$_SET** siempre estará disponible, y es el canal que puede utilizar el programador PHP para dar acceso a determinados datos.
También es la variable en la que se almacenarán todos los valores seteados por medio de **rind:set**

Para acceder a un valor de **$_SET** se debe emplear la siguiente sintáxis:
- **{\$_SET.name}** o **{\$_SET["name"]}** para variables simples
- **{\$_SET.name.foo.bar}** o **{\$_SET["name"]["foo"]["bar"]}** para arrays
<br />
<br />

## Comando SET
El comando **rind:set** es probablemente el mas completo de todo el sistema de plantillas. Con él no sólo podemos setear un valor en una variable, sino que antes de hacerlo podemos procesarlo con uno o varios métodos de parseo.

La estructura básica del comando es:

``` php
<rind:set>
    <@name></@name>
    <@value></@value>
    <@operator></@operator>
</rind:set>
```
Aunque **operator** no es necesario en el caso de las asignaciones (=)
<br />

Para setear una variable sólo debemos hacer:

``` php
*** index.html ***

<rind:set>
    <@name>username</@name>
    <@value>John Smith</@value>
</rind:set>

Hola <b>{$_SET.username}</b>
```

``` txt
*** Navegador ***

Hola <b>John Smith</b>
```
<br />
<br />

## SET Métodos
El comando **rind:set** cuenta con una gran cantidad e métodos para aplicar sobre los datos antes de ser almacenados.
Los métodos se aplican por medio del parámetro <@method>, y pueden aplicarse de a uno o en grupo, separandolos por coma (,)

Estos son los métodos disponibles:
- **_N_** = el contenido alamcenado contendrá al elemento **_N_** del conjunto **value**, ya sea el valor de un índice cuando sea un array o un caracter cuando sea una cadena.<br />**_N_** comienza a contar desde 1, cuando el valor sea negativo se contará desde el final del conjunto.<br />Cuando el resultado sea un array bidimensional de un único subindice, este será retornado como vector. Para conseguir un array de vectores se deberá anteponer el método **each**
- **_N:L_** = el contenido alamcenado será la secuencia de elementos del conjunto **value** tal y como se especifiquen los valores **_N_** y **_L_**. Donde el primero es la posición del primer elemento y el segundo la cantidad de elementos a capturar.<br />**_N_** comienza a contar desde 1, cuando el valor sea negativo se contará desde el final del conjunto.
- **_N\|N1\|N2\|Nn_** = el contenido alamcenado será la secuencia de elementos del conjunto **value** tal y como se especifiquen los valores **_N_**. Donde cada **N** es la posición del elemento a capturar. **_N_** puede ser un valor entero o una cadena de texto para el caso de arrays asociativos.<br />Los números enteros comienza a contar desde 1 y deben ser siempre mayores a 0.
- **base64dec** = antes de ser almacenado el contenido será tratado con **base64_decode**
- **base64enc** = antes de ser almacenado el contenido será tratado con **base64_encode**
- **chkeys** = cuando el contenido sea un array, sus claves serán reemplazadas por los valores de la cadena <@keys>
- **each** = cuando el contenido sea un array bidimensional, indica que el siguiente método deberá aplicarse de manera recursiva sobre todos sus elementos
- **element** = en caso de existir, el contenido alamcenado será el resultado de ejecutar el método **get** del objeto **value**
- **elements** = en caso de existir, el contenido alamcenado será el resultado de ejecutar el método **getall** del objeto **value**
- **explode** = antes de ser almacenado el contenido será explotado utilizando **splitter** como separador. Opcionalmente se podrá especificar el parámetro **linebreak** como salto de línea, consiguiendo asi un array multidimensional
- **file** = el valor será tratado como un path de archivo y se intentará obtener su contenido. Cuando se especifiquen mas de un método, sólo el primero de ellos podrá ser **file**.
- **filter** = trabaja sobre un array de datos el cual es tratado con **array_filter**, la condición evaluada será la de argumento <@filter> y debe repetar la sintáxsis del comando [ifcase](#ifcase) en su modalidad **inline** sin los  aréntesis. En cada iteración se pasarán al métodos 2 valores:
  - **$k** clave de la vuelta actual
  - **$v** datos de la vuelta
- **group** = agrupa los valores según la estructura **structure** Cuando no se proporcione una estructura, los valores únicos de cada columna formarán subgrupos
- **implode** = antes de ser almacenado el contenido será tratado con **implode** utilizando **splitter** como caracter de unión
- **jsondec** = antes de ser almacenado el contenido será tratado con **json_decode** y convertido en un array asociativo
- **jsonenc** = antes de ser almacenado el contenido será tratado con **json_encode** con los flags:JSON_HEX_TAG \| JSON_NUMERIC_CHECK \| JSON_HEX_APOS \| JSON_HEX_QUOT \| JSON_HEX_AMP \| JSON_UNESCAPED_UNICODE
- **keys** = trabaja sobre un array de datos el cual es tratado con **array_keys**, es decir, que retorna un vector con las claves del valor de origen
- **length**/**len** = calcula el largo del valor, utilizando **count** y **strlen**
- **nest** = trabaja sobre un array, anidando sus elementos convirtiendose asi en un árbol. Para ello se deberá definir una relación de dos indices (padre,hijo) en el argumento <@relation>
- **number** = se eliminarán del contenido todos los caracteres NO numéricos antes almacenarlo. Si el resultado de la limpieza fuese un valor NO numérico, el valor será 0
- **object** = antes de ser almacenado el contenido será convertido en un array de manera recursiva
- **querydec** = antes de ser almacenado el contenido será tratado con **parse_str** y convertido en un array
- **queryenc** = antes de ser almacenado el contenido será tratado con **http_build_query**
- **rawurldec** = antes de ser almacenado el contenido será tratado con **rawurldecode**
- **rawurlenc** = antes de ser almacenado el contenido será tratado con **rawurlencode**
- **serialenc** = antes de ser almacenado el contenido será tratado con **serialize**
- **serialdec** = antes de ser almacenado el contenido será tratado con **unserialize** y convertido en un array
- **urldec** = antes de ser almacenado el contenido será tratado con **urldecode**
- **urlenc** = antes de ser almacenado el contenido será tratado con **urlencode**
- **vector** = genera un vector clave/valor a partir de un array bidimensional, con **index** como clave y **column** como valor
- **xml** = antes de ser almacenado el contenido será convertido en un array


Veamos un ejemplo rápido de la aplicación de un método

``` php
*** index.html ***

<rind:set>
    <@name>user</@name>
    <@value>name=John Smith&email=jsmith@zaraza.com&score=12345</@value>
    <@method>querydec</@method>
</rind:set>

Nombre: <b>{$_SET.user.name}</b><br />
Email: <b>{$_SET.user.email}</b><br />
Puntos: <b>{$_SET.user.score}</b><br />
```

``` txt
*** Navegador ***

Nombre: <b>John Smith</b>
Email: <b>jsmith@zaraza.com</b>
Puntos: <b>12345</b>
```
<br />
<br />

## Métodos Múltiples
Es importante tener en cuenta que los métodos se aplican de manera secuencial, es decir, que se aplican de a uno y uno despues del otro en el orden declarado.

Veamos como podriamos obtener el domino del email del ejemplo anterior.

``` php
*** index.html ***
<rind:set>
    <@name>domain</@name>
    <@value>name=John Smith&email=jsmith@zaraza.com&score=12345</@value>
    <@method>querydec,2,explode,2</@method>
    <@splitter>@</@splitter>
</rind:set>

Dominio: <b>{$_SET.domain}</b><br />
```

``` txt
*** Navegador ***

Dominio: <b>zaraza.com</b>
```

**Explicación:**
- transformamos la cola de la URL en un Array
- tomamos el segundo elemento (email)
- transformamos el email en un Array, explotandolo por la @ (para eso seteamos @ como splitter)
- tomamos el segundo elemento (dominio)
<br />
<br />
<br />

Para ver la descripción completa del comando ver la [ayuda](commands.md#set)

---
- [Clase 3 - Bucles](clase-03.md)

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2021 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br />
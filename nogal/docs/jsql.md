# jsql
JSQL es una sintáxis que busca estandarizar las consultas SQL en un formato JSON. El objeto **jsql** proporciona un conjunto de métodos que posibilita el parseo de dichas cadenas.  
Luego, cada objeto de base de datos deberá contar con un método que interprete esos datos y los traduzca en una sentencia SQL válida para su motor.  

Para los condicionales en JOIN, WHERE y HAVING, o asignación de valores en INSERT y UPDATE, los nombres de los campos deben ir entre **[ ]**.
```json
    ...["[tabla1.campo1]", "eq", "[tabla2.campo1"]...
    ...["[tabla1.campo1]", "eq", "foobar"]...
```

## Estructuras de las queries

**select**
``` json
{
    "query" : "select",
    "columns" : [
        ["tabla.campo1", "foo"],
        ["alias2.campo2"],
        ["campo3","bar"]
    ],
    "tables" : [
        ["tabla"],
        [
            "tabla2",
            "alias2", [
                ["[tabla.campo1]","eq", "[alias2.campo2]"],
                "AND",
                [
                    ["[tabla.campo2]", "gt", "[alias2.campo3]"], 
                    "OR",
                    ["[tabla.campo2]", "lt", "alias2.campo4"]
                ],
                "AND",
                ["[tabla.campo3]", "like", "I'm John"],
                "AND",
                ["[tabla.campo4]", "eq", "2"]
            ]
        ]
    ],
    "where" : [
        ["[tabla.campo1]", "eq", "[alias2.campo2]"],
        "AND",
        [
            ["[tabla.campo2]", "gt", "[alias2.campo3]"], 
            "OR",
            ["[tabla.campo2]", "lt", "[alias2.campo4]"]
        ],
        "AND",
        ["[tabla.campo3]", "eq", "string"],
        "AND",
        ["[tabla.campo4]", "like", "don't"]
    ],
    "group" : ["tabla.campo1", "tabla.campo2"],
    "having" : [
        ["[tabla.campo2]", "gt", "[alias2.campo3]"], 
        "OR",
        ["[tabla.campo2]", "lt", "[alias2.campo4]"]
    ],
    "order" : ["tabla.campo1:ASC", "alias2.campo1:DESC"],
    "offset" : 0,
    "limit" : 10
}
```

**create**
```json
{
    "query" : "create",
    "table" : "nombre tabla",
    "type": "",
    "drop" : "true o false. Elimina la tabla si existe",
    "comment" : "comentarios",
    "columns" : [
        {"name":"campo1", "type":"", "length":"", "null":"", "default":"", "index":"", "attrs":[], "comment":"", "autoinc":"", "after":""}
    ]
}

#ejemplo
{
    "query" : "create",
    "table" : "log",
    "drop" : "true",
    "comment" : "Log de operaciones realizadas",
    "columns" : [
        {"name":"id", "type":"INT", "null":"false", "index":"PRIMARY", "autoinc":"true"},
        {"name":"user", "type":"INT", "null":"true", "index":"INDEX", "default":"null", "comment":"id del usuario que ejecutó la acción"},
        {"name":"action", "type":"ENUM", "length":"'insert','delete','update'", "null":"false", "default":"insert", "comment":"acción"},
        {"name":"date", "type":"TIMESTAMP", "null":"false", "default":"CURRENT_TIMESTAMP", "comment":"fecha y hora de la ejecución"}
    ]
}
```

**comment**
```json
{
    "query" : "comment",
    "table" : "nombre_tabla",
    "comment" : "comentario"
}
```

**drop**
```json
{
    "query" : "drop",
    "table" : "nombre_tabla"
}
```

**rename**
```json
{
    "query" : "rename",
    "name" : "viejo nombre",
    "newname": "nuevo nombre"
}
```

**colmodify**
```json
{
    "query" : "alter",
    "table" : "nombre de tabla",
    "column" : {"name":"campo1", "type":"", "length":"", "null":"", "default":"", "index":"", "attrs":[], "comment":"", "autoinc":"", "after":""}
}
```

**coldrop**
```json
{
    "query" : "dropcol",
    "table" : "nombre de tabla",
    "column" : "nombre de columna"
}
```

**colremame**
```json
{
    "query" : "remamecol",
    "table" : "nombre de tabla",
    "column" : {"oldname":"", "name":"campo1", "type":"", "length":"", "null":"", "default":"", "index":"", "attrs":[], "comment":"", "autoinc":"", "after":""}
}
```

**index**
```json
{
    "query" : "index",
    "table" : "nombre de tabla",
    "column" : "nombre de columna",
    "type" : "tipo de indice" 
}
```

## Tipos de datos
### Números
- TINYINT: pequeño, from -128 to 127
- SMALLINT: entero chico, from -32768 to 32767 
- MEDIUMINT: entero mediano, from -8388608 to 8388607
- INT: entero, from -2147483648 to 2147483647 
- BIGINT: entero grande
- DECIMAL: número exacto de coma flotante
- FLOAT: número de coma flotante de presición simple
- DOUBLE: número de coma flotante de presición doble

### Cadenas
- CHAR: longitud fijo, longitud máxima de 255 caracteres
- VARCHAR: longitud variable length, longitud máxima de 255 caracteres
- TINYTEXT: cadena, longitud máxima de 255 caracteres
- TEXT: cadena, longitud máxima de 65.535 caracteres
- MEDIUMTEXT: cadena, longitud máxima de 16.777.215 caracteres
- BIGTEXT: cadena, longitud máxima de 4.294.967.295 caracteres
- JSON: cadena, longitud máxima de 4.294.967.295 caracteres
- TINYBLOB: cadena binaria, longitud máxima de 255 caracteres
- BLOB:  cadena binaria, longitud máxima de 65.535 caracteres
- MEDIUMBLOB:  cadena binaria, longitud máxima de 4.294.967.295 caracteres
- BIGBLOB:  cadena binaria, longitud máxima de 4.294.967.295 caracteres
- ENUM: enumeración

### Fechas
- DATE: fecha YYYY-MM-DD
- TIME: hora HH:MM:SS.ssssss
- DATETIME: fecha y hora juntos, YYYY-MM-DD HH:MM:SS
- TIMESTAMP: fecha y hora con presición de nanosegundos YYYY-MM-DD HH:MM:SS.ffffff
- YEAR: año, 4 dígitos





Cuando sea necesario hacer comparaciones de valor, ya sea en las condiciones **WHERE** o en las **ON**, se deberá encerrar el valor entre paréntesis, utilizando estos como "comillas".  
Ejemplos basados en MySQL

|JSQL|MySQL|
|---|---|
|["tabla.campo", "eq", "tabla2.campo1"]|\`tabla\`.\`campo\` = \`tabla2\`.\`campo1\`|
|["tabla.campo1", "gt", "(18)"]|\`tabla\`.\`campo1\` > '18'|
|["tabla.campo2", "like", "(stri%)"]|\`tabla\`.\`campo2\` LIKE 'stri%'|

___

# Métodos
|Método|Descripción|
|---|---|
|[column](#column)|Transforma la representación **JSQL** de una columna y la retorna en formato **SQL**|
|[conditions](#conditions)|Transforma un array de condiciones JSQL en una sentencia SQL WHERE|
|[decode](#decode)|Transforma una cadena JSON en un Array asociativo|
|[encode](#encode)|Transforma un Array asociativo en una cadena JSON|
|[operator](#operator)|Retorna un operador válido en función su codificación, invocando al método nglCo...|
|[value](#value)|Prepara un valor en formato JSQL para ser usado en una consulta SQL|
|Internos||
|[Condition](#Condition)|Transforma la representación JSQL de una condición y la retorna en formato SQL a...|


## column
> Transforma la representación **JSQL** de una columna y la retorna en formato **SQL**
> Formatos válidos y sus resultados
> - **["TABLE.FIELD", "ALIAS"]** => \`TABLE\`.\`FIELD\` AS 'ALIAS'
> - **["TABLE.FIELD"]** => \`TABLE\`.\`FIELD\`
> - **["FIELD","ALIAS"]** => \`FIELD\` AS 'ALIAS'

**[string]** =  *public* function ( *string* $mField, *string* $sAliasQuote, *string* $sQuote, *string* $sTableColumnGlue );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mField**|mixed||Array o cadena representando el nombre de la columna **JSQL**. Formatos esperados: <ul><li>field</li><li>table.field</li><li>array(field, alias)</li><li>array(table.field, alias)</li></ul>|
|**$sAliasQuote**|string|'|Caracter utilizado para encerrar los alias|
|**$sQuote**|string|`|Caracter utilizado para encerrar los nombres de tablas y columnas|
|**$sTableColumnGlue**|string|.|Caracter utilizado para unir el nombre de la tabla con el de la columna|

&nbsp;
___
&nbsp;

## conditions
> Transforma un array de condiciones **JSQL** en una sentencia **WHERE** o en un grupo de par de valores **SQL SET**
> Formatos válidos y sus resultados
> - **["TABLE1.FIELD1", "OPERATOR", "TABLE2.FIELD2"]** => \`TABLE1\`.\`FIELD1\` = \`TABLE2\`.\`FIELD2\`
> - **["TABLE1.FIELD1", "OPERATOR", "(STRING VALUE)"]** => \`TABLE1\`.\`FIELD1\` LIKE '%foobar%'

**[string]** =  *public* function ( *array* $aSource, *boolean* $bSetMode );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aSource**|array||Array de condiciones en formato **JSQL**|
|**$bSetMode**|boolean|false|Determina si la salida será utilizada en una sentencia INSERT/UPDATE:<ul><li>**true** =  genera una salida para ser utilizada en el SET en una sentencia INSERT o UPDATE</li><li>**false** =  genera una salida para ser utilizada como condición WHERE</li></ul>|

### Ejemplos  
#### Condición MySQL WHERE  
``` php
#entrada
$a = Array(
    Array("tabla.campo1", "eq", "alias2.campo2"),
    "AND",
    Array (
        Array("tabla.campo2", "gt", "alias2.campo3"), 
        "OR",
        Array("tabla.campo2", "lt", "alias2.campo4")
    ),
    "AND",
    Array("tabla.campo3", "eq", "(foobar)");
);

#ejecución
echo $jsql->conditions($a);

#salida
`tabla`.`campo1` = `alias2`.`campo2` AND (
    `tabla`.`campo2` > `alias2`.`campo3` OR 
    `tabla`.`campo2` < `alias2`.`campo4`
) AND `tabla`.`campo3` = 'foobar'
```

#### valores MySQL SET  
```php
#entrada
$a = Array(
    array("tabla.campo1", "eq", "alias2.campo2"),
    array("tabla.campo3", "eq", "(foobar)");
);

#ejecución
echo $jsql->conditions($a, true);

#salida
`tabla`.`campo1` = `alias2`.`campo2`, `tabla`.`campo3` = 'foobar'
```

&nbsp;
___
&nbsp;

## operator
> Retorna un operador válido en función su codificación, invocando al método [strOperator](fn#stroperator)
> - **eq** =  **=** (Equal)
> - **noteq** =  **!=** (Not equal)
> - **lt** =  **<** (Less than)
> - **gt** =  **>** (Greater than)
> - **lteq** =  **<=** (Less than or equal to)
> - **gteq** =  **>=** (Greater than or equal to)
> - **like** =  **LIKE** (Like)
> - **rlike** =  **RLIKE** (Rlike)
> - **and** =  **AND** (And)
> - **or** =  **OR** (Or)
> - **xor** =  **XOR** (Exclusive OR)
> - **in** =  **IN** (Exclusive OR)
> - **notin** =  **NOT** (Exclusive OR)
> - **is** =  **IS** (Is)
> - **isnot** =  **IS NOT** (Is Not)
> 
> Si **$sSign** no se encuentra entre las opciones, se retornará el signo =
> Si **$sSign** no es especificado, se retornará un array asosiativo con todos los operadores  

**[mixed]** =  *public* function ( *string* $sSign );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sSign**|string||Código del signo que se quiere obtener|

&nbsp;
___
&nbsp;

## decode
> Transforma una cadena JSON en un Array asociativo  

**[array]** =  *public* function ( *string* $sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sString**|string||Valor a decodificar|

&nbsp;
___
&nbsp;

## encode
> Transforma un Array asociativo en una cadena JSON  

**[string]** =  *public* function ( *array* $aArray );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aArray**|array||Array a codificar|

&nbsp;
___
&nbsp;

## value
> Prepara un valor en formato **JSQL** para ser usado en una consulta **SQL**  
> El entrecomillado de valores deberá realizarse utilizando paréntesis
``` json
["tabla.campo", "eq", "(foobar)"]
...
["tabla.campo1", "gt", "(18)"]
...
["tabla.campo2", "like", "(%foo%)"]
```

**[string]** =  *public* function ( *string* $sString, *boolean* $bQuoted, *boolean* $bIsSet );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sString**|string||Valor a preparar|
|**$bQuoted**|boolean||Determina si el valor debe retornarse entrecomillado|
|**$bIsSet**|boolean||Determina si el valor debe ser tratado como un SET de datos, por ejemplo, para ser usado con el operador IN|

&nbsp;
___
&nbsp;

# Internos
## Condition
> Transforma la representación **JSQL** de una condición y la retorna en formato **SQL** aplicando los métodos [column](#column), [operator](#operator) y [value](#value)

**[string]** =  *private* function ( *array* $aCondition );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$aCondition**|array||Array con la condición en formato **JSQL**|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2021 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br /> 
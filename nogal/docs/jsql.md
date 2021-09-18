# jsql
JSQL es una sintáxis que busca estandarizar las consultas SQL en un formato JSON. El objeto **jsql** proporciona un conjunto de métodos que posibilita el parseo de dichas cadenas.  
Luego, cada objeto de base de datos deberá contar con un método que interprete esos datos y los traduzca en una sentencia SQL válida para su motor.  
La estructura del objeto es:

``` json
{
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
				["tabla.campo1","eq", "alias2.campo2"],
				"AND",
				[
					["tabla.campo2", "gt", "alias2.campo3"], 
					"OR",
					["tabla.campo2", "lt", "alias2.campo4"]
				],
				"AND",
				["tabla.campo3", "like", "(I'm John)"],
				"AND",
				["tabla.campo4", "eq", "(2)"]
			]
		]
	],
	"where" : [
		["tabla.campo1", "eq", "alias2.campo2"],
		"AND",
		[
			["tabla.campo2", "gt", "alias2.campo3"], 
			"OR",
			["tabla.campo2", "lt", "alias2.campo4"]
		],
		"AND",
		["tabla.campo3", "eq", "(string)"],
		"AND",
		["tabla.campo4", "like", "(don't)"]
	],
	"group" : ["tabla.campo1", "tabla.campo2"],
	"having" : [
		["tabla.campo2", "gt", "alias2.campo3"], 
		"OR",
		["tabla.campo2", "lt", "alias2.campo4"]
	],
	"order" : ["tabla.campo1:ASC", "alias2.campo1:DESC"],
	"offset" : 0,
	"limit" : 10
}
```

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
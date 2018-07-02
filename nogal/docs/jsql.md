# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# jsql
## nglJSQL *extends* nglStd [main] [20160201]
M茅todos auxiliares para el parseo de una cadena JSQL.

#Estructura JSQL
<pre>
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
["tabla.campo3", "eq", "(I'm John)"],
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
</pre>
  
  
&nbsp;

# M茅todos
|M茅todo|Descripci贸n|
|---|---|
|[Condition](#Condition)|Transforma la representaci髇 JSQL de una condici髇 y la retorna en formato SQL aplicando los m閠odos nglJSQL::column, nglJSQL::operator y nglJSQL::...|
|[column](#column)|Transforma la representaci髇 JSQL de una columna y la retorna en formato SQLFormatos v醠idos y sus resultados["TABLE.FIELD", "ALIAS"] => `TABLE`.`FI...|
|[conditions](#conditions)|Transforma un array de condiciones JSQL en una sentencia WHERE o en un grupo de par de valores SQL SETFormatos v醠idos y sus resultados["TABLE1.FIELD...|
|[decode](#decode)|Transforma una cadena JSON en un Array asociativo|
|[encode](#encode)|Transforma un Array asociativo en una cadena JSON|
|[operator](#operator)|Retorna un operador v醠ido en funci髇 su codificaci髇, invocando al m閠odo nglCommon::strOperator:eq: = (Equal)noteq: != (Not equal)lt:  (Greater ...|
|[value](#value)|Prepara un valor en formato JSQL para ser usado en una consulta SQL|

  
&nbsp;


## column
Transforma la representaci贸n **JSQL** de una columna y la retorna en formato **SQL**
Formatos v谩lidos y sus resultados<ul><li>**["TABLE.FIELD", "ALIAS"]** => `TABLE`.`FIELD` AS 'ALIAS'</li><li>**["TABLE.FIELD"]** => `TABLE`.`FIELD`</li><li>**["FIELD","ALIAS"]** => `FIELD` AS 'ALIAS'</li></ul>  

**[string]** =  *public* function ( *string* \$mField, *string* \$sAliasQuote, *string* \$sQuote, *string* \$sTableColumnGlue );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$mField**|string||Cadena que representa la condici贸n en formato **JSQL**|
|**\$sAliasQuote**|string|'|Caracter utilizado para encerrar los alias|
|**\$sQuote**|string|`|Caracter utilizado para encerrar los nombres de tablas y columnas|
|**\$sTableColumnGlue**|string|.|Caracter utilizado para unir el nombre de la tabla con el de la columna|

&nbsp;
___
&nbsp;

## Condition
Transforma la representaci贸n **JSQL** de una condici贸n y la retorna en formato **SQL** aplicando los m茅todos **nglJSQL::column**, **nglJSQL::operator** y **nglJSQL::value**
Formatos v谩lidos y sus resultados<ul><li>**["TABLE1.FIELD1", "OPERATOR", "TABLE2.FIELD2"]** => `TABLE1`.`FIELD1` = `TABLE2`.`FIELD2`</li><li>**["TABLE1.FIELD1", "OPERATOR", "(STRING VALUE)"]** => `TABLE1`.`FIELD1` LIKE '%foobar%'</li></ul>  

**[string]** =  *private* function ( *array* \$aCondition );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$aCondition**|array||Array con la condici贸n en formato **JSQL**|

&nbsp;
___
&nbsp;

## conditions
Transforma un array de condiciones **JSQL** en una sentencia **WHERE** o en un grupo de par de valores **SQL SET**
Formatos v谩lidos y sus resultados<ul><li>**["TABLE1.FIELD1", "OPERATOR", "TABLE2.FIELD2"]** => `TABLE1`.`FIELD1` = `TABLE2`.`FIELD2`</li><li>**["TABLE1.FIELD1", "OPERATOR", "(STRING VALUE)"]** => `TABLE1`.`FIELD1` LIKE '%foobar%'</li></ul>  

**[string]** =  *public* function ( *array* \$aSource, *boolean* \$bSetMode );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$aSource**|array||Array de condiciones en formato **JSQL**|
|**\$bSetMode**|boolean|false|Determina el tipo de salida SQL:<ul><li>**true** =  genera una salida para ser utilizada en el SET en una sentencia INSERT o UPDATE</li><li>**false** =  genera una salida para ser utilizada como condici贸n WHERE</li></ul>|
### Ejemplos  
#### condici贸n MySQL WHERE  
```php
#entrada
$a = Array(
    array("tabla.campo1", "eq", "alias2.campo2"),
    "and",
    Array (
        array("tabla.campo2", "gt", "alias2.campo3"), 
        "or",
        array("tabla.campo2", "lt", "alias2.campo4")
    ),
    "and",
    array("tabla.campo3", "eq", "(foobar)");
);

#ejecuci贸n
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

#ejecuci贸n
echo $jsql->conditions($a, true);

#salida
`tabla`.`campo1` = `alias2`.`campo2`, `tabla`.`campo3` = 'foobar'
```

&nbsp;
___
&nbsp;

## operator
Retorna un operador v谩lido en funci贸n su codificaci贸n, invocando al m茅todo **nglCommon::strOperator**:<ul><li>**eq** =  <em>=</em> (Equal)</li><li>**noteq** =  <em>!=</em> (Not equal)</li><li>**lt** =  <em><</em> (Less than)</li><li>**gt** =  <em>></em> (Greater than)</li><li>**lteq** =  <em><=</em> (Less than or equal to)</li><li>**gteq** =  <em>>=</em> (Greater than or equal to)</li><li>**like** =  <em>LIKE</em> (Like)</li><li>**rlike** =  <em>RLIKE</em> (Rlike)</li><li>**and** =  <em>AND</em> (And)</li><li>**or** =  <em>OR</em> (Or)</li><li>**xor** =  <em>XOR</em> (Exclusive OR)</li><li>**in** =  <em>IN</em> (Exclusive OR)</li><li>**notin** =  <em>NOT</em> (Exclusive OR)</li><li>**is** =  <em>IS</em> (Is)</li><li>**isnot** =  <em>IS NOT</em> (Is Not)</li></ul>Si **\$sSign** no se encuentra entre las opciones, se retornar谩 el signo =
Si **\$sSign** no es especificado, se retornar谩 un array asosiativo con todos los operadores  

**[mixed]** =  *public* function ( *string* \$sSign );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sSign**|string||C贸digo del signo que se quiere obtener|

&nbsp;
___
&nbsp;

## decode
Transforma una cadena JSON en un Array asociativo  

**[array]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sString**|string||Valor a decodificar|

&nbsp;
___
&nbsp;

## encode
Transforma un Array asociativo en una cadena JSON  

**[string]** =  *public* function ( *array* \$aArray );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$aArray**|array||Array a codificar|

&nbsp;
___
&nbsp;

## value
Prepara un valor en formato **JSQL** para ser usado en una consulta **SQL**  

**[string]** =  *public* function ( *string* \$sString, *boolean* \$bQuoted );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sString**|string||Valor a preparar|
|**\$bQuoted**|boolean||Determina si el valor debe retornarse entrecomillado|

&nbsp;
___
&nbsp;

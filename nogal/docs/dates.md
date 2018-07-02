# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# dates
## nglDates *extends* nglStd [main] [20160127]
Fechas.
  
## Variables

  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[format](#format)|Formatea un valor en segundos, según $sFormat|
|[microtimer](#microtimer)|Retorna la cantidad de segundos transcurridos desde $nTimeIni|
|[now](#now)|Retorna un array con los nombres de los días de la semana y meses del año en el array bidimensional basado en los valores de las constantes NGL_DATE...|
|[settings](#settings)|Retorna un array con los nombres de los días de la semana y meses del año en el array bidimensional basado en los valores de las constantes NGL_DATE...|

  
&nbsp;


## settings
Retorna un array con los nombres de los días de la semana y meses del año en el array bidimensional 
basado en los valores de las constantes **NGL_DATE_DAYS** y **NGL_DATE_MONTHS**.
los índices son:<ul><li>**days** =  valores de Domingo a Sábado</li><li>**days_short** =  Dom a Sáb</li><li>**months** =  valores de Enero a Diciembre</li><li>**months_short** =  valores de Ene a Dic</li></ul>todos sub-arrays comienzan en el índice 1  

**[array]** =  *public* function ( );
  
### Ejemplos  
#### ejemplo  
```php
$dt = $ngl("dates");
echo $dt->settings()["days"][2]; // retornará Lunes
echo $dt->settings()["months_short"][8]; // retornará Ago
```

&nbsp;
___
&nbsp;

## now
Retorna un array con los nombres de los días de la semana y meses del año en el array bidimensional 
basado en los valores de las constantes **NGL_DATE_DAYS** y **NGL_DATE_MONTHS**.
los índices son:<ul><li>**timestamp** =  candidad de segundos desde el 1/1/1970</li><li>**year** =  año en 4 dígitos</li><li>**month** =  mes en 2 dígitos</li><li>**day** =  día en 2 dígitos</li><li>**date** =  fecha en formato Y-m-d</li><li>**time** =  fecha en formato H:i:s</li><li>**hour** =  fecha en formato H:i</li><li>**datetime** =  fecha en formato Y-m-d H:i:s</li><li>**days** =  valores de Domingo a Sábado</li><li>**days_short** =  Dom a Sáb</li><li>**months** =  valores de Enero a Diciembre</li><li>**months_short** =  valores de Ene a Dic</li></ul>todos sub-arrays comienzan en el índice 1  

**[array]** =  *public* function ( );
  
### Ejemplos  
#### ejemplo  
```php
$dt = $ngl("dates");
echo $dt->settings()["days"][2]; // retornará Lunes
echo $dt->settings()["months_short"][8]; // retornará Ago
```

&nbsp;
___
&nbsp;

## format
Formatea un valor en segundos, según **\$sFormat**  

**[array o string]** =  *public* function ( *int* \$nTime, *string* \$sFormat );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nTime**|int||Cantidad de segundos|
|**\$sFormat**|string|1|Modo de salida<ul><li>**1** =  array de datos con los indices<ul><li>year</li><li>month</li><li>day</li><li>hour</li><li>minute</li><li>second</li></ul></li><li>**2** =  cadena textual: 1 year 4 months 1 day 3 hours 45 minutes 10 seconds</li><li>**string** =  cadena con el formato de salida según los estandares del método **date** de PHP</li>|

&nbsp;
___
&nbsp;

## microtimer
Retorna la cantidad de segundos transcurridos desde **\$nTimeIni**  

**[float]** =  *public* function ( *float* \$nTimeIni );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nTimeIni**|float|nogal->startime()|Indice de tiempo en sedundos/microsegundos.|

&nbsp;
___
&nbsp;

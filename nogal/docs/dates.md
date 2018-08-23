# nogal::dates
Utilidades para operaciones con fechas y horas<br />
Generación de Calendarios
  
## Variables
`private` $aSettings = Array bidimensional con los nombres de los días de la semana y meses del año

&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[calendar](#calendar)|Genera un array bidimensional con los datos necesarios para imprimir un calendario|
|[daysdiff](#daysdiff)|Retorna la cantidad de días transcurridos entre 2 fechas|
|[elapsed](#elapsed)|Retorna el tiempo transcurrido (en formato literal) desde una fecha, entre dos fechas ó de una **x** cantidad de una medida de tiempo|
|[info](#info)|Retorna un array con la información de una fecha determinada|
|[microtimer](#microtimer)|Retorna la cantidad de segundos transcurridos desde **$nTimeIni**|
|[monthsdiff](#monthsdiff)|Retorna la cantidad de meses transcurridos entre 2 fechas|
|[settings](#settings)|Retorna un array bidimensional con los nombres de los días de la semana y meses del año|
|[timesdiff](#timesdiff)|Retorna la diferencia, en segundos, entre 2 horas|
|Privados||
|[CalendarMonth](#calendarmonth)|Genera el array bidimensional retornado por el método **calendar**|
 
&nbsp;

## calendar
> Genera un array bidimensional con los datos necesarios para imprimir un calendario, donde elemento principal del Array corresponde a una semana del mes y cada sub-elemento a un día en particular.
> Cada sub-elemento tiene almacenada toda la información retornada por el método [info](#info) para esa fecha, más un indice llamado **day_of_month** util para los casos en que el calendario se pida *completo*, que indica si el día corresponde al mes solicitado, al anterior ó al siguiente.


**[array]** =  *public* function ( *mixed* $mDate, *boolean* $bComplete );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mDate**|mixed|now|Fecha en formato timestamp o una cadena que pueda ser decodificada por [strtotime](http://php.net/strtotime)|
|**$bComplete**|boolean|false|Determina si el calendario debe calcularse ó no, con los meses adyacentes|

### Ejemplos  
#### Calendario Simple del mes en curso 
```php
$calendar = $ngl("dates")->calendar();
print_r($calendar);
```

#### Calendario Completo del mes que viene
```php
$calendar = $ngl("dates")->calendar("+1 month", true);
print_r($calendar);
```
&nbsp;
___
&nbsp;

## daysdiff
> Retorna la cantidad de días transcurridos entre 2 fechas, expresadas en notación [strtotime](http://php.net/strtotime)

**[int]** =  *public* function ( *string* $sDate1, *string* $sDate2 );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sDate1**|string||Fecha incial.|
|**$sDate2**|string|now|Fecha final.|

&nbsp;
___
&nbsp;

## elapsed
> Retorna el tiempo transcurrido (en formato literal) desde una fecha, entre dos fechas ó de una **x** cantidad de una medida de tiempo, por ejemplo: a cuanto tiempo equivalen 12456 horas 

**[array o string]** =  *public* function ( *mixed* $mTime, *string* $sFrom, *boolean* $bReturnString );

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mTime**|mixed||Número entero ó fecha en notación [strtotime](http://php.net/strtotime)|
|**$sFrom**|string|second|<ul><li>**fecha** fecha en notación **strtotime**, unicamente cuando *$mTime* también lo sea</li><li>year</li><li>month</li><li>day</li><li>hour</li><li>minute:</li><li>second</li></ul>|
|**$bReturnString**|boolean|false|Si es *true* retorna el resultado como una cadena, ej: 1 year 4 months 1 day 3 hours 45 minutes 10 seconds|

### Ejemplos  
#### ejemplo  
```php
echo $ngl("dates")->elapsed("1977-08-15", "now", true); // cuanto tiempo pasó desde el 15 de agosto de 1977
echo $ngl("dates")->elapsed(35840, "day"); // a cuanto tiempo equivalen 35840 días
```

&nbsp;
___
&nbsp;

## info
> Retorna un array con la información de una fecha determinada. Esta fecha puede ser un TIMESTAMP o cualquier formato soportado por [strtotime](http://php.net/strtotime).
> Los datos retornados son:
> - **timestamp:** candidad de segundos desde el 1/1/1970
> - **date:** en formato Y-m-d
> - **datetime:** Y-m-d H:i:s
> - **number:** número del día
> - **month:** mes en 2 dígitos
> - **year:** año en 4 dígitos
> - **week:** número de semana del año
> - **day_week:** número de día en la semana (0-6)
> - **single_month:** mes en 1 ó 2 dígitos
> - **single_year:** año en 2 dígitos
> - **day_name:** nombre del día basado en **settings**
> - **day_shortname:** nombre del día (tres letras) basado en **settings**
> - **month_name:** nombre del mes basado en **settings**
> - **month_shortname:** nombre corto del mes (tres letras) basado en **settings**
> - **ampm:** sigla AM ó PM
> - **hour_12:** hora en formato AM/PM
> - **hour:** hora en formato 24hs
> - **time:** hora completa en formato H:i:s
> - **minutes:** cantidad de minutos
> - **seconds:** cantidad de minutos

**[array]** =  *public* function ( *mixed* $mTime );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$mTime**|mixed|now|Fecha en formato timestamp o una cadena que pueda ser decodificada por [strtotime](http://php.net/strtotime)|
  
### Ejemplos  
#### ejemplo  
```php
$now = $ngl("dates")->info();
print_r($now);

# salida
Array (
    [timestamp] => 1534359486
    [date] => 2018-08-15
    [datetime] => 2018-08-15 15:58:06
    [number] => 15
    [day] => 15
    [month] => 08
    [year] => 2018
    [week] => 33
    [day_week] => 3
    [single_month] => 8
    [single_year] => 18
    [day_name] => Miércoles
    [day_shortname] => Mié
    [month_name] => Agosto
    [month_shortname] => Ago
    [ampm] => PM
    [hour_12] => 03
    [hour] => 15
    [time] => 15:58:06
    [minutes] => 06
);
```

&nbsp;
___
&nbsp;

## microtimer
> Retorna la cantidad de segundos transcurridos desde **$nTimeIni**  

**[float]** =  *public* function ( *float* $nTimeIni );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$nTimeIni**|float|nogal->startime()|Indice de tiempo en sedundos/microsegundos.|

&nbsp;
___
&nbsp;

## monthsdiff
> Retorna la cantidad de meses transcurridos entre 2 fechas, expresadas en notación [strtotime](http://php.net/strtotime)

**[int]** =  *public* function ( *string* $sDate1, *string* $sDate2 );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sDate1**|string||Fecha incial.|
|**$sDate2**|string|now|Fecha final.|

### Ejemplos  
#### cuantos meses pasaron desde la crisis del 30 a la fecha
```php
echo $ngl("dates")->monthsdiff("1929-10-29");
```

&nbsp;
___
&nbsp;

## settings
> Retorna un array bidimensional con los nombres de los días de la semana y meses del año 
> basado en los valores de las constantes **NGL_DATE_DAYS** y **NGL_DATE_MONTHS**.
> los índices son:
> - **days** =  valores de Domingo a Sábado
> - **days_short** =  Dom a Sáb
> - **months** =  valores de Enero a Diciembre
> - **months_short** =  valores de Ene a Dic
> todos sub-arrays comienzan en el índice 1  

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

## timesdiff
> Retorna la diferencia, en segundos, entre 2 horas. Cuando la segunda hora sea menor a la primera, el método asume que se trata de 2 días diferentes.

**[float]** =  *public* function ( *float* $nTimeIni );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sTime1**|string||Hora en notación [strtotime](http://php.net/manual/es/datetime.formats.time.php)|

&nbsp;
___
&nbsp;

# Privados
## CalendarMonth
> Genera el array bidimensional retornado por el método [calendar](#calendar)

**[float]** =  *public* CalendarMonth ( *int* $nYear, *int* $nMonth, *boolean* $bComplete );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$nYear**|int||Año del calendario a generar.|
|**$nMonth**|int||Mes del calendario a generar.|
|**$bComplete**|boolean||Determina si el calendario debe calcularse ó no, con los meses adyacentes|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
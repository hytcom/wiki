# Clase 2 - Condicional IF
- [Concepto general](#concepto-general)
- [Comando IFCASE](#Comando-IFCASE)
- [Anidamiento](#anidamiento)
- [Modo Swtich](#modo-swtich)
&nbsp;
&nbsp;

## Concepto general
El condicional **if**, es una estructura de control que nos permite determinar que acciones tomar dada o no cierta condición.
Su funcionamiento es simple, se evalúa una condición, si **(if)** es verdadera ejecuta un grupo de sentencias, sino **(else)** otra o ninguna.

El siguiente ejemplo es sólo a modo explicativo

``` php
*** index.php ***

<?php

if($edad >= 18) {
    echo "eres mayor de edad";
} else {
    echo "eres menor de edad";
}

?>
```

**If** siempre evalúa la condición como verdadera, aún cuando esperemos que sea falsa.
Esto puede entenderse como que "es **VERDADERO** que sea **FALSO**" o "es **FALSO** que sea **FALSO**", es decir que es **VERDADERO**

Existe una variante de **if** que permite decidir entre mas de 2 opciones, y es **else if**. Su sintáxis es similar a la tradicional, sólo que se evalúan mas condiciones antes llegar al **else**

``` php
*** index.php ***

<?php

if($edad >= 18) {
    echo "eres mayor de edad";
} else if($edad >= 6) {
    echo "eres menor de edad";
} else {
    echo "eres un infante";
}

?>
```
&nbsp;
&nbsp;

## Comando IFCASE
If es la estructura de programación mas utilizada, sin **IF** no podríamos programar! es por eso que el primer comando que aprenderemos será **rind:ifcase**, que es la manera de declarar un **IF** en **Rind**.

El comando cuenta con varios parámetros, pero para empezar nos concentraremos en los básicos. Para ver el comando en profundidad consultar la [ayuda](ifcase.md)

- **<@iff>** en este parámetro ubicaremos la condición a evaluar
- **<@then>** acciones que deben ejecutar en caso de una condición VERDADERA
- **<@else>** acciones que se deben ejecutar en caso de FALSA

``` html
*** index.html ***

<rind:ifcase>
    <@iff>{$genero}=="M"</@iff>
    <@then>Eres un hombre</@then>
    <@else>Eres una mujer</@else>
</rind:ifcase> 
```

En el caso de querer evaluar mas de una condición de manera simultanea, se deben utilizar los mismos [operadores lógicos](http://php.net/language.operators.logical) que en PHP

``` html
*** index.html ***

<rind:ifcase>
    <@iff>{$genero}=="M" && {$edad}<18</@iff>
    <@then>Eres un hombre menor de edad</@then>
    <@else>No eres un hombre menor de edad</@else>
</rind:ifcase> 
```
&nbsp;
&nbsp;

## Anidamiento
Muchas veces necesitamos evaluar una condición que depende de un resultado anterior, y para ello solo debemos anidar los **if**, es decir, meter uno dentro de otro.
La única recomendación es ser cuidadosos con los cierres y aperturas de las condiciones.

``` html
*** index.html ***

<rind:ifcase>
    <@iff>{$genero}=="M"</@iff>
    <@then>
        <rind:ifcase>
            <@iff>{$edad}<18</@iff>
            <@then>Eres un hombre menor de edad</@then>
            <@else>Eres un hombre mayor de edad</@else>
        </rind:ifcase> 
    </@then>
    <@else>
        <rind:ifcase>
            <@iff>{$edad}<18</@iff>
            <@then>Eres una mujer menor de edad</@then>
            <@else>Eres una mujer mayor de edad</@else>
        </rind:ifcase> 
    </@else>
</rind:ifcase> 
```
&nbsp;
&nbsp;

## Modo Swtich
Como mensionamos al comienzo de esta clase, la estructura **if** cuenta con la variante **else if**; en **Rind** esta funcionalidad se logra colocando un identificador alfanumérico a los componentes **<@iff>** y **<@then>** separado por dos puntos **(:)**

- <@iff:identificador>...</@iff:identificador>
- <@then:identificador>...</@then:identificador>


Para un mejor mantenimiento del código fuente, recomendamos utilizar un identificador que describa el caso que se está evaluando.

``` html
*** index.html ***

<rind:ifcase>
    <@iff:hombremenor>{$genero}=="M" && {$edad}<18</@iff:hombremenor>
    <@then:hombremenor>Eres un hombre menor de edad</@then:hombremenor>

    <@iff:hombremayor>{$genero}=="M" && {$edad}>=18</@iff:hombremayor>
    <@then:hombremayor>Eres un hombre mayor de edad</@then:hombremayor>

    <@iff:mujermenor>{$genero}=="F" && {$edad}<18</@iff:mujermenor>
    <@then:mujermenor>Eres una mujer menor de edad</@then:mujermenor>

    <@else>Eres una mujer mayor de edad</@else>
</rind:ifcase> 
```

Existen otra variantes de **ifcase**, como por ejemplo chequear si una variable existe, o buscar si un valor está en una lista.

Para ver la descripción completa del comando ver la [ayuda](ifcase.md)
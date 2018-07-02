# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# crypt
## nglCrypt *extends* nglStd [instanciable] [20160127]
Implementa la clase 'phpseclib', de algoritmos de encriptaci贸n
  
## Variables
`public` $crypter = Objeto phpseclib  
`private` $sCrypter = Algoritmo de encriptaci贸n  
`private` $vAlgorithms = 
			Metodos de encriptaci贸n soportados
			<ul>
				<li>aes</li>
				<li>blowfish</li>
				<li>des</li>
				<li>tripledes</li>
				<li>rc2</li>
				<li>rc4</li>
				<li>rijndael</li>
				<li>rsa</li>
				<li>twofish</li>
			</ul>
		  

## Argumentos
|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**text**|mixed||Cadena a encriptar/desencriptar|
|**type**|string|aes|M茅todo de encriptaci贸n|
|**key**|mixed|null|Clave de encriptaci贸n|
|**keyslen**|int|512|Longuitud de las claves. Minima admitida: 32|

  
&nbsp;

# M茅todos
|M茅todo|Descripci贸n|
|---|---|
|[SetKey](#SetKey)|Aplica la clave de encriptaci髇/desencriptaci髇 en el objeto principal, a traves del m閠odo setKey|
|[decrypt](#decrypt)|Desencripta una cadena con el m閠odo seleccionado|
|[encrypt](#encrypt)|Encripta una cadena con el m閠odo seleccionado|
|[keys](#keys)|Genera un array con el par de claves p鷅lica y privada cuando el modo de encriptaci髇 es RSA|
|[type](#type)|Establece el m閠odo de encriptaci髇. M閠odos soportados:aesblowfishdestripledesrc2rc4rijndaelrsatwofish|

  
&nbsp;


## type
Establece el m茅todo de encriptaci贸n. M茅todos soportados:<ul><li>aes</li><li>blowfish</li><li>des</li><li>tripledes</li><li>rc2</li><li>rc4</li><li>rijndael</li><li>rsa</li><li>twofish</li></ul>  

**[$this]** =  *public* function ( *string* \$sCrypter );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sCrypter**|string|aes|M茅todo de encriptaci贸n|

&nbsp;
___
&nbsp;

## keys
Genera un array con el par de claves p煤blica y privada cuando el modo de encriptaci贸n es RSA  

**[array]** =  *public* function ( *int* \$nBits );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$nBits**|int|512|Longuitud de las claves. Minima admitida: 32|
### Ejemplos  
#### Claves  
```php
$keys = $ngl("crypt")->type("rsa")->keys(32);

#salida
Array (
    [private] =>
        -----BEGIN RSA PRIVATE KEY-----
        MC4CAQACBQDt4DWxAgMBAAECBAEQtUcCAwD4dwIDAPUXAgMA9DcCAwDQ3wIDAKeo
        -----END RSA PRIVATE KEY-----

    [public] =>
        -----BEGIN PUBLIC KEY-----
        MCAwDQYJKoZIhvcNAQEBBQADDwAwDAIFAO3gNbECAwEAAQ==
        -----END PUBLIC KEY-----
)
```

&nbsp;
___
&nbsp;

## encrypt
Encripta una cadena con el m茅todo seleccionado  

**[string]** =  *public* function ( *mixed* \$sString );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sString**|mixed||Cadena a encriptar/desencriptar|
### Ejemplos  
#### AES  
```php
echo $ngl("crypt.")
    ->key("asd123")
    ->text("hola mundo!")
    ->encrypt()
;
```
#### RSA  
```php
$cr = $ngl("crypt.");
$keys = $cr->type("rsa")->keys();
$cr->key($keys["private"])->encrypt("hola mundo!");
```

&nbsp;
___
&nbsp;

## decrypt
Desencripta una cadena con el m茅todo seleccionado  

**[string]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sString**|string||Cadena a desencriptar|

&nbsp;
___
&nbsp;

## SetKey
Aplica la clave de encriptaci贸n/desencriptaci贸n en el objeto principal, a traves del m茅todo setKey  

**[boolean]** =  *protected* function ( *string* \$sKey );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sKey**|string|null|Clave de encriptaci贸n/desencriptaci贸n|

&nbsp;
___
&nbsp;

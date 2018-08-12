# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  
# crypt
## nglCrypt *extends* nglStd [instanciable] [2018-08-12]
Implementa la clase 'phpseclib', de algoritmos de encriptación
  
## Variables
`public` $crypter = Objeto phpseclib  
`private` $sCrypter = Algoritmo de encriptación  
`private` $vAlgorithms = 
			Metodos de encriptación soportados
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
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**key**|mixed|null|Clave de encriptación|
|**keyslen**|int|512|Longuitud de las claves. Minima admitida: 256|
|**text**|mixed||Cadena a encriptar/desencriptar|
|**type**|string|aes|Método de encriptación:<ul><li>aes</li><li>blowfish</li><li>des</li><li>tripledes</li><li>rc2</li><li>rc4</li><li>rijndael</li><li>rsa</li><li>twofish</li></ul>|
  
&nbsp;

# Métodos
|Método|Descripción|
|---|---|
|[decrypt](#decrypt)|Desencripta una cadena con el método seleccionado|
|[encrypt](#encrypt)|Encripta una cadena con el método seleccionado|
|[keys](#keys)|Genera un array con el par de claves pública y privada cuando el modo de encriptación es RSA|
|[RSAMode](#rsamode)|Prepara el objeto para utilizar el método de encriptación RSA|

## Privados
|Método|Descripción|
|---|---|
|[SetKey](#setkey)|Aplica la clave de encriptación/desencriptación al invocar al argumento **key**|
|[SetType](#settype)|Establece el método de encriptación al invocar al argumento **type**|

&nbsp;

## decrypt
Desencripta una cadena con el método seleccionado  

**[string]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|string||Cadena a desencriptar|

&nbsp;
___
&nbsp;

## encrypt
Encripta una cadena con el método seleccionado  

**[string]** =  *public* function ( *mixed* \$sString );  

|Argumento|Tipo|Default|Descripción|
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

## keys
Genera un array con el par de claves pública y privada cuando el modo de encriptación es RSA  

**[array]** =  *public* function ( *int* \$nBits );  

|Argumento|Tipo|Default|Descripción|
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

## RSAMode
Prepara el objeto para utilizar el método de encriptación RSA. Chequeando y parseando la clave pública/privada

**[boolean]** =  *private* function ();  
&nbsp;
___
&nbsp;

## SetKey
Aplica la clave de encriptación/desencriptación en el objeto principal, a traves del método setKey del mismo

**[boolean]** =  *private* function ( *string* \$sKey );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sKey**|string|null|Clave de encriptación/desencriptación|

&nbsp;
___
&nbsp;

## SetType
Establece el método de encriptación al invocar al argumento **type**.
Métodos Soportados:
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

**[$this]** =  *private* function ( *string* \$sCrypter );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCrypter**|string|aes|Método de encriptación|
# crypt
Implementa la clase 'phpseclib', de algoritmos de encriptación, con soporte para: 
- AES
- BLOWFISH
- DES
- TRIPLEDES
- RC2
- RC4
- RIJNDAEL
- RSA
- TWOFISH
___

## Variables
|Argumento|Tipo|Descripción|
|---|---|---|
|**$crypter**|public|Objeto phpseclib|
|**$sCrypter**|private|Algoritmo de encriptación|
|**$vAlgorithms**|private|Metodos de encriptación soportados:  <ul><li>aes</li><li>blowfish</li><li>des</li><li>tripledes</li><li>rc2</li><li>rc4</li><li>rijndael</li><li>rsa</li><li>twofish</li></ul>|

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
|**Internos**||
|[RSAMode](#rsamode)|Prepara el objeto para utilizar el método de encriptación RSA|
|[SetKey](#setkey)|Aplica la clave de encriptación/desencriptación al invocar al argumento **key**|
|[SetType](#settype)|Establece el método de encriptación al invocar al argumento **type**|

&nbsp;

## decrypt
> Desencripta una cadena con el método seleccionado  

**[string]** =  *public* function ( *string* $sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sString**|string|*arg::text*|Cadena a desencriptar|

&nbsp;
___
&nbsp;

## encrypt
> Encripta una cadena con el método seleccionado  

**[string]** =  *public* function ( *mixed* $sString );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sString**|mixed|*arg::text*|Cadena a encriptar/desencriptar|
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
> Genera un array con el par de claves pública y privada cuando el modo de encriptación es RSA  

**[array]** =  *public* function ( *int* $nBits );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$nBits**|int|*arg::keyslen*|Longuitud de las claves. Minima admitida: 256|
### Ejemplos  
#### Claves  
```php
$keys = $ngl("crypt")->type("rsa")->keys(256);

#salida
Array (
	[private] =>
		-----BEGIN RSA PRIVATE KEY-----
		MIGoAgEAAiEAuVxGKBvOd7UqRRTp7k9WSbsIKhZPHW+yufySCWdePJ8CAwEAAQIf
		aq1bCGT4bpcqZ0JMtNpJeIAUNToWFU/MNp0z3bn6sQIRANyc1D3hGqnqk/wCBCeH
		JxUCEQDXF9xDpacmIisUHIHlwYHjAhBV625tuyHbU1TXLSHZEzYRAhAhi6IZlsM7
		ykZnq46Cs6w7AhAUaAwFtzctOcvARCt/GWGb
		-----END RSA PRIVATE KEY-----

	[public] =>
		-----BEGIN PUBLIC KEY-----
		MDwwDQYJKoZIhvcNAQEBBQADKwAwKAIhALlcRigbzne1KkUU6e5PVkm7CCoWTx1v
		srn8kglnXjyfAgMBAAE=
		-----END PUBLIC KEY-----
)
```

&nbsp;
___
&nbsp;

# Internos
## RSAMode
> Prepara el objeto para utilizar el método de encriptación RSA. Chequeando y parseando la clave pública/privada

**[boolean]** =  *private* function ();  
&nbsp;
___
&nbsp;

## SetKey
> Aplica la clave de encriptación/desencriptación en el objeto principal, a traves del método setKey del mismo

**[boolean]** =  *protected* function ( *string* $sKey );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sKey**|string|*arg::key*|Clave de encriptación/desencriptación|

&nbsp;
___
&nbsp;

## SetType
> Establece el método de encriptación al invocar al argumento **type**.
> Métodos Soportados:
> <ul>
> 	<li>aes</li>
> 	<li>blowfish</li>
> 	<li>des</li>
> 	<li>tripledes</li>
> 	<li>rc2</li>
> 	<li>rc4</li>
> 	<li>rijndael</li>
> 	<li>rsa</li>
> 	<li>twofish</li>
> </ul>  

**[$this]** =  *protected* function ( *string* $sCrypter );  

|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**$sCrypter**|string|*arg::type*|Método de encriptación|

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2019 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
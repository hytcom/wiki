# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# crypt
## nglCrypt *extends* nglStd [instanciable] [20160127]
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
|**text**|mixed||Cadena a encriptar/desencriptar|
|**type**|string|aes|Método de encriptación|
|**key**|mixed|null|Clave de encriptación|
|**keyslen**|int|512|Longuitud de las claves. Minima admitida: 32|

  
&nbsp;

# Métodos
- [SetKey](#SetKey)
- [decrypt](#decrypt)
- [encrypt](#encrypt)
- [keys](#keys)
- [type](#type)

  
&nbsp;


## type
Establece el método de encriptación. Métodos soportados:<ul><li>aes</li><li>blowfish</li><li>des</li><li>tripledes</li><li>rc2</li><li>rc4</li><li>rijndael</li><li>rsa</li><li>twofish</li></ul>  

**[$this]** =  *public* function ( *string* \$sCrypter );  
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sCrypter**|string|aes|Método de encriptación|
&nbsp;
___
&nbsp;

## keys
Genera un array con el par de claves pública y privada cuando el modo de encriptación es RSA  

**[array]** =  *public* function ( *int* \$nBits );  
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$nBits**|int|512|Longuitud de las claves. Minima admitida: 32|
&nbsp;
___
&nbsp;

## encrypt
Encripta una cadena con el método seleccionado  

**[string]** =  *public* function ( *mixed* \$sString );  
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sString**|mixed||Cadena a encriptar/desencriptar|
&nbsp;
___
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

## SetKey
Aplica la clave de encriptación/desencriptación en el objeto principal, a traves del método setKey  

**[boolean]** =  *protected* function ( *string* \$sKey );  
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**\$sKey**|string|null|Clave de encriptación/desencriptación|
&nbsp;
___
&nbsp;

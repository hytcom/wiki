# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# mail
## nglMail *extends* nglStd [instanciable] [20160201]
Este objeto permite gestionar operaciones sobre los servidores de correo. Recibiendo y enviando mensajes.
Entre las funciones del objecto se encuentran:<ul><li>Revisar correos IMAP</li><li>Revisar correos POP3</li><li>Enviar correos via SMTP</li></ul>
  
## Variables
`private` $sBoundary = Separador de contenidos  
`private` $aAttachments = Array con los archivos adjuntos del correo a enviar  
`private` $socket = Puntero de la conexión con el servidor  
`private` $sCRLF = Caracteres del salto de línea  
`private` $nCRLF = Longuitud del salto de línea  
`private` $sTag = Tag antivo en las conexiones IMAP  
`private` $sServerType = Tipo de Servidor: smtp | imap | pop3  
`private` $sSMTPUsername = Nombre de usuario SMTP  
`private` $sSMTPPassword = Contraseña SMTP  

## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**from**|string||Dirección de correo saliente|
|**name**|string|null|Nombre asociado a la dirección de correo saliente|
|**reply**|string|null|Dirección de correo de respuesta|
|**reply_name**|string|null|Nombre asociado a la dirección de correo de respuesta|
|**to**|string|null|Correos de destino, separados cualquier caracter NO válido en una dirección de correo (espacio, coma, punto y coma, etc)|
|**cc**|string|null|Direcciones en copia, separados cualquier caracter NO válido en una dirección de correo (espacio, coma, punto y coma, etc)|
|**bcc**|string|null|Direcciones en copia oculta, separados cualquier caracter NO válido en una dirección de correo (espacio, coma, punto y coma, etc)|
|**subject**|string|null|Asunto del correo a enviar|
|**message**|string|null|Cuerpo del correo|
|**attach**|string|null|Ruta de un archivo adjunto|
|**attach_name**|string|null|Nombre que llevará el archivo adjunto **argument::attach**|
|**notify**|string|null|Dirección del acuse de recibo|
|**priority**|int|null|Prioridad del correo a enviar, de 1 a 5|
|**charset**|string|UTF-8|Codificación de caracteres del correo a enviar|
|**timediff**|string|0 hours|Diferencia horaria entre el cliente y el servidor SMTP|
|**server**|string|smtp|Tipo de servidor al que se realizará la conexión: smtp | imap | pop3|
|**host**|string||Dominio o IP del servidor|
|**secure**|string|null|Protocolo de seguridad del servidor: SSL, TLS o NULL|
|**port**|int||Puerto del servidor|
|**user**|string||Nombre de usuario en el servidor|
|**pass**|string||Contraseña del servidor|
|**timeout**|int|20|Tiempo de espera del servidor|
|**mailbox**|string|INBOX|Nombre de la carpeta activa|
|**mail**|int|null|Id del correo activo|
|**fields**|string|INBOX|Nombres, separados por espacios, de los campos del correo activo|
|**smtp_authtype**|string|null|Método de autenticación: CRAM-MD5 | LOGIN | PLAIN|
|**localhost**|string|localhost|Nombre del servidor local|

## Atributos
|Atributo|Tipo|Descripción|
|---|---|---|
|**mail_headers**|string|Cabeceras del correo a enviar|
|**mail_body**|string|Cuerpo del correo a enviar|
|**mail_from**|string|Dirección del remitente del correo a enviar|
|**mail_name**|string|Nombre del remitente del correo a enviar|
|**mail_reply**|string|Dirección de respuesta del correo a enviar|
|**mail_to**|string|Destinatarios del correo a enviar|
|**mail_cc**|string|Destinatarios de las copias del correo a enviar|
|**mail_bcc**|string|Destinatarios de las copias ocultas del correo a enviar|
|**mail_subject**|string|Asunto del correo a enviar|
|**mail_text**|string|Contenido HTML del correo a enviar|
|**mail_priority**|int|Prioridad del correo a enviar|
|**mail_notify**|int|Dirección de correo para la confirmación de lectura|
|**state**|null||
|**log**|string|Log de la comunicación con el servidor|
|**exists**|null||
|**recent**|null||

  
&nbsp;

# Métodos

  
&nbsp;


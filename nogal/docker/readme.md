# nogal en Windows
## Instalación en Docker con MariaDB Local

- Instalar Docker [https://www.docker.com/get-started](https://www.docker.com/get-started)
- Crear una carpeta y copiar en ella los archivos *dockerfile* y *php.ini*
- Abrir PowerShell o el CMD y situarse en esa carpeta
- Ejecutar el archivo dockerfile tipeando: ```docker build -t php:nogal -f dockerfile .``` (punto incluído)
- Una vez generada la imagen se debe crear el contenedor ejecutando
```bash
docker run -d -it -p 80:80 -v PATH_LOCAL:/var/www --name=NOMBRE_CONTENEDOR php:nogal
```
> donde PATH_LOCAL es la ruta de la carpeta donde se instalará nogal
> y NOMBRE_CONTENEDOR es el nombre que se le dará al proyecto
> por ejemplo:
```
docker run -d -it -p 80:80 -v /c/mydockers/nogaltest:/var/www --name=nogaltest php:nogal
```
- Ya creado el contenedor, verificar su estado ejecutando: ```docker ps -a```
- Si se encuentra detenido ejecutar: ```docker start NOMBRE_CONTENEDOR```
- Instalar MariaDB de manera local [https://mariadb.org/download](https://mariadb.org/download)
- Averiguar la IP local desde los contenedores, para eso ejecutar: ```ipconfig /all``` y buscar el adaptador "Hyper-V Virtual Ethernet Adapter" (el nombre puede variar)
- Copiar en la carpeta **PATH_LOCAL** la distribución de nogal

Si por algún motivo fuese necesario ingresar al contenedor como si estuvieramos por SSH, se debe ejecutar ```docker exec -i -t NOMBRE_CONTENEDOR /bin/bash```

&nbsp;
___
<sub><b>nogal v1.0</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
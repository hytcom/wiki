# Instalación de nogal en Windows
## Docker con MariaDB Local

### Docker
- Instalar Docker [https://www.docker.com/get-started](https://www.docker.com/get-started)
- Configurar Docker para que utilice [Contenedores Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)
- Crear una carpeta y copiar en ella los archivos *dockerfile* y *php.ini*
- Abrir PowerShell o el CMD y situarse en esa carpeta
- Ejecutar el archivo dockerfile
```bash
docker build -t php:nogal -f dockerfile .
```
- Una vez generada la imagen se debe crear el contenedor ejecutando
```bash
docker run -d -it -p 80:80 -v PATH_LOCAL:/var/www --name=NOMBRE_CONTENEDOR php:nogal
```
> donde PATH_LOCAL es la ruta de la carpeta donde se instalará nogal
> y NOMBRE_CONTENEDOR es el nombre que se le dará al proyecto
> por ejemplo:
```bash
docker run -d -it -p 80:80 -v /c/mydockers/nogaltest:/var/www --name=nogaltest php:nogal
```
- Ya creado el contenedor, verificar su estado ejecutando: ```docker ps -a```
- Si se encuentra detenido ejecutar
```bash
docker start NOMBRE_CONTENEDOR
```
- Copiar en la carpeta **PATH_LOCAL** la distribución de nogal
- Abrir en el navegador [http://localhost](http://localhost)

Si por algún motivo fuera necesario ingresar al contenedor como si estuvieramos por SSH, puede hacerse ejecutando
```bash
docker exec -i -t NOMBRE_CONTENEDOR /bin/bash
```

### MariaDB
- Instalar MariaDB de manera local [https://mariadb.org/download](https://mariadb.org/download)
- Para acceder desde los contenedores se debe averiguar la IP de la maquina local desde Docker. Para ello se debe ejecutar: ```ipconfig /all``` y buscar el adaptador "Hyper-V Virtual Ethernet Adapter" (el nombre puede variar)
- En caso de querer modificar la carpeta en donde se guardan las bases de datos seguir estos pasos:
	- reemplazar el archivo my.ini de la instalación, por ejemplo: **c:\Program Files\MariaDB 5.5\data\my.ini** por el archivo **my.ini**
	- en el nuevo archivo **my.ini** configurar el path de la carpeta de bases de datos
	- reinicar el servicio de MariaDB desde el administrador de servicios. Para abrirlo puede ejecutarse ```services.msc``` en PowerShell 

&nbsp;
___
<sub><b>nogal v1.0</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2018 by <a href="http://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
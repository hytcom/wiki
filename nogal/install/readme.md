# Instalación de nogal en Windows
## Utilizando contenedores Docker

### Docker
- Instalar Docker [https://www.docker.com/get-started](https://www.docker.com/get-started)
- Configurar Docker para que utilice [Contenedores Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

### Nogal
- Crear la carpeta local en la que se instalará el framework y copiar en ella los archivos *dockerfile* y *php.ini*
- Abrir PowerShell y situarse en esa carpeta
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
docker exec -it NOMBRE_CONTENEDOR bash
```

En el caso de trabajar en varios proyectos bajo una misma versión de **nogal**, es conveniente compartir la carpeta **nogal**.  
Para ello debemos ubicar esta carpeta en un nivel superior al proyecto y montarla en un directorio diferente dentro del contenedor
> por ejemplo:
```bash
docker run -d -it -p 80:80 -v /c/mydockers/nogal:/var/nogal -v /c/mydockers/proyecto1:/var/www --name=proyecto1 php:nogal
docker run -d -it -p 80:80 -v /c/mydockers/nogal:/var/nogal -v /c/mydockers/proyecto2:/var/www --name=proyecto2 php:nogal
docker run -d -it -p 80:80 -v /c/mydockers/nogal:/var/nogal -v /c/mydockers/proyecto3:/var/www --name=proyecto3 php:nogal
```


### MariaDB
- Crear la carpeta local para el contendor MariaDB y copiar en ella el archivo *my.cnf*
- Crear una subcarpeta **datadir** donde se almacenarán las bases de datos
- Abrir PowerShell y descargar la última versión de MariaDB desde el repositorio oficial
```bash
docker pull mariadb
```
- Crear el contenedor MariaDB ejecutando
```bash
docker run --name NOMBRE_CONTENEDOR -v PATH_LOCAL:/etc/mysql/conf.d -v PATH_LOCAL/datadir:/var/lib/mysql -p 3306:3306 -d mariadb:TAG
```
> donde PATH_LOCAL es la ruta de la carpeta donde se instalará MariaDB
> NOMBRE_CONTENEDOR es el nombre del contenedor
> TAG es la versión de MariaDB que queremos instalar. Ver versiones en [https://hub.docker.com/_/mariadb](https://hub.docker.com/_/mariadb)
> por ejemplo:
```bash
docker run --name mariadb -v /c/mydockers/mariadb:/etc/mysql/conf.d -v /c/mydockers/mariadb/datadir:/var/lib/mysql -p 3306:3306 -d mariadb:10
```
- Ya creado el contenedor, verificar su estado ejecutando: ```docker ps -a```
- Si se encuentra detenido ejecutar
```bash
docker start NOMBRE_CONTENEDOR
```
- Lo siguiente es generar los permisos para poder ingresar como **root** a nuestras base de datos. Para ello debemos:
  - Acceder a la consola del contenedor
  - Ingresar a la base de datos con el usuario **root**
  - Generar los permisos
```bash
docker exec -it NOMBRE_CONTENEDOR bash
```
```bash
# mysql -uroot
```
```bash
MariaDB [(none)]> CREATE USER 'root'@'%' IDENTIFIED BY 'rootoor';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON * . * TO 'root'@'%';
MariaDB [(none)]> FLUSH PRIVILEGES;
MariaDB [(none)]> exit
```
```bash
# exit
```

Para conectarse a MariaDB desde el host, basta con apuntar a **localhost**, puerto 3306 y utilizando el protocolo **TCP**
```bash
mysql -h localhost -P 3306 --protocol=tcp -u root
``` 

Para conocer el host al que deberemos apuntar desde otros contenedores, debemos inspeccionar la red de contenedores y averiguar la IP del contenedor MariaDB, para ello ejecutuamos
```bash
docker network inspect bridge
```
En el valor **Containers** encontraremos los datos de los contenedores que componen la red
```json
"Containers": {
    "ec400c2dba04182f61b35b5781d2e0ade16a643cad40bd4a035653dc9051988c": {
        "Name": "nogaltest",
        "EndpointID": "e7ff2012abb5e86a38932a8c37629c8305b3ed97c5fc5b57df209d59b4dc37b8",
        "MacAddress": "02:42:ac:04:00:42",
        "IPv4Address": "172.17.0.2/16",
        "IPv6Address": ""
    },
    "691ae0817a30c9b4236e12c528e7a0849af7e89c505ad59072e47631923ff864": {
        "Name": "mariadb",
        "EndpointID": "d1dc692034601e511748133a939e8d38ccee88953d598130b7aa980d9150ae35",
        "MacAddress": "11:00:02:ab:10:03",
        "IPv4Address": "172.17.0.3/16",
        "IPv6Address": ""
    }
}
```
&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2019 by <a href="https://hytcom.net/nogal">hytcom.net/nogal</a> - <a href="https://github.com/arielbottero">@arielbottero</a></sup><br />
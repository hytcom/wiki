# Instalación de nogal y mariadb con Docker
## nogal / mariadb

### Docker
- Instalar Docker [https://www.docker.com/get-started](https://www.docker.com/get-started)
- Si tu OS es Windows, debés configurar Docker para que utilice [Contenedores Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

### nogal
- Crear la carpeta local y dentro crar 2 carpetas, que servirán de repositiorio para los archivos del proyecto y las bases de datos, por ejemplo:
```bash
/home/me
/home/me/project
/home/me/mariadb
```
- Copiar en la carpeta principal (/home/me) los archivos **docker-compose.yml** y **.env**
- Editar el archivo **env** colocar tu configuración, por ejemplo:
```bash
# .env
PROJECT_PATH=/home/me/project
MARIADB_PATH=/home/me/mariadb
MARIADB_PASSWORD=mypass
```
- Abrir una terminal, cmd ó PowerShell, situarse en esa carpeta y ejecutar
```bash
docker-compose up -d
```
- LISTO! ya podés usar el **nogal**  

&nbsp;

### Notas
- Si por algún motivo fuera necesario ingresar a los contenedores, podés hacer por medio de la terminal ejecutando
```bash
# nogal
docker exec -it nogal bash

# mariadb
docker exec -it mariadb bash
```
- En la carpeta **/urs/share/nogal/samples** están diponibles ejemplos de los 3 modos de arranque del framework. Para probarlos, se debe copiar el contenido de cada una de ellas en la carpeta **/home/me/project**  

&nbsp;

## IMPORTANTE!
El contenedor **mariadb** es accesible desde el contenedor **nogal** mediante el nombre **mariadb**, es decir, que en la configuración del conector de base de datos, el host debe ser **mariadb**

&nbsp;
___
<sub><b>nogal</b> - <em>the most simple PHP Framework</em></sub><br />
<sup>&copy; 2021 by <a href="https://hytcom.net">hytcom.net</a> - <a href="https://github.com/hytcom">@hytcom</a></sup><br />
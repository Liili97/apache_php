##Crear un servidor apache con Docker
Necesitamos la imagen de Apache con soporte PHP, yo he escogido esta: https://hub.docker.com/_/php/

## Docker-compose

```
version: '3.9'
services:
  asir-apache.elisa:
    image: php:7.4-apache
    container_name: asir_apache.elisa
    ports:
    - '80:80'
    - '8080:8080'
    volumes:
    - ./html:/var/www/html
    - ./confApache:/etc/apache2
```

***version**: La version del la imagen que elegiremos.
	***Nombre del contenedor**: para configurar el servicio, le ponemos un nombre.
	***image**: El nombre que le da hub.docker a la imagen elegida, despues de los `:` la versión.
	***container-name**: El nombre que le queremos dar a nuestro contenedor.
	***ports**:Los puertos a los que queremos mapear nuestro contenedor; `contenedor:host`, lo hacemos para los dos sitios, `sitio1 :80 sitio2:8080`.
	***volumes**: Mapeamos las rutas de configuracion principal para poder tener la misma configuracion aunque la maquina se caiga, o le pase cualquier cosa.

Una vez configurado usamos el comando `docker-compose up`

##Creacion del sitio

En `/var/www/html` creamos nuestro tenemos un `000-default.conf`, que usaremos como zona `sitio1`.
 
```
 <VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html/site1

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

En este fichero tendremos que cambiar:
-La primera linea, el puerto al que queremos acceder. 
-La linea 3, la ruta donde queremos poner el contenido de la pagina. En dicho directorio añadimos un Hola Mundo en HTML5 para realizar la prueba.

Tambien actualizaremos en Visual code, en el apartado `Explorer>Apache>confApache>ports.conf` para que el Docker escuche por el puerto que queramos.

Por último, para pasar el el sitio de enable a avaliable usaremos el comando: `a2ensite`

Solo tendremos que hacer un `docker restart`

Para hacer más sitios, simplemente reharemos el ultimo punto.

Si ponemos en el navegador `localhost:(puerto)` en mi caso: `localhost:80`, saldrá el Hola Mundo de la zona.



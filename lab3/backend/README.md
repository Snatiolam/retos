# Creacion del backend

Para el backend necesitamos editar primero nuestro archivo .env
Debemos editar la linea que URL_DB_CONNECTION la cual es la que nos conectara con la base de datos mongo.

```
URL_DB_CONNECTION = mongodb://<username>:<password>@172.31.0.98:27017/bookstore
```

Donde <username> Es el usuario que creaste en tu base de datos de mongo.
Donde <password> Es la contraseña que el dimos al usuario que creamos para la base de datos.

Instalamos Docker en nuestra maquina con los siguientes comando:

```bash
sudo yum install -y docker
sudo systemctl start docker
sudo systemctl enable docker
```

Ahora creamos un Dockerfile con una imagen base de node:14 la que nos permitira correr el servidor

Copiamos todos los package\*.json a la imagen del contender y corremos npm install para instalar las dependencias

Luego copiamos los demas archivos que son codigo fuente y configuracion a la imagen y corremos node server.js

Procedemos a crear la imagen:

```bash
sudo docker build -t backend_image .
```

puedes poner depues de -t el nombre de la imagen que quieras

Ahora que tenemos una imagen del backend podemos poner a correr nuestro contenedor

```bash
sudo docker run -d -p 5000:5000 --name backend_container backend_image
```

La opcion -d nos permite hacer un detach del container para que no siga corriendo en la terminal

La opcion -p nos permite hacer un bind de puertos, en este caso el puerto 5000 del container (derecha) 
se encuentra vinculado al puerto 5000 de nuestra maquina (izquierda).

La opcion --name nos permite darle un nombre a nuestro contenerdor para poder reconocerlos mas facilmente

Por ultimo ponemos la imagen que usaremos para correr el contenedor

Ahora nuestra aplicacion backend esta corriendo exitosamente en el puerto 5000 de nuestra maquina.

Ahora hay que cambiar el security group de la maquina y abrir el puerto 5000 para que pueda pasar trafico a nuestra aplicación.

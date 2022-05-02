# Creación del frontend

Primero edite el archivo nginx.conf el cual esta en el repositorio.
Agregue la ip privada en la cual esta en backend agregando:

```
upstream backend {
    server <ip>:5000;
}

```

Donde <ip> es la ip privada de su servidor backend. En nuestro caso es 172.31.88.2.

Luego cambie el root donde quiere que su aplicacion este alojada:

```
...

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html/bookstore;

...
```

Procedemos a instalar docker:

```bash
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
```

Ahora podemos instalar docker-compose:

```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-`uname -s`-`uname -m` | sudo tee /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

Ahora que tenemos docker instalado y corriendo podemos crear nuestro Dockerfile para poder armar una imagen que puedan compilar y correr una aplicación en React.

Abre Dockerfile con tu editor de texto favorito (Vim)

Usaremos una imagen base de node:14-alpine y la llamaremos build con la directiva *FROM* de docker

Definimos el directorio en el cual estaremos trabajando. Ej: *WORKDIR /usr/src/app*

Copiamos todos los archivos package\*.json a nuestro "container" con la directiva *COPY* de docker

Despues corremos el comando npm install con la directiva *RUN* de docker

Copiamos de nuevo el resto de archivos que son el codigo fuente y configuraciones a nuestro container

Ahora corremos 'npm run build' para compilar el codigo de React con la directiva *RUN* de docker

```dockerfile
FROM node:14-alpine as build
WORKDIR /usr/src/app
ENV PATH /usr/src/app/node_modules/.bin:$PATH
COPY package*.json ./
Run npm install
COPY . ./
RUN npm run build
```

Ahora que nuestro codigo fue compilado exitosamente podemos utilizar una imagen nginx para correr la aplicacion con el html, css y js generado.
Es importante utilizar la version 1.12.0 de nginx debido a que la configuracion que estamos usando funciona unicamente para esa versión
```dockerfile
FROM nginx:1.12.0-alpine
```


Ahora copiamos la respectiva configuración de nginx que es el archivo nginx.conf en /etc/nginx/nginx.conf que es donde nginx mira su configuracion por defecto en el container.

```dockerfile
COPY ./nginx.conf /etc/nginx/nginx.conf
```

Copiamos todos los archivos que compilamos de la imagen anterior en nuestro directorio de trabajo (/usr/src/app) a /usr/share/nginx/html/bookstore que es donde nuestra configuracion de nginx tiene su root de la aplicacion

```dockerfile
COPY --from=build /usr/src/app/build /usr/share/nginx/html/bookstore
```

Ahora que tenemos nuestro Dockerfile listo nos falta asegurar la aplicación, para esto utilizaremos una imagen de letsencrypt linuxserver/letsencrypt o linuxserver/swag,
ambas son validas pero linuxserver/letsencypt dejo de ser mantenida. Sin embargo sus configuraciones son exactamente iguales.


Primero creamos un docker-compose.yml con nuestro editor de texto favorito y añadimos un servicio que va a ser nuestra aplicacion en React:


```
version: '3'

services:
  react-frontend:
    build:
      context: .
```

Llamamos nuestro servicio react-frontend y le decimos que arme la imagen con el Dockerfile en el directorio actual (.)

Ahora podemos agregar un nuevo servicio que es el de letsencrypt el cual tendra unas variables de entorno necesarios para generar un certificado valido.

```
  letsencrypt:
    image: linuxserver/letsencrypt
    container_name: letsencrypt
    depends_on:
      - react-frontend
    volumes:
      - ./config:/config
      - ./default:/config/nginx/site-confs/default
    environment:
      - EMAIL=salzatec1@eafit.edu.co
      - URL=reto-telekinesis.tk
      - SUBDOMAINS=www
      - VALIDATION=http
      - TZ="America/Bogota"
      - PUID=500
      - PGID=500
    ports:
      - "443:443"
      - "80:80"
```

La URL sera el dominio el cual obtuvimos y en el que estara corriendo nuestra aplicacion, en nuestro caso reto-telekinesis.tk.

Los SUBDOMAINS son los subdominios que tambien certificaremos dentro del dominio

El EMAIL es opcional

TZ es la zona en la que esta corriendo la aplicacion

PUID y PGID son los id del usuario y el grupo con los que trabaja letsencrypt en su imagen

Conectamos los puertos 443 y 80 del container letsencrypt y los "Mapeamos" a los respectivos puertos en nuestra maquina local

El depends_on significa que usara de la imagen react-frontend por lo que usara algunas configuraciones de esta.

Ahora debemos crear un archivo default que es una configuracion de nginx y necesita letsencrypt para recibir las peticiones de los clientes y redirigirlas al container que corre la aplicación.

Cambiamos la linea proxy_pass por el nombre del servicion que necesitaba letsencrypt para redirigir las peticiones.


```
...
 location @app {
        proxy_pass http://react-frontend;
        proxy_set_header Host $host;
...
```

Con todo montado procedemos a subir los servicios con docker-compose:

```bash
sudo docker-compose up -d
```

La opción -d nos permite hacer un detach de la terminal por lo que el proceso correra en segundo plano

No olvides abrir los puertos 443 y 80 de los security groups de la maquina frontend para que se puedan recibir las peticiones.

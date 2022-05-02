# Lab 2

Este reto consiste en desplegar una aplicacion en wordpress, dockerizarla y asegurar la pagina con un certificado ssl.

## Nota

Cree una ip elastica y asignesela a la maquina para que tener que cambiar los registros DNS cada vez que su maquina se reinicie.

1. Actualizamos el sistema e instalamos docker

```bash

sudo yum update -y 
sudo yum install docker -y

```

2. Habilitamos el servicio de docker y lo ponemos a correr


```bash

sudo systemctl start docker
sudo systemctl enable docker
```


3. Instalamos docker-compose

```bash

sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

```

4. Creamos un arhivo docker-compose.yml el cual contiene tres imagenes: wordpress, db y swag (letsencrypt). El container de letsencrypt se encargara de generar
los certificados y hacer la redireccion al container de wordpress.

5. Creamos un archivo llamado default que es un archivo de configuracion de nginx que va a utilizar nuestra imagen de letsencrypt para
redireccionar las peticiones al container de wordpress y responder


6. Corremos docker-compose

```bash

sudo docker-compose up -d

```

## Importante

Abrir los puertos *80* y *443* de la maquina para que la aplicacion pueda recibir las peticiones HTTP/HTTPS

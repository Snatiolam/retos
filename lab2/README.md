# Lab 2

Este reto consiste en desplegar una aplicacion en wordpress, dockerizarla y asegurar la pagina con un certificado ssl.

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

4. Corremos docker-compose

```bash

sudo docker-compose up -d

```

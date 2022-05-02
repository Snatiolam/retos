# Mongo Local Database

Los siguientes pasos son para configurar una base de datos en una maquina con Amazon Linux 2 utilizando mongodb.

1. Abrimos el archivo /etc/yum.repos.d/mongodb-org-4.4.repo con nuestro editor de preferencia (Vim) e ingresamos las siguientes lineas

```
[mongodb-org-4.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2/mongodb-org/4.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.4.asc
```

2. Actualizamos los repositorios de yum e instalamos mongodb

```bash
sudo yum update -y
sudo yum install -y mongodb-org
```

3. Corremos el comando 'mongo' para entrar en modo interactivo y corremos los siguientes comandos:

```
use <database>
db.createUser({user: "<user>", pwd: "<password", roles:[{role: "readWriteAnyDatabase" , db:"admin"}]})
db.books.insert(<data>)
```

Donde <database> es la base de datos que se quiera crear en nuestro caso bookstore.
Donde <user> es el nombre del nuevo usuario y <password> su clave.
Donde <data> es la informacion a insertar en la base de datos.

4. Ahora editamos el archivo /etc/mongod.conf para permitir que otras maquinas se conecten a la base de datos y se autentiquen.

Cambiamos el bindIp 127.0.0.1 por 0.0.0.0

Descomentamos la directiva security y ponemos authorization : "enabled"

```
bindIp 0.0.0.0

...

security:
    authorization: "enabled"
```

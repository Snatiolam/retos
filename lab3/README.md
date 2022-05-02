# Lab 3

Este reto consiste en 3 instancias: una para el frontend, una para el backend y otra para la base de datos o el modelo.
Tanto el frontend como el backend se dockerizan y la pagina se asegurara con un certificado ssl.

Los respectivos comandos para cada maquina se encuentran en los READMES del directorio destinado para cada instancia.
El frontend se comunica con el backend y el backend con la base de datos.

Cree una ip elastica en AWS y asignele esa ip elastica a su frontend, asi cada vez que sus maquina se reinicien no sera necesario
cambiar los registros DNS

## Importante

Abrir los puertos *80* y *443* a todo el publico en la maquina del frontend para recibir las peticiones HTTP/HTTPS.

Abrir el puerto *5000* en la maquina backend para recibir las llamadas de la API

Abrir el puerto *27017* de la maquina de la base de datos mongo para que mongo pueda recibir las peticiones a la base de datos.

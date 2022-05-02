# Retos 2 y 3 Telematica

Las carpetas de los retos son llamadas lab seguido del numero del reto a seguir y tendran un
README donde se explicara la solucion.

El primer reto o lab2 consiste en desplegar una aplicacion en docker y asegurarla con un certificado ssl para el dominio.
Todo esto se logra a travez de docker.

El segundo reto o lab3 consiste en desplegar una aplicacion donde el backend, frontend y el modelo se comuniquen entre si,
dockerizando tanto el frontend como el backend y asegurando la pagina con un certificado ssl. El modelo tambien se desplegara
en la nube en una maquina de aws utilizando mongo como tecnologia.

Los dominios para ambos laboratios fueron obtenidos a traves de la pagina de Freenom.com.

Una vez obtenido los dominios importante crear un registro tipo A (address) que apunte a las direcciones de las ip elasticas correspondientes a las maquinas
que reciben las peticiones HTTP/HTTPS, en el lab3 esta maquina es el frontend.

Los dominios para cada laboratorio son respectivamente:

 ## Laboratorio 2
 
 www.telematica-dns-alzate.tk
 
 ## Laboratorio 3
 
  www.reto-telekinesis.tk

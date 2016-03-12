##Pr�ctica 1: Preparaci�n de las herramientas de la asignatura SWAD
#Autor: Ram�n S�nchez Garc�a

El objetivo de esta primera pr�ctica es la de crear y configurar dos m�quinas virtuales sobre las que
trabajaremos a lo largo del curso. Para ello he usado:

* VMware para las m�quinas virtuales
* Ubuntu Server 12.04

He creado dos m�quinas virtuales, M1SWAP y M2SWAP, en las cuales he instalado tambi�n un LAMP Server
junto a un OpenSSH (que usaremos en la siguiente pr�ctica). 

Tras la instalaci�n y configuraci�n de estos, compruebo que todo funcione correctamente, para ello:

#->M�quina M1SWAP

Lo primero que he hecho ha sido mirar su IP:  
![img](capturas/capturaswap_p1_2.png)

En segundo lugar, ejecutro los comandos que indica el gui�n de pr�cticas, que son:
    apache -v  
    ps aux | grep apache  

![img](capturas/capturaswap_p1_1.png)

De forma optativa, he instalado el paquete cURL con la orden
`sudo apt-get install curl`
para poder ver archivos y bajarlos. En la prueba siguiente hago esto �ltimo, bajar una imagen remota:
![img](capturas/capturaswap_p1_3.png)

Por �ltimo, en el directorio /var/www he creado un html de prueba para poder visualizarlo
en la segunda m�quina con cURL.

#->M�quina M1SWAP

Realizo las mismas acciones que he hecho en la m�quina anterior, hasta instalar el cURL.

Obtengo la IP:

![img](capturas/capturaswap_p1_4.png)

Instalo el paquete cURL y lo pruebo. Para ello voy a usar esta vez el comando
`curl http://192.168.18.128/hola.html`
donde esa es la ip de la primera m�quina, y lo siguiente es el nombre del archivo html que creamos:
![img](capturas/capturaswap_p1_5.png)

Por �ltimo vuelvo a comprobar que el servidor apache est� funcionando en esta m�quina:
![img](capturas/capturaswap_p1_6.png)

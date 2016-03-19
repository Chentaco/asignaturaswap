##Pr�ctica 2: Clonar la informaci�n de un sitio web
#Autor: Ram�n S�nchez Garc�a

Para desarrollar esta pr�ctica partimos de las m�quinas virtuales y todo lo instalado en la pr�ctica 1. La pr�ctica est� realizada por mi y 
documentada en este enlace.

El desarrollo de esta segunda pr�ctica se divide en los siguientes objetivos:  

* Utilizar tar para migrar varios archivos locales a otro equipo remoto  
* Instalar y utilizar rsync para clonar una carpeta entre dos m�quinas  
* Probar y configurar ssh para realizar accesos sin contrase�a  
* Programar una tarea b�sica con crontab, en concreto que realice rsync de la carpeta /var/www/ cada hora  


#Crear un tar con ficheros locales en un equipo remoto  

En esta primera parte hacemos algo tan b�sico como utilizar el comando tar para enviar una carpeta y/o archivos a un equipo remoto. El comando es el siguiente:  

``` 
tar czf - /home/ramon/carpetaMaquina1/ | ssh 192.168.18.129 'cat > ~/tar.tgz'
```
Donde:  
* *carpetaMaquina1* es la carpeta que queremos clonar al equipo remoto
* *192.168.18.129* es la direcci�n IP de la m�quina remota a la que enviamos el archivo

Recordemos que ten�amos dos m�quinas (M1SWAP y M2SWAP), donde M1SWAP ser� la m�quina "servidora" y la otra la cliente. Ambas tienen las IP's acabadas en
128 y 129 respectivamente.   
  
Podemos observar en la siguientes capturas como partiendo de la m�quina servidora hemos enviado el tar correctamente a la m�quina cliente:  

![img](capturas/capturaswap_p2_1.png)  

![img](capturas/capturaswap_p2_1.png) 

#Instalar y utilizar rsync  

Utilizar tar es util para archivos peque�os. Sin embargo, para grandes cantidades de informaci�n viene mejor utilizar otras herramientas, como por ejemplo
*rsync*. Para instalarlo:  
```
sudo apt-get install rsync  
``` 
Ahora, para probar que funciona, voy a copiar una carpeta que he creado llamada *carpetaRsyncMaquina1* en el directorio /var/www/ a esta misma carpeta pero
de la segunda m�quina. Utilizo por tanto el siguiente comando DESDE LA SEGUNDA M�QUINA:  
```
sudo rsync -avz --delete -e ssh root@192.168.18.128:/var/www/ /var/www/  
```
  
Si todo ha ido correcto, despu�s de que nos pida la contrase�a del root de la primera m�quina, deber�a de haberse copiado.  
![img](capturas/capturaswap_p2_3.png) 

![img](capturas/capturaswap_p2_5.png) 

![img](capturas/capturaswap_p2_6.png) 

Cada vez que usemos el comando nos pedir� contrase�a, por lo que ser� algo inc�modo, (y si programamos un cron, no nos dejar� usarlo), por lo que se 
propone configurar el ssh para que no lo haga. 

**NOTA IMPORTANTE**: Las carpetas de /var/www/ de ambos equipos tienen permisos de escritura de root. Vamos a darle permisos para que los usuarios puedan
trabajar en esta carpeta, para ello hago lo siguiente en ambas m�quinas:  

![img](capturas/capturaswap_p2_13.png) 

Donde *ramon* es el nombre de usuario.

#Acceso sin contrase�a para ssh

Lo primero es utilizar el *ssh-keygen* en la m�quina cliente, dependiendo de quien haga el rsync (root o usuario), lo configuraremos para que este pueda
acceder sin contrase�a:  

![img](capturas/capturaswap_p2_7.png) 

![img](capturas/capturaswap_p2_14.png) 

Una vez hecho esto, enviamos la configuraci�n a la m�quina sevidor:   
![img](capturas/capturaswap_p2_8.png) 
Por �ltimo, probamos a loguear a la m�quina host desde la cliente, incluso usar otros comandos:  
![img](capturas/capturaswap_p2_9.png)  
![img](capturas/capturaswap_p2_10.png)  

#Programar tareas con crontab

Lo �ltimo que realizamos en esta pr�ctica 2 es la de programar el crontab de forma que realice una copia o actualizaci�n de la carpeta /var/www/ cada hora.
Lo primero ser� editar el archivo crontab:  
```
sudo nano /etc/crontab  
```
A�adimos una linea con el tiempo que quiero que haga esa acci�n. Como dice cada hora, edito el primer campo "minutos" para que lo haga cada "00" minutos:  
![img](capturas/capturaswap_p2_11.png)  

Si todo ha ido bien, y el ssh sin contrase�a est� bien configurado, deber�a funcionar correctamente. Para probarlo, nosotros hemos editado el crontab para
que lo haga cada minuto. He creado "HOLAESTOFUNCIONA.txt" para ver si despu�s de 1 minuto se copia:  
![img](capturas/capturaswap_p2_15.png)  

Como observamos en la captura, al minuto el archivo se ha copiado correctamente.
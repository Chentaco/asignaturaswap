##Pr�ctica 3: Balanceo de carga
#Autor: Ram�n S�nchez Garc�a

Para desarrollar esta pr�ctica continuamos con el uso de las m�quinas virtuales creadas en la 
[Practica 1](https://github.com/Chentaco/asignaturaswap/blob/master/Practicas/P1/Practica1.md) y con las configuraciones realizadas en la 
[Practica 2](https://github.com/Chentaco/asignaturaswap/blob/master/Practicas/P2/Practica2.md). 

El desarrollo de esta tercera pr�ctica se divide en:  

* Crear y configurar una tercera m�quina virtual adem�s de las que ya tenemos.  
* Instalaci�n, configuraci�n y pruebas del balanceador de carga *Nginx*.  
* Instalaci�n, configuraci�n y pruebas del balanceador de carga *Haproxy*.  
* Optativo: Instalaci�n, configuraci�n y pruebas del balanceador de carga *Pound*.  


#Creaci�n de la Tercera m�quina Virtual  

Procedemos a la instalaci�n de la m�quina virtual de la misma forma que hicimos con la primera pr�ctica, siendo el SO el mismo que usamos (Ubuntu server 12) y 
las caracter�sticas parecidas. Esta m�quina se llama *M3SWAP*.  
  
*OJO*: Para las m�quinas M1SWAP y M2SWAP instalamos los servicios de Apache (LAMP). Para esta tercera m�quina NO los instalaremos, pues bloquearian el puerto 80, 
que es el que utilizaremos para usar los balanceadores de carga.  
  
Una vez que ha terminado la instalaci�n, vemos que direcci�n ip es la que tiene:    
  
![img](capturas/capturaswap_p3_1.PNG)   
  
Y si es accesible entre las m�quinas que ya teniamos y esta con un ping:  
![img](capturas/capturaswap_p2_2.PNG)  
  
Si todo ha ido correcto, procedemos a la instalaci�n y configuraci�n de los balanceadores de carga que vamos a probar.  

#Nginx: Instalaci�n y configuraci�n

Lo primero que hay que realizar es la importaci�n de la clave del repositorio de software para poder instalar nginx, para ello:  
![img](capturas/capturaswap_p3_4.PNG)  
  
Hecho esto, pasamos a a�adir el respositorio al fichero con los siguientes echos:
![img](capturas/capturaswap_p3_5.PNG)  
Como podemos ver, necesito permisos de administrador para poder realizarlo, adem�s que termina con un *apt-get upgrade*.  
Por �ltimo solo queda ejecutar:  
``` 
apt-get install nginx
```
  
Ya lo tenemos instalado. Toca configurarlo. Abrimos el archivo */etc/nginx/conf.d/default.conf*, borramos todo lo que tiene y a�adimos la siguiente configuraci�n:  
```
upstream apaches {
 server 192.168.18.128;
 server 192.168.18.129;
}
server{
 listen 80;
 server_name balanceador;
 access_log /var/log/nginx/balanceador.access.log;
 error_log /var/log/nginx/balanceador.error.log;
 root /var/www/;
 location /
 {
 proxy_pass http://apaches;
 proxy_set_header Host $host;
 proxy_set_header X-Real-IP $remote_addr;
 proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
 proxy_http_version 1.1;
 proxy_set_header Connection "";
 }
}
```  
Donde las dos primeras direcciones ip son las de nuestras m�quinas M1SWAP y M2SWAP, respectivamente.  
Ejecutamos el siguiente comando para reiniciar el servicio y ya podemos empezar a realizar las pruebas:  
```
service nginx restart
```  
  
**NOTA IMPORTANTE 1:** Para realizar las pruebas y ver que cada vez se llama a un servidor distinto, modificar los index.html de las dos m�quinas para que sean diferentes 
y/o indiquen en qu� m�quina estamos.  
**NOTA IMPORTANTE 2:** Como estamos usando las m�quinas de la pr�ctica 2, es recomendable evitar que se haga el rsync que teniamos programado en el crontab. Para ello comentamos 
la linea:  
![img](capturas/capturaswap_p3_8.PNG)  
  
Realizamos un curl 192.168.18.130, es decir, a la m�quina que est� haciendo de balanceador de carga, y si todo va bien, veremos como cada vez que lo hagamos cambia de m�quina:  
![img](capturas/capturaswap_p3_9.PNG)  
Podemos probar tambi�n desde la m�quina real a acceder desde el navegador, y que al actualizar la p�gina, esta cambie de servidor:  
![img](capturas/capturaswap_p3_10.PNG)  
![img](capturas/capturaswap_p3_11.PNG)  
  
La pr�ctica tambi�n dice que supongamos que la primera m�quina, la M1SWAP, tiene el doble de potencia que la segunda, por lo que tendr� m�s "peso" con el que cargar. Para 
ello a�adimos en el archivo de configuraci�n lo siguiente:  
![img](capturas/capturaswap_p3_12.PNG)  
  
Ahora, cuando hagamos peticiones, se har�n el doble de peticiones a la m�quina 1 que a la m�quina 2:  
![img](capturas/capturaswap_p3_13.PNG)  
  
Se pueden realizar muchas m�s configuraciones, pero las b�sicas y que nos pide la pr�ctica son las ya indicadas.  
Acabado esto, toca parar el servicio (pues usa el mismo puerto que hproxy) y empezar a trabajar con el siguiente balanceador de carga. 
Para ello, hay dos formas de detener el proceso:
* Con el comando *kill* y la ID del proceso  
![img](capturas/capturaswap_p3_14.PNG)  
* Con los comandos que ya trae nginx  
![img](capturas/capturaswap_p3_15.PNG)  
  
#Haproxy: Instalaci�n y configuraci�n

Instalamos el programa con una simple ejecuci�n de:  
```
apt-get install haproxy  
```
  
Ahora, al igual que nginx, toca configurar. El archivo a configurar es */etc/haproxy/haproxy.cfg*:  
```
global
  daemon
  maxconn 256  
  
defaults
  mode http
  contimeout 4000
  clitimeout 42000
  srvtimeout 43000  
  
frontend http-in
  bind *:80
  default_backend servers  
  
backend servers  
  server m1 192.168.18.128:80 maxconn 32
  server m2 192.168.18.129:80 maxconn 32  
```
Como podemos ver, idicamos nuevamente la ip de nuestras m�quinas servidoras. Si queremos editar el peso, solo hay que a�adir:  
![img](capturas/capturaswap_p3_18.PNG)  
  
Realizamos las pruebas del mismo modo que con nginx:  
![img](capturas/capturaswap_p3_17.PNG)  

Y cuando hayamos terminado, paramos el proceso. En esta ocasi�n, solo funciona con kill:  
![img](capturas/capturaswap_p3_20.PNG)  

#Opcional: Instalaci�n y configuraci�n de Pound

Un tercer programa que nos permitir� utilizarlo como balanceador de carga es Pound. La instalaci�n con:  
```
apt-get install pound  
```
  
Tras la instalaci�n nos indicar� que el archivo no est� configurado y que est� "apagado" por defecto. En primer lugar editamos el archivo */etc/pound/pound.cfg* y 
a�adimos la configuraci�n de nuestras m�quinas. En mi caso solo he a�adido lo siguiente:  
![img](capturas/capturaswap_p3_22.PNG)  
  
Tras esto, editamos el fichero */etc/default/pound*, en concreto startup=0 y lo ponemos a 1:  
![img](capturas/capturaswap_p3_21.PNG)  

Ejecutamos el servidor (o reiniciamos en caso de que ya estuviese encendido):  
```
/etc/init.d/pound start  
```

Solo queda ver si funciona, de nuevo con un curl:  
![img](capturas/capturaswap_p3_23.PNG)  
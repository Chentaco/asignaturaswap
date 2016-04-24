##Pr�ctica 4: Comprobar el rendimiento de servidores web
#Autor: Ram�n S�nchez Garc�a

Tal y como dice el titulo, el objetivo de esta cuarta pr�ctica es el de comprobar el rendimiento de servidores web. Para ellos vamos a retomar nuestras 
m�quinas virtuales con todos sus servidores instalados y configurados como hicimos en las pr�cticas anteriores. Adem�s es importante tener configurados 
los balanceadores de carga tal y como hicimos en la [Pr�ctica 3](https://github.com/Chentaco/asignaturaswap/blob/master/Practicas/P3/Practica3.md).

En esta pr�ctica 4 hemos realizado las siguientes tareas:  

* Usar Apache Benchmark para comprobar el rendimiento, tanto de un servidor concreto como usando tanto nginx como hproxy (de la anterior pr�ctica).  
* Realizar lo mismo que en el punto anterior, pero usando Siege.  
* Optavivo: Realizar las mismas pruebas pero con un programa extra, en mi caso he usado OpenWebLoad.  
  
Para ello, y antes de empezar, he realizado un documento html llamado "prueba.hmtl" en el directorio /var/www/ en ambas m�quinas 
servidoras (recordemos que son M1 y M2):  
![img](capturas/capturasswap_p4_01.PNG)  
![img](capturas/capturasswap_p4_02.PNG)  
  
Hecho esto, vamos a realizar las pruebas desde la m�quina REAL (tambi�n se puede crear una M4 y realizarlas desde ella). Yo, por simple comodidad, las he 
hecho en la m�quina real, un Windows 7 desde el que trabajaremos en linea de comandos.  

#Comprobando el rendimiendo con Apache Benchmark

El primer m�todo que he usado es el de Apache Benchmark, herramienta incluida con la instalaci�n de Apache. Para m�s informaci�n, visita el siguiente 
[link](http://httpd.apache.org/docs/2.2/programs/ab.html).  
Para empezar a utilizarlo, iremos al directorio donde tenemos apache (normalmente C:\ o Program Files), y dentro de la carpeta, buscamos otra llamanda "bin". 
Dentro de esta deber�a estar un ejecutable "ab.exe". Para hacerlo funcionar, una vez hemos entrado al directorio **por l�nea de comandos**, usamos el siguiente 
comando:  
```
ab -n 1000 -c 10 http://ipmaquina/prueba.html
```  
Donde:  
* ab es la herramienta  
* -n 1000 es el n�mero de peticiones a la direcci�n web  
* -c 10 la cocurrencia de las peticiones (de 10 en 10)  
* http://ipmaquina/prueba.html la direcci�n a la que le haremos la prueba, en nuestro caso 192.168.18.128 para la M1 (servidora) y 192.168.18.130 para 
la M3 (la del balanceador). Mientras que prueba.html es el archivo de prueba que hemos creado antes.  

  
Si ejecutamos el comando, nos saldr� algo como esto:  
![img](capturas/capturasswap_p4_03.PNG)  
  
Entre todos estos datos, para la comparativa nos vamos a quedar con **"Time taken for tests"**, **"Failed requests"**, 
**"Requests per second"** y **"Time per request"**.  
  
Para cada tipo de escenario (servidor directa, con nginx y hapache) he realizado 10 pruebas distintas (solo en el caso de Apache Benchmark como veremos) 
que recojo en la siguiente tablas:  
![img](capturas/tablasapache.png)  

Ahora, para una mejor comparativa, recojo una comparativa con la media de cada escenario con Apache Benchmark, en la siguiente gr�fica:  
![img](capturas/graficasapache.png)  
Como los dem�s datos son iguales, no he hecho gr�fica para ellos.  
Podemos observar como claramente la conexi�n directa con el servidor es mucho m�s r�pida que usando un servidor intermedio para distribuir la carga. 
Tarda mucho menos tiempo directamente haciendo las peticiones y realiza muchas m�s que usando el balanceador de carga. Tambi�n podemos ver 
que hay haproxy tarda menos que nginx en hacer las peticiones.  


**NOTA 1**: Es MUY importante fijarse que cuando hagamos las pruebas a la m�quina balanceadora, esta tenga encendida el servicio correspondiente y los dem�s 
est�n apagados. Cada vez que probemos uno distinto, apagamos los otros, para evitar colisiones.  
**NOTA 2**: Es curioso como, al realizar una prueba con nginx activo, como Apache Benchmark nos dice que el balanceador est� activo:  
![img](capturas/capturasswap_p4_04.PNG)  

#Comprobando el rendimiendo con Siege  

Toca cambiar de herramienta, ahora le toca a Siege. Para informaci�n sobre esta herramienta, consulta el siguiente [enlace](https://www.joedog.org/siege-home/).  

Aunque parece similar a la herramienta de Apache, presenta opciones de ejecutci�n algo diferentes. Para utilizarla, tras descargarlo, entramos al directorio 
en el que est� el .exe y ejecutamos el siguiente comando:  
```
siege -b -t60S -v http://ipmaquina
```  
Donde:  
* -t60S es para indicarle que durante 60 segundos, va a estar realizando las pruebas.
* http://ipmaquina, en esta ocasi�n ser� directamente a la m�quina que queremos hacerle las pruebas la que pondremos (la de 128 y la de 130).  
Adem�s, por defecto, utiliza 15 usuarios para realizar las peticiones.  

Tras ejecutarlo, tendremos algo como esto:  
![img](capturas/capturasswap_p4_07.PNG)  
Nos quedamos con **"Availability"**, **"Elapsed time"**, **"Response time"** y **"Transaction rate"**.  
  
Realizamos las bater�as de pruebas a los mismos escenarios que con la herramienta del apartado anterior.  
**NOTA 3**: Al parecer, la carpeta de siege-windows debe estar en C:\ directamente. Si intentamos ejecutarlo desde otro directorio, nos saldr� el siguiente 
mensaje:  
![img](capturas/capturasswap_p4_06.PNG)  
**NOTA 4**: Por temas de tiempo (cada prueba dura un minuto) y que a veces salian errores, he optado por realizar 5 pruebas de cada escenario en lugar de 
10 como hice con Apache Benchmark.  
  
Recojo los datos de nuevo en las siguientes tablas y realizo la comparativa en los gr�ficos siguientes:  
![img](capturas/tablassiege.png)    
![img](capturas/graficasiege.png)  
Al igual que en el apartado anterior, solo he hecho gr�fica para aquellos datos que considero que se tienen que comparar y no son demasiado iguales.  
De nuevo, el n�mero de transiciones por segundo es mejor si est� conectado directamente que usando los balanceadores. En este caso, nginx es un poquito mejor 
que haproxy.  

#Opcional: Comprobando el rendimiento con OpenWebLoad  
La herramienta opcional que he usado es la que cita el titulo. La documentaci�n est� en [este enlace](http://openwebload.sourceforge.net/#how).  
Yo he utilizado el comando con los par�metros por defecto, donde utiliza 5 clientes:  
```
openload http://ipmaquina
```  
Tras realizar una ejecuci�n, obtenemos los siguientes datos:  
De aqu� me he quedado con **"Total TPS"**, **"Avg. Response time"** y **"Max Response Time"**.  
**NOTA 5**: Las peticiones de OpenWedLoad NO paran. Para hacerlo hay que presionar ENTER. Yo lo he hecho cada **5 peticiones**.  
Una vez m�s, los resultados y la comparativa con gr�ficas est� recogida en el siguiente apartado:  
![img](capturas/tablasopl.png)   
![img](capturas/graficaopl.png)   

Solo he realizado la gr�fica para el Total TPS (Transmision per second). El resultado es muy parecido al que obtimos anteriormente. La conexi�n directa 
sigue siendo la mejor, realiza muchas m�s transmisiones por segundo que con el balanceador.


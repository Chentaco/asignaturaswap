#Ejercicio 5.1: Buscar información sobre cómo calcular el número de conexiones por segundo.
  
Como hemos visto en prácticas, hemos podido ajustar el número de conexiones al realizar pruebas de peticiones, sin embargo, lo que buscamos es saber como se  
calcula.  
Indagando por internet he encontrado la siguiente formula:  
```
Requests per connection = handles requests / handled connections  
```
Necesitamos conocer ambos parámetros. Un programa que nos permite conocerlos es el propio nginx, solo hay que editar el archivo **nginx.conf** con:  
```
   location /nginx_status {
        # Turn on stats
        stub_status on;
        access_log   off;
        # only allow access from 192.168.1.5 #
        allow 192.168.1.5;
        deny all;
   }  
```
Y realizar la prueba con el comando siguiente:  
```
http://ip.address.here/nginx_status
```
  
Al realizar la prueba, en el apartado **server accepts handled requests** saldrán una serie de numeros, donde, de izquierda a derecha son **Accepted connections**, 
**Handled connections**, **Handles requests** respectivamente.
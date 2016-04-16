#Ejercicio T4.6: Buscar información sobre los bloques de IP para los distintos países o continentes. Implementar en JavaScript o PHP la detección de la zona desde donde se conecta un usuario.

*No he encontrado nada por continentes, pero sí por paises. Dado que son 250 paises, dejo mejor el link que copiarlo todo aquí:  
  
[Asignación de direcciones IP por país](https://www.maxmind.com/es/allocation-of-ip-addresses-by-country)  

*Sobre el script, he encontrado en internet el código de un programa en PHP que permite la detección de la zona donde está conectado el usuario haciendo uso de [una herramienta de terceros](http://ipinfo.io/) cuyo código es el siguiente:  
  
  
```
$ip = $_SERVER['REMOTE_ADDR'];
$details = json_decode(file_get_contents("http://ipinfo.io/{$ip}/json"));
echo $details->city; 
```
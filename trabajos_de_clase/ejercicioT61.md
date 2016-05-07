#Ejercicio T6.1: Aplicar con iptables una política de denegar todo el tráfico en una de las máquinas de prácticas. Comprobar el funcionamiento.  
Desde una de las máquinas servidoras (M1 o M2) configuramos con iptables el acceso. Lo que tenemos que hacer es bloquearlo todo, para ello usamos el comando:  
```
iptables -A INPUT -j DROP  
```
Ahora, al realizar peticiones desde una máquina cliente, no recibiremos nada, todas las peticiones están bloqueadas.
  
#Aplicar con iptables una política de permitir todo el tráfico en una de las máquinas de prácticas. Comprobar el funcionamiento.  
Del mismo modo, toca autorizar todo, simplemente uso el comando anterior pero con **ACCEPT** en lugar de **DROP**:  
```
iptables -A INPUT -j ACCEPT  
```
Deberá permitir el acceso desde un cliente. Sin embargo, es vulnerable a cualquier acceso. Lo mejor es volver todo a como estaba.  
Para comprobar el estado del firewall, usamos:  
```
iptables -n -L -v
```
Fuente: http://www.cyberciti.biz/tips/linux-iptables-examples.html
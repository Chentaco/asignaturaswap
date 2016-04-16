#Ejercicio T4.4: Instala y configura en una máquina virtual el balanceador ZenLoadBalancer. Compara con la dificultad de la instalación y configuración usando nginx o haproxy (práctica 3).

En primer lugar, bajamos la ISO del balanceador ZenLoadBalancer desde [la página de descargas](http://www.zenloadbalancer.com/community/downloads/) y procedemos a su instalación en una máquina virtual vacia. 
Se nos ejecutará un instalador, el cual nos recordará a la instalación de Ubuntu Server que realizamos en prácticas anteriores. Seguimos el instalador de la misma forma que hicimos en dichas prácticas.  
Terminada la instalación, logueamos y añadimos al repositorio de updates los recursos para dicha actualización. Al final del ejercicio dejo un enlace donde pone lo que hay que añadir.  
Realizamos el apt-get upgrade y comprobamoso que tenemos la última versión de ZenLoadBalancer con:  
```
apt-cache search zenloadbalancer
```

Si todo ha ido bien, ya tendriamos nuestro balanceador funcionando. Ahora, para configurarlo tendriamos que acceder a él desde un navegador con la url:  
```
https:\\ipdelamaquinaconZenLoadBalancer:444
```  
  
Nos pedirá contraseña, usamos "admin" para usuario y contraseña, accediendo asi al panel de administración.  
Desde aquí será donde configuraremos todo lo reference a balanceadores, granjas, etc.

Aquí está la guia para la configuración del balanceador: (http://www.zenloadbalancer.com/zlb-administration-guide-v304/)  

*Desde mi punto de vista, me parece algo más complejo usar este tipo de balanceador, aunque está mucho más completo que nginx y haproxy. Además permite realizar muchas otras funciones a través de su panel de administración, con una interfaz fuera de la
 de comandos que ofrecen los otros programas. Aun asi, me parece más complejo, ya que los otros encendiéndolos y con una configuración base, funcionaban.


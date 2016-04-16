#Ejercicio T4.5: Probar las diferentes maneras de redirección HTTP. ¿Cuál es adecuada y cuál no lo es para hacer balanceo de carga global? ¿Por qué?

He encontrado tres métodos de redireccionamiento HTTP:  
* HTML: Se muestra un mensaje a la hora de abrir la página que nos lleva a la nueva ubicación de la página.  
* PHP: No muetra ningún mensaje al usuario, simplemente realiza la redirección cuando abrimos la página.
* JavaScript: Se puede realizar de las dos maneras anteriores, con un script que avise como con HTML o que lo haga sin avisar, como con PHP.  

  
Si tuviera que elegir alguno de esots métodos para el balanceo de carga, utilizaría alguno que no avisase al usuario, ya que el simple hecho de que se nos tenga que avisar consume más tiempo que el simple cambio automático. Descartaria HTML o cualquiera que envie un mensaje y cargue la página a la que quiero ir tras un tiempo.

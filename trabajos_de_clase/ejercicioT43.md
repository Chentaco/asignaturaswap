#Ejercicio T4.3: Buscar información sobre los métodos de balanceo que implementan los dispositivos recogidos en el ejercicio 4.2

No mencionan ningún método de balanceo de carga en específico, por lo que supongo que tendrá las básicas en el caso de LoadMaster, que son:  
* Round-Robin. Las peticiones se entregan uno a uno en los servidores.  
* Weighted Round-Robin Las peticiones se entregan dependiendo del peso que se le de a cada servidor.  
* LeastConnection. Las peticiones se hacen dependiendo del número de conexiones que tenga cada servidor.  
* Weighted LeastConnection. Las peticiones se entregan dependiendo del peso y el número de conexiones que se tengan. 
#Calcular la disponibilidad del sistema descrito en edgeblog.net si en cada subsistema tenemos en total 3 elementos

La disponibilidad se calcula como:

· As = Ac1 * Ac2 * Ac3 * ... * Acn

Para un sistema con "n" componentes.

Desde [la web indicada](http://www.edgeblog.net/2007/in-search-of-five-9s/) podemos obtener los datos de todos los elementos. Con ellos calculamos la disponibilidad de cada subsistema y el total:

Web2= 0.85+(1-0.85)*0.85 = 0.9775 (97.75%)
Web3= 0.9775+(1-0.9775)*0.85 = 0.9966

App2(Application)= 0.9+(1-0.9)*0.9 = 0.99
App3= 0.99+(1-0.99)*0.9 = 0.999

Db2(Database)=0.999+(1-0.999)*0.999 = 0.99999
Db3=0.99999+(1-0.99999)*0.999 = 0.9999999

DNS2(Domain Name System)= 0.98+(1-0.98)*0.98 = 0.9996
DNS3 0.9996+(1-0.9996)*0.98 = 0.9999

Firewall2= 0.85+(1-0.85)*0.85 = 0.9775
Firewall3= 0.9775+(1-0.9775)*0.85 = 0.9966

Switch2= 0.99+(1-0.99)*0.99= 0.9999
Switch3= 0.9999+(1-0.9999)*0.99= 0.999999

DC2(Data Center)= 0.9999+(1-0.9999)*0.9999 = 0.99999999
DC3= 0.99999999+(1-0.99999999)*0.9999 = 0.999999999999

ISP2(Internet Service Provider)= 0.95+(1-0.95)*0.95 = 0.9975
ISP3= 0.9975+(1-0.9975)*0.95 = 0.999875


El resultado final lo obtenemos de la siguiente forma:

As = Web3*App3*Db3*DNS3*Firewall3*Switch3*DC3*ISP3 = 0.9908 (99.08%)


# Tema 2

**1. Calcular la disponibilidad del sistema si tenemos dos réplicas de cada elemento (en total 3 elementos de cada subsistema).**
Basándonos en los datos de las diapositivas, replicamos cada elemento:
	
		web3 = 97.75% + ((1-97.75%) * 85%) = 99.6625%
	
		aplication3 = 99% + ((1-99%) * 90%) = 99.9%
	
		database3 = 99.9999% + ((1-99.9999%) * 99.9%) = 99.999999%

        DNS3 = 99.96% + ((1-99.96%) * 98%) = 99.9992%

 	    firewall3 = 97.75% + ((1-97.75%) * 85%) = 99.6625%
	
    	switch3 = 99.99% + ((1-99.99%) * 99%) = 99.9999%

    	DataCenter3 = 99.99% + ((1-99.99%) * 99.99%) = 99.9999999%

    	ISP3 = 99.75% + ((1-99.75%) * 95%) = 99.9875%

Calculamos ahora la disponibilidad del sistema:
	
	disponibilidad = 99.6625% *  99.9% * 99.999999% * 99.9992% * 99.6625% * 99.9999% * 99.9999999% * 99.9875% = 99.213515%
	
**2. Buscar frameworks y librerías para diferentes lenguajes que permitan hacer aplicaciones altamente disponibles con relativa facilidad.Como ejemplo, examina PM2 https://github.com/Unitech/pm2 que sirve para administrar clústeres de NodeJS.**
Frameworks:
- **MooTools:** framework de JavaScript compacto, orientado a objetos utilizados para el intermedio con JavaScript.
- **Prototype:** framework de JavaScript para facilitar el desarrollo de aplicaciones web dinámicas.
- **Linux-HA:** es un framework para cluster que usen Linux.

Librerías:
- **JQuery:** biblioteca de JavaScript para ayudar en el manejo de documentos HTML, manejo de eventos, animación e interacciones Ajax para el desarrollo web rápido.
- **Math.js:** librería de Node.js centrada en cálculos matemáticos.

**3. ¿Cómo analizar el nivel de carga de cada uno de los subsistemas en el servidor? Buscar herramientas y aprender a usarlas.**
- **Top:** nos muestra el uso de memoria y de CPU que están haciendo los procesos que se están ejecutando. Los procesos son ordenados por el uso de CPU y muestran PID, usuario, porcentaje de CPU y porcentaje de memoria.
- **Munin:** es un software de monitorización que permite monitorizar uno o varios equipos y presenta los resultados vía web.
- **Nagios:** es un sistema de monitorización de redes ampliamente utilizado, de código abierto, que vigila los equipos (hardware) y servicios (software) que se especifiquen. Entre sus características principales está la monitorización de servicios red, de los recursos de sistemas hardware, etc.
- **Gnome-System-Monitor:** es una aplicación que permite monitorizar tanto los procesos como los sistemas de memoria y red.
- 
**4. Buscar ejemplos de balanceadores software y hardware. Buscar productos comerciales para servidores de aplicaciones. Buscar productos comerciales para servidores de almacenamiento.**
- **Ejemplos de balanceadores software y hardware (productos no comerciales):** entre los balanceadores de carga software más conocidos están haproxy, nginx, pen y pound. Entre los balanceadores de carga software se encuentran barracuda y cisco.
- **Buscar productos comerciales para servidores de aplicaciones:** los más conocidos son: Oracle Application Server, Apache Tomcat y Microsoft Azure.
- **Buscar productos comerciales para servidores de almacenamiento:** algunos ejemplos son: Dell Complellent FS8600 y IBM EXP2500 Storage Enclosure. 


# Práctica 3: #

## Instalación de nginx ##

En primer lugar, importamos la clave del repositorio tal y como aparece en el guión:


![primera imagen de la terminal](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/1.png)

![segunda imagen de la terminal](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/2.png)

El siguiente paso es añadir el repositorio al fichero /etc/apt/sources.list. Para ello, hay que darle permisos a este fichero para poder añadirlo. Se usan las órdenes que aparecen en el guión y ya se puede instalar mediante:

		sudo apt-get update

		sudo apt-get install nginx

## Configuración del balanceo de carga usando nginx ##

En primer lugar definimos el grupo de servidores a los que redirigiremos el tráfico que llegue al balanceador mediante la directiva upstream.

Tal y como nos indica el guión, la configuración tal cual está corresponde a una funcionalidad de servidor web por lo que debemos modificar el archivo /etc/nginx/conf.d/default.conf. Modificamos dicho archivo de la siguiente manera:

	upstream apaches {
		server 192.168.2.137;
		server 192.168.2.138;
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

Una vez configurado, lanzamos el servicio nginx:

		sudo service nginx restart

Ahora, usando el comando curl, le enviamos peticiones a la IP de la máquina balanceadora como vemos en la siguiente imagen, se aplica el algoritmo “round-robin”:

![tercera imagen terminal](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/4.png)

A continuación le asignamos un peso a cada máquina (en mi caso a la primera le he asignado peso uno y a la segunda peso dos) y probamos otra vez el comando curl:

![cuarta imagen terminal](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/5.png)

## Instalación de haproxy: ##

Para realizar la instalación de haproxy, debemos parar el servicio de nginx con:

		service nginx stop

Una vez parado, debemos instalar con apt-get el balanceador de carga haproxy:

		sudo apt-get install haproxy

## Configuración de haproxy: ##

Una vez instalado, modificamos el archivo /etc/haproxy/haproxy.cfg para modificar la configuración por defecto.
 
Debemos añadir al archivo, tal y como aparece en el guión de prácticas, lo siguiente:

		global
		daemon
    	maxconn 256

		default
		mode http
		contimeout 4000
		clitimeout 42000
		srvtimeout 43000


		frontend http-in
 	 	bind *:80
    	default_backend servers

		backend servers
    	server  m1 192.168.2.137:80 maxconn 32
    	server  m2 192.168.2.138:80 maxconn 32

Para comprobar el funcionamiento, lanzamos el comando:

		sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg 

Y lanzamos peticiones con curl como vemos en la imagen:

![quinta imagen terminal](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/8.png)

## Someter a una alta carga el servidor: ##

En primer lugar, instalamos el apache benchmark con:

		sudo apt-get install apache2-utils

A continuación pasamos a probar el apache benchmark con cada uno de los balanceadores.

### nginx: ###

Para probar la granja web, ejecutamos:

		ab -n 10000 -c 500 http://192.168.2.139/hola.html

![sexta imagen terminal](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/6.png)


![septima imagen terminal](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/7.png)

Como podemos ver en la imagen, ha tardado 27.575 segundos en realizar 10000 peticiones, ejecutadas de 500 en 500. 

Además podemos ver que no ha fallado ninguna operación y, por tanto, no se ha saturado.

### haproxy: ###

Para probar la granja web, ejecutamos igual que en el caso anterior:

	ab -n 10000 -c 500 http://192.168.2.139/hola.html

![terminal8](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/9.png)

![terminal9](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/10.png)

Como podemos ver en la imagen, ha tardado 7.866 segundos en realizar 10000 peticiones, ejecutadas de 500 en 500. Además podemos ver que no ha fallado ninguna operación y, por tanto, no se ha saturado.

### Tabla comparativa de resultados ##

|			| nginx | haproxy |
|-----------|:-------:|:---------:|
|Tiempo total de ejecución  | 27.575		| 7.866	  |
|Operaciones fallidas  | 0	| 0	  |
|Operaciones por segundo  | 362.65		| 1271.34  |
|Tiempo por petición  | 1378.748	| 393.285	  |

Como podemos observar en la tabla anterior, haproxy da mejores resultados puesto que los tiempos son menores, es decir, ha tardado menos en ejecutar las mismas peticiones. Aún así, podemos comprobar que ninguno de los dos se ha saturado al ejecutarlas.



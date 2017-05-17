# Práctica 4. Asegurar la granja web. #

## 1. Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS ##

##### Generar e instalar un certificado autofirmado #####

Para generar un certificado SSL autofirmado solo debemos activar el módulo SSL de Apache, generar los certificados y especificarle la ruta a los certificados en la configuración. 
Para ello, en primer lugar, ejecutamos **a2enmod ssl** y reiniciamos el apache con **service apache2 restart**.

A continuación, creamos la carpeta llamada ssl dentro de apache2 con la orden **mkdir /etc/apache2/ssl** y generamos los certificados con:
	
	openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout 
	
	/etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt

y por último rellenamos los datos que se nos piden tal y como vemos en la siguiente imagen:

![clave SSL](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/claveSSL.png)

Después tenemos que editar el archivo de configuración del sitio default-ssl tal y como indica el guión de prácticas. Para ello:

	sudo nano /etc/apache2/sites-available/default-ssl.conf

añadimos las dos siguientes líneas a este archivo:

	SSLCertificateFile /etc/apache2/ssl/apache.crt
	SSLCertificateKeyFile /etc/apache2/ssl/apache.key

![modificación de la configuración](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/modConf.png)

Para terminar, activamos el sitio default-ssl y reiniciamos el apache:

	a2ensite default-ssl
	service apache2 reload

Comprobamos que el certificado es autofirmado:

![prueba SSL](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/pruebaSSL.png)

## Configuración del cortafuegos ##

En primer lugar, comprobamos el estado del cortafuegos con el siguiente comando:

	iptables -L -n -v

y vemos en la siguiente imagen dicho estado.

![Estado Cortafuegos](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/estadoCortaguegos.png)

A continuación, pasamos a denegar cualquier tráfico de información, para ello, ejecutamos lo siguiente:

	iptables -P INPUT DROP
	iptables -P OUTPUT DROP
	iptables -P FORWARD DROP

y volvemos a comprobar con la orden anterior el estado del cortafuegos, el cual se muestra en la siguiente imagen.

![Estado Cortafuegos 2](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/estadoCortaguegos2.png)

Ahora, permitimos el tráfico de salida y bloqueamos el de entrada con los siguientes comandos:

	iptables -P INPUT DROP
	iptables -P FORWARD DROP
	iptables -P OUTPUT ACCEPT
	iptables -A INPUT -m state --state NEW, ESTABLISHED -j ACCEPT 

y comprobamos con la primera orden el estado del cortafuegos.

![Estado Cortafuegos 3](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/estadoCortaguegos3.png)

Seguimos los pasos del guión y bloqueamos todo el tráfico ICMP para evitar ataques como el del ping de la muerte, para ello:

	iptables -A INPUT -p icmp --icmp-type echo-request -j DROP

y comprobamos el estado del cortafuegos.

![Estado Cortafuegos 4](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/estadoCortaguegos4.png)

El siguiente paso es abrir el puerto 22 para permitir el acceso por SSH:

	iptables -A INPUT -p tcp --dport 22 -j ACCEPT
	iptables -A OUTPUT -p udp --sport 22 -j ACCEPT

y comprobamos el estado del cortafuegos.

![Estado Cortafuegos 5](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/estadoCortaguegos5.png)

El siguiente paso es abrir los puertos 80 y 443 de HTTP y HTTPS para la configuración de un servidor:

	iptables -A INPUT -m state --state NEW -p tcp --dport 80 -j ACCEPT
	iptables -A INPUT -m state --state NEW -p tcp --dport 443 -j ACCEPT

y comprobamos de nuevo el estado del cortafuegos.

![Estado Cortafuegos 6](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/estadoCortaguegos6.png)

Ahora abrimos el puerto 53 para permitir el acceso a DNS:

	iptables -A INPUT -m state --state NEW -p udp --dport 53 -j ACCEPT
	iptables -A INPUT -m state --state NEW -p tcp --dport 53 -j ACCEPT 

y comprobamos el estado del cortafuegos.

![Estado Cortafuegos 7](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/estadoCortaguegos7.png)

Por último, bloqueamos todo el tráfico de entrada y de salida desde una IP:

	iptables -I INPUT -s 192.168.2.138 -j DROP
	iptables -I OUTPUT -s 192.168.2.138 -j DROP

y comprobamos el estado del cortafuegos.

![Estado Cortafuegos 8](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/estadoCortaguegos8.png)

También podemos evitar el acceso a facebook por ejemplo:

	iptables -A OUTPUT -p tcp -d www.facebook.com -j DROP

y mostramos el cortafuegos.

![Estado Cortafuegos 9](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/estadoCortaguegos9.png)

##### Creación de un script que se ejecute en el arranque del sistema. #####

![Script](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/script.png)

Para terminar, para que este archivo se ejecute al arrancar la máquina, tenemos que añadir al archivo de crontab la siguiente linea:

	@reboot /home/inma/script-iptables.sh 
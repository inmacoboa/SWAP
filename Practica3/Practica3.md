# Práctica 3: #

## Instalación de nginx ##

En primer lugar, importamos la clave del repositorio tal y como aparece en el guión:


![primera imagen de la terminal](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/1.png)

![segunda imagen de la terminal](https://github.com/inmacoboa/SWAP1617/blob/master/Practica3/imagenes/2.png)

El siguiente paso es añadir el repositorio al fichero /etc/apt/sources.list. Para ello, hay que darle permisos a este fichero para poder añadirlo. Se usan las órdenes que aparecen en el guión y ya se puede instalar mediante:

	sudo apt-get update
	sudo apt-get install nginx
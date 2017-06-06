# Práctica 6: Discos en RAID #

### Añadir dos nuevos discos a una de las máquinas ###

Añadimos dos discos nuevos a una de las máquinas tal y como vemos en la siguiente imagen de 3GB cada uno:

![discos duros](https://github.com/inmacoboa/SWAP1617/blob/master/Practica6/imagenes/discosduros.png)

### Configuración del RAID por software ###

Instalamos la herramienta mdadm con el siguiente comando:

	sudo apt-get install mdadm

Una vez instalado, buscamos la información de los discos que anteriormente hemos añadido. Para ello, usamos la siguiente orden:

	sudo fdisk -l 

y vemos en la siguiente imagen la información que se nos proporciona:

![discos](https://github.com/inmacoboa/SWAP1617/blob/master/Practica6/imagenes/discos.png)

Ahora creamos el RAID1 indicando el número de dispositivos a usar, en nuestro caso 2, y el nombre de ambos. Usamos el siguiente comando:

	sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sda /dev/sdb

Además debemos darle formato con:

	sudo mkfs /dev/md0

Creamos el directorio donde se montará la unidad del RAID:

	sudo mkdir /dat

	sudo mount /dev/md0 /dat

Ahora comprobamos el estado del RAID tal y como vemos en la siguiente imagen:

![estadoRaid](https://github.com/inmacoboa/SWAP1617/blob/master/Practica6/imagenes/estadoRaid.png)

Podemos automatizar este proceso modificando el archivo /etc/fstab.
Para ello, obtenemos el UUID del disco tal y como vemos en la imagen:

![uuid](https://github.com/inmacoboa/SWAP1617/blob/master/Practica6/imagenes/uuid.png)

Añadimos al final del fichero nombrado anteriormente el uuid. Podemos verlo en la siguiente imagen:

![fstab](https://github.com/inmacoboa/SWAP1617/blob/master/Practica6/imagenes/fstab.png)

### Simular un fallo en uno de los discos: ###

Vamos a simular un error en uno de los discos para poder comprobar que se ha recuperado correctamente.
Para simular el fallo usamos el comando:

	sudo mdadm --manage --set-faulty /dev/md127 /dev/sdb

Para retirar el disco “en caliente” usamos:

	sudo mdadm --manage --add /dev/md127 /dev/sdb

![fallo](https://github.com/inmacoboa/SWAP1617/blob/master/Practica6/imagenes/fallo.png)

Comprobamos el estado:

![estadoRaid2](https://github.com/inmacoboa/SWAP1617/blob/master/Practica6/imagenes/estadoRaid2.png)

Por último volvemos a añadir el disco en caliente y comprobamos el estado:

![estadoRaid3](https://github.com/inmacoboa/SWAP1617/blob/master/Practica6/imagenes/estadoRaid3.png)

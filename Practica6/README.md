
# Práctica 6: Discos en RAID

## 1. Realizar la configuración de dos discos en RAID 1 bajo Ubuntu, automatizando el montaje del dispositivo creado al inicio del sistema.

- Primero tenemos que agregar dos discos del mismo tipo y capacidad

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/agregarHD.png)

- Tras esto ejecutamos el comando `fdisk -l` para ver la información de los discos

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/fdisk.png)

- Ahora creamos **RAID1**, usando **/dev/md0**, indicando el número de dispositivos que va a utilizar y su ubicación

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/crearArray.png)

- Luego le damos formato con el comando `mkfs`, pero como no hemos reiniciado aún la máquina, el nombre que vamos a utilizar es **/dev/md0**

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/mkfs.png)

- Después de crear el directorio donde montaremos RAID1 (`mkdir /dat`) y montar RAID en el (`mount /dev/md0 /dat`), comprobamos que el proceso se ha ralizado correctamente con `mount`

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/mount.png)

- Con `mdadm --detail /dev/md0` comprovaremos el funcionamiento del RAID

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/mdadmDetails.png)

- Ahora tenemos que configurar el sistema para que monte RAID al arrancar el sistema, para ello utilizaremos su identificador único, que podemos obtenerlo con `ls -l /dev/disk/by-uuid`

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/uuids.png)

- Tras obtener el identificador tenemos que editar el archivo **/etc/fstab** y añadir la línea:
    - `UUID=ccbbbbcc-dddd-eeee-ffff-aaabbbcccddd /dat ext2 defaults 0 0`

## 2. Simular un fallo en uno de los discos del RAID (mediante comandos con el mdadm), retirarlo “en caliente”, comprobar que se puede acceder a la información que hay almacenada en el RAID, y por último, añadirlo al conjunto y comprobar que se reconstruye correctamente.

- Primero simularemos un fallo en uno de los discos con `mdadm --manage --set-faulty /dev/md0 /dev/sdb` y vemos su comportamiento

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/mdadmFaulty.png)

- También retiramos "en caliente" un disco, concretamente el que está marcado como que ha fallado con `mdadm --manage --remove /dev/md0 /dev/sdb` y vemos su comportamiento

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/mdadmRemove.png)

- Por último añadimos el disco que hemos quitado antes con `https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/mdadmAdd.png` y comprovamos su funcionamiento

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica6/mdadmAdd.png)

- Para comprobar que todo funciona correctamente reiniciamos la máquina y ejecutamos el comando `mdadm --detail /dev/md127`. Ahora lo hacemos con `/dev/md127` porque hemos reiniciado el sistema y se renombra con este nombre

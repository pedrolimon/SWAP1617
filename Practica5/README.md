
# Práctica 5. Replicación de bases de datos MySQL

## 1. Crear una BD con al menos una tabla y algunos datos.

- Para crear una Base de Datos con una tabla tenemos que iniciar el servicio **MySQL**: `mysql -u root -p`
- Después de teclear la contraseña, creamos la Base de Datos: `create database contactos;`

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica5/creacionBD.png)

- Tras esto creamos la tabla ** *usuarios* ** y le introducimos algún dato

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica5/creacionTabla.png)

## 2. Realizar la copia de seguridad de la BD completa usando mysqldump.

- Para realizar una copia de seguridad tendremos que utilizar la herramienta `mysqldump`, tenemos que bloquear las tablas antes de realizar este paso para evitar futuros errores

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica5/creacionBackup.png)

## 3. Restaurar dicha copia en la segunda máquina (clonado manual de la BD).

- Copiamos el archivo de seguridad a la otra máquina con el comando `scp`, después creamos la Base de Datos y restauramos los datos contenidos en la Base de Datos de la copia de seguridad, en este proceso se crearán tantas tablas como sean necesarias

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica5/copiadoBD.png)

## 4. Realizar la configuración maestro-esclavo de los servidores MySQL para que la replicación de datos se realice automáticamente.

- Primero tenemos que hacer la configuración inicial de mySQL en el maestro que consiste en comentar la línea:
    - `#bind-address 127.0.0.1`
- Y añadir la líneas:
    - `log_error = /var/log/mysql/error.log`
    - `server-id = 1`
    - `log_bin = /var/log/mysql/bin.log`
- Tras esto reiniciamos el servicio: `/etc/init.d/mysql restart`
- Aún en el equipo que albergará al maestro tenemos que crear un usuario y darle permisos. Por último obtendremos los datos de la configuración del maestro para utilizarlos en el esclavo

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica5/maestro1.png)

- Ahora en la máquina esclava entramos en **MySQL** e introducimos los datos del maestro (ya que nuestra versión es superior a la 5.5)

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica5/esclavo1.png)

- Tenemos que revisar el valor de **Seconds_Behind_Master** y comprobar que es distinto de “null”. Si es igual a 0 todo funcionará bien

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica5/esclavo2.png)

- Para probar que funciona correctamente introducimos algunos datos en la tabla desde la máquina esclavo

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica5/maestro2.png)

- Y comprobamos que funciona correctamente si los cambios se han producido también en el esclavo

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica5/esclavo3.png)

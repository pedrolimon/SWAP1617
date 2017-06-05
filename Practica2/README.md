
# Práctica 2: Clonar la información de un sitio web

## 1. Clonado de una carpeta entre las dos máquinas

- Partimos desde la idea de que la máquina 1 tiene una carpeta llamada m1 y la máquina 2 no

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica2/archivos.png)

- Tras esto, instalamos la herramienta **Rsync**: `apt-get install rsync` y clonamos la carpeta `/var/www/` de la máquina 1 a la máquina 2 con el comando: `rsync -avz -e ssh root@10.0.2.8:/var/www/ /var/www/`
- Si no hacemos nada, nos dará error a la hora de introducir la contraseña ya que la configuración para permitir que el usuario root pueda logearse está desahbilitada
- Para habilitarla editamos el archivo `/etc/ssh/sshd_config` y cambiar la instrucción `PermitRootLogin` a `yes`
- Una vez editado el archivo, reiniciamos el archivo `service ssh restart`

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica2/clonadoRsync.png)

## 2. Configuración de ssh para acceder sin que solicite contraseña

- Lo primero que hay que hacer es crear las claves para poder acceder mediante **SSH** con el comando `ssh-keygen -t rsa`

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica2/clavesParaSSH.png)

- No pedirá una serie de datos, pero los dejaremos en blanco ya que queremos acceder sin que nos pida la contraseña para poder acceder
- Una vez que tengamos generadas las claves tendremos que copiarlas de la máquina 2 a la máquina 1 con el comando `ssh-copy-id -i ~/.ssh/id_rsa.pub root@10.0.2.8`
- Cuando copiemos as claves provamos el acceso mediante: `ssh 10.0.2.8 -l root`

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica2/SSHSinClaves.png)

## 3. Establecer una tarea en cron que se ejecute cada hora para mantener actualizado el contenido del directorio /var/www entre las dos máquinas

- Tenemos que editar el archivo `etc/crontab` (que especifica los procesos que deben ejecutarse) y añadimos la línea: `00 * * * * root rsync -avz -e ssh root@10.0.2.8:/var/www/ /var/www/`
- Esta línea indica que cada mínuto 00, de cada hora, de cada día del mes, de cada día de la semana, el usuario **root** ejecutará la instrucción descrita para clonar la carpeta `/var/www/` de la máquina 1 a la máquina 2

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica2/crontab.png)

- Tras esto esperamos unos minutos y comprobamos el correcto funcionamiento de la instrucción descrita en el archivo **crontab**

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica2/comprobar.png)

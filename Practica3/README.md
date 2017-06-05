
# Práctica 3. Balanceo de carga

## 1. Configurar una máquina e instalarle el nginx como balanceador de carga

- Si tenemos instalado **Apache** debemos parar el servicio: `service apache2 stop`. En mi caso no lo tengo instalado
- Tras esto instalamos **Nginx**: `apt-get install nginx`. Una vez instalado nginx, pasamos a su configuración modificando el archivo `/etc/nginx/conf.d/default.conf`
- El archivo de configuración debe quedar así
![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica3/confNginxRoundRobin.png)
- Tras la configuración tenemos que deshabilitar la opción de que funcione como servidor web, para ello hay que comentar la línea `#include /etc/nginx/sites-enabled/*;` dentro del archivo: `/etc/nginx/nginx.conf`. Tras la configuración tenemos que deshabilitar la opción de que funcione como servidor web, para ello hay que comentar la línea `#include /etc/nginx/sites-enabled/*;` dentro del archivo: `/etc/nginx/nginx.conf`
- Por último reiniciamos el servicio: `service nginx restart` y desde una máquina extrna y con la herramienta **curl** probamos que funcione bien
![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica3/curlNginx.png)

## 2. Configurar una máquina e instalarle el haproxy como balanceador de carga

- Tenemos que instalar **Haproxy**: `apt-get install haproxy`
- Tras esto debemos configurar el archivo: `/etc/haproxy/haproxy.cfg` quedando de la siguiente forma
![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica3/confHaproxyRoundRobin.png)
- Para probar este nuevo balanceador debemos parar el servicio de nginx: `service nginx stop`. A continuación lanzamos haproxy con: `/usr/sbin/haproxy -f  /etc/haproxy/haproxy.cfg`
- Desde una máquina extrna y con la herramienta **curl** probamos que funcione bien
![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica3/curlHaproxy.png)

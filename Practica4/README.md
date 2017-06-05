
# Práctica 4. Asegurar la granja web

## 1. Instalar un certificado SSL autofirmado para configurar el acceso HTTPS a los servidores.

- Para generar un certificado **SSL** autofirmado en **Ubuntu Server** lo primero que hay que hacer es activar el módulo **SSL** de **Apache**: `a2enmod ssl`
- Tras esto reiniciamos el servicio: `service apache2 restart` y creamos el directorio donde guardaremos los certificados generados

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica4/a2enmod.png)

- Después generamos los certificados en os que hay que rellenar unos simples datos

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica4/openssl.png)

- Hay que editar el archivo `/etc/apache2/sites-available/default-ssl.conf` y añadimos debajo de `SSLEngine on`: 
    - `SSLCertificateFile /etc/apache/ssl/apache.crt`
    - `SSLCErtificateKeyFile /etc/apache/ssl/apache.key`

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica4/default-conf.png)

- Por último activamos el sitio `defauls-ssl` y reiniciamos apache: `service apache2 reload`

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica4/a2ensite.png)

- Para hacer que nginx pueda hacer uso de HTTPS tenemos que copiar el par de claves de la máquina 1 a la máquina balanceadora:
    - `scp root@10.0.2.8:/etc/apache/ssl/apache.key /etc/nginx/ssl/nginx.key`
    - `scp root@10.0.2.8:/etc/apache/ssl/apache.crt /etc/nginx/ssl/nginx.crt`

- Tras esto editamos el archivo de configuración `/etc/nginx/conf.d/default.conf` y añadimos las líneas:
    - `listen 443 ssl;`
    - `ssl_certicate /etc/nginc/ssl/nginx.crt;`
    - `ssl_certificate_key /etc/nginx/ssl/nginx.key;`
    
![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica4/sslNginx.png)

- Por último reiniciamos **Nginx** y probamos su funcionamiento

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica4/comprobacionSSL.png)

## 2. Configurar las reglas del cortafuegos con IPTABLES para asegurar el acceso a los servidores web.

- Para la configuración del cortafuegos con iptables en linux, lo primero que hacemos es un script con las reglas necesarias

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica4/scriptiptables.png)

- Después le damos permisos de ejecución: `chmod +x iptablesScript.sh` y lo ejecutamos: `./iptablesScrip.sh`
- Tras esto comprobamos el correcto funcionamiento: `netstat -tulpn`
 
![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica4/netstat.png)
 
- Por último copiamos el script creado al archivo `/etc/rc.local` para que la configuración del cortafuegos sea permanente


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

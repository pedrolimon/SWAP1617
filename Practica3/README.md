
# Práctica 3. Balanceo de carga

## 1. configurar una máquina e instalarle el nginx como balanceador de carga

- Si tenemos instalado **Apache** debemos parar el servicio: `service apache2 stop`. En mi caso no lo tengo instalado
- Tras esto instalamos **Nginx**: `apt-get install nginx`. Una vez instalado nginx, pasamos a su configuración modificando el archivo `/etc/nginx/conf.d/default.conf`
- El archivo de configuración debe quedar así

![img](https://github.com/pedrolimon/SWAP1617/blob/master/Practica3/confNginxRoundRobin.png)

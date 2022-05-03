# Practica-servidor-web-Nginx

# Instalación Nginx
Comando: ```sudo apt-get-install nginx``` <br> <br>
Como en Ubuntu se inicia automaticamente, simplemente poniendo la ip del servidor en el navegador (localhost en este caso), se puede comprobar que se ha instalado correctamente. 
![imagen](https://user-images.githubusercontent.com/91600940/166141928-8765504d-e54a-43fd-b92b-1451cca5b83f.png)

# Configuración Nginx
Los archivos de configurarlo se encuentran en el directorio ```/etc/nginx/```
```
conf.d          koi-win            nginx.conf       sites-enabled
fastcgi.conf    mime.types         proxy_params     snippets
fastcgi_params  modules-available  scgi_params      uwsgi_params
koi-utf         modules-enabled    sites-available  win-utf

```
1. Dentro de *sites-available copio* el archivo default ```cp default onehtmlpage.babywantsmilk.com ```
2. Después, modifico tanto el *server_name*, con el dominio de la pagina, y *root*, con la ruta del donde estará el html. <br>
```
server {
        listen 80 default_server;
        listen [::]:80 default_server;
        
        ...
        
        root /var/www/baby;
        server_name onehtmlpage.babywantsmilk.com;
        ...
```
3. Ahora en *sites-enabled* creo un link simbolico que apunte a este archivo
```
root@Fran:/etc/nginx/sites-enabled# ln -s ../sites-available/onehtmlpage.babywantsmilk.com  .
root@Fran:/etc/nginx/sites-enabled# ll
total 8
drwxr-xr-x 2 root root 4096 may  3 12:29 ./
drwxr-xr-x 8 root root 4096 may  1 12:22 ../
lrwxrwxrwx 1 root root   34 may  1 12:22 default -> /etc/nginx/sites-available/default
lrwxrwxrwx 1 root root   48 may  3 12:29 onehtmlpage.babywantsmilk.com -> ../sites-available/onehtmlpage.babywantsmilk.com

```
4. Una vez hecho esto, como quiero que esta pagina sea la principal, haré un ```rm default``` tanto en *sites-enabled* como en *sites-available*, ya que aparte de que ya no lo voy a usar para nada, solo se permite 1 solo archivo con la configuracion de ```listen 80 default_server``` .
5. Después de hacer todos estos cambios es recomendable hacer un ```nginx -s reload``` para reiniciar el servidor.

## Creación de la pagina web.
El directorio donde se guardan las paginas es ```/var/www/```. <br>
Aquí crearé el respectivo directorio que puse en el *root* y el html donde copiaré la página de *onehtmlpage*.
```
mkdir baby
cd baby
nano index.html
```

## Redirección del dominio
Finalmente para que el dominio redirija correctamente a la ip local, tendria que añadir el dominio al archivo ```hosts``` dentro del directorio ```/etc/```
```
127.0.0.1       localhost
127.0.1.1       Fran
127.0.0.1       onehtmlpage.babywantsmilk.com
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```
Finalmente, poniendo el dominio en el navegador, muestra correctamente la pagina html
![imagen](https://user-images.githubusercontent.com/91600940/166503671-35f715cc-4402-42af-b6da-7921e2cae675.png)


## Creación segunda pagina
Para esta segunda página consistiria en repetir los mismos pasos que para la página anterior, lo único que habria que cambiar el *root* y el *server_name* con sus respectivos directorio y dominio, además de que esta no tendria que tener la opción de ```default_server```, ya que como he explicado antes, solo se puede tener 1 archivo con esta configuración.
```
server{
        listen 80;
        listen [::]:80;

        ...

        root /var/www/colony;
        server_name onehtmlpage.antcolony.com;
        ...
```
También se añadiria su respectivo link simbolico en *sites-enabled* y sus respectiva carpeta y html.
```
root@Fran:/etc/nginx/sites-enabled# ln -s ../sites-available/onehtmlpage.antcolony.com  .
cd /var/www/
mkdir colony
cd colony/
nano index.html
```
Por último, también habria que añadirlo al ```hosts```.
```
  GNU nano 4.8                        hosts                         Modificado  
127.0.0.1       localhost
127.0.1.1       Fran
127.0.0.1       onehtmlpage.babywantsmilk.com
127.0.0.1       onehtmlpage.antcolony.com

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```
De esta forma ya tendría en funcionamiento las 2 paginas con los diferentes subdominios.
![imagen](https://user-images.githubusercontent.com/91600940/166507495-5bde98b6-faac-45ec-9f33-790d09d72830.png)

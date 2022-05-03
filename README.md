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
...
 root /var/www/baby;

# Add index.php to the list if you are using PHP
index index.html index.htm index.nginx-debian.html;

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
Aqui creare un directorio con el directorio *baby*, mismo nombre que puse en el *root*, para luego crear un html y copiar el contenido ahí dentro.
```
mkdir baby
cd baby
nano index.html
```

# DEPLOY EN A2

## APACHE2
En este punto se supone que se cuenta con un sitio instalado en una cuenta cliente en el VPS de Macher en A2 (__server.macherit.tk - 75.98.172.211__). Supondremos que:
- La URL configurada para el sitio es _usuario\_cliente.com.ar_,
- Que el usuario se llama _usuario_cliente_ y por ende el sitio está en la carpeta /home/usuario_cliente/public_html,

La función del Apache2 es la de servir todos los assets estáticos, es decir, los _.js, .css, .png, .jpg, etc_. En definitiva, todo lo que no provenga de la API, en el esquema actual de los sitios de Macher con el stack MEAN. Por ende, lo configuraremos con las siguientes prestaciones:
- Redirección por defecto hacia la versión HTTPS.
- Aliases hacia las carpetas que estén sirviendo subdominios en ubicaciones no canónicas (donde iría a buscar Apache2 por defecto).
- Compresión de archivos.
- Caché.
- Redirección hacia los endpoints de la API.

### Comentario acerca del VPS
El VPS tiene corriendo un WHM con cPanels para los clientes. Por ende, la configuración de Apache2 y asociados se debe hacer de manera especial por WHM.

Para configurar Apache2 para un el sitio de nuestro cliente en particular, debemos entrar por SSH al VPS como __root__. Luego, crearemos y modificaremos 2 archivos, uno para el acceso sin SSL y otro para el acceso son SSL. Ellos serán, respectivamente __/etc/apache2/conf.d/userdata/std/2_4/usuario\_cliente/usuario\_cliente.com.ar/usuario\_cliente.conf__ (versión sin SSL) y  __/etc/apache2/conf.d/userdata/ssl/2_4/usuario\_cliente/usuario\_cliente.com.ar/usuario\_cliente.conf__ (versión con SSL)

Para aplicar una configuración, deberemos modificar cada uno de estos dos archivos, y luego correr los comandos `/usr/local/cpanel/scripts/rebuildhttpdconf` y `/usr/local/cpanel/scripts/restartsrv_httpd`. El primero reconstruye la configuración de Apache2, y el segundo reinicia el servicio.

### Modificando la configuración de Apache2
El contenido de los 2 archivos recién mencionados debe quedar como sigue:
```
# COMPRESIÓN
<IfModule mod_deflate.c>
    <filesMatch "\.(js|css|html|ttf|otf|eot|woff|woff2)$">
        SetOutputFilter DEFLATE
    </filesMatch>
</IfModule>


# CACHE
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType image/png "access 1 week"
    ExpiresByType image/gif "access 1 week"
    ExpiresByType image/jpeg "access 1 week"
    ExpiresByType application/javascript "modification 1 week"
    ExpiresByType text/css "modification 1 week"
    ExpiresByType text/html "modification 1 week"
    ExpiresByType font/ttf "access 1 month"
    ExpiresByType font/woff "access 1 month"
    ExpiresByType font/woff2 "access 1 month"
    ExpiresByType font/otf "access 1 month"
    #ExpiresDefault "access 2 days"             # Notar que no se asigna un tiempo por defecto, para evitar cachear las llamadas a endpoints de la API.
</IfModule>


# REESCRITURAS
RewriteEngine On

# Si se quisiera debuguear las reescrituras. Comentado en producción.
# LogLevel debug alias:trace6 rewrite:trace6

# Redirección a versión SSL
RewriteCond %{HTTPS} off
RewriteRule .* https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

# Alias a rutas particulares
Alias "/ca"    		    "/home/usuario_cliente/public_html/static/ca"
Alias "/backoffice"     "/home/usuario_cliente/public_html/static/admin"
Alias "/public"    	    "/home/usuario_cliente/public_html/public"
Alias "/node_modules"  	"/home/usuario_cliente/public_html/node_modules"

# Redirección a endpoints de la API
ProxyPreserveHost  On
ProxyPass 	        "/api" 	"http://0.0.0.0:50602/api"
ProxyPassReverse    "/api" 	"http://0.0.0.0:50602/api"


# Path a la carpeta root del sitio
Alias "/"    		"/home/usuario_cliente/public_html/static/site/"
```
donde __50602__ es el puerto en el que está escuchando _Express_, para atender los llamados a la API.

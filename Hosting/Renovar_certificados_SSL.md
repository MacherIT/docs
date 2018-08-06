# Certificados SSL

El cPanel renueva los certificados automáticamente, pero a veces éstos no se pueden renovar por la configuración que le estamos imponiendo al Apache.

En esos casos, hay que mover temporalmente el archivo de configuración, reiniciar el Apache, lanzar el comando de renovación de certificados, y luego restaurar la configuración inicial del Apache. Por ejemplo, para APDP:

```
[root@server:/root]$ cd /etc/apache2/conf.d
[root@server:conf.d]$ mv userdata/ssl/2_4/apdp/apdp.com.ar/apdp.conf userdata/ssl/2_4/apdp/apdp.com.ar/apdp.conf.stop
[root@server:conf.d]$ /usr/local/cpanel/scripts/rebuildhttpdconf
[root@server:conf.d]$ /usr/local/cpanel/scripts/restartsrv_httpd
[root@server:conf.d]$ /usr/local/cpanel/bin/autossl_check --user=apdp
[root@server:conf.d]$ mv userdata/ssl/2_4/apdp/apdp.com.ar/apdp.conf.stop userdata/ssl/2_4/apdp/apdp.com.ar/apdp.conf
[root@server:conf.d]$ /usr/local/cpanel/scripts/rebuildhttpdconf
[root@server:conf.d]$ /usr/local/cpanel/scripts/restartsrv_httpd
```

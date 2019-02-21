# Configuración de Emails en servidor de A2 con cPanel

## Emails a través de un servidor externo

Si se va a utilizar un servidor remoto para la recepción y envío de los emails del dominio de un cliente, se debe realizar lo siguiente:

1. Crear las entradas DNS (desde **Zone Editor**) correspondientes a los subominios utilizados por los clientes de correo para acceder al servidor remoto, en el servidor local. Estos suelen ser smpt.dominio-del-cliente.com, pop.dominio-del-cliente.com, mail.dominio-del-cliente.com, etc.
2. Configurar el **Enrutamiento de correo electrónico** a _Intercambiador de correo remoto_. Usualmente, las cuentas se configuran para una detección automática, lo cual por defecto configura al servidor para utilizar el servidor de correo local.

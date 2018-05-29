# Analitica en Sitios MEAN
## Facebook Pixel
- Crear un nuevo pixel en Facebook Pixel (ej.: Hanomag)
- Crear un 'ad account' asociada al pixel
- Asignar la 'ad account' al pixel
- Añadir snippet de conexion al index del sitio dentro de la etiqueta <head>
```
// Facebook Pixel Code
  script.
    !function(f,b,e,v,n,t,s)
    {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
    n.callMethod.apply(n,arguments):n.queue.push(arguments)};
    if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
    n.queue=[];t=b.createElement(e);t.async=!0;
    t.src=v;s=b.getElementsByTagName(e)[0];
    s.parentNode.insertBefore(t,s)}(window, document,'script',
    'https://connect.facebook.net/en_US/fbevents.js');
    fbq('init', '<id del pixel>');
    fbq('track', 'PageView');
  noscript
    img(height='1', width='1', style='display:none', src='https://www.facebook.com/tr?id=<id del pixel>&ev=PageView&noscript=1')
  // End Facebook Pixel Code
```

- Definir que *páginas* se quieren trackear

```
// Utilizar el siguiente código dentro de los controladores de la página/modulo correspondiente

fbq("track", "ViewContent", {
	content_ids: `<etiqueta identificadora>`
});
```

- Crear en el dashboard de pixel el público relacionado con dichas páginas
- Definir que *eventos* se quieren trackear

```
// Utilizar el siguiente código dentro de los controladores de la página/modulo correspondiente

fbq("track", "AddToWishlist", {
content_ids: `<etiqueta identificadora>`
});
```

- Crear en el dashboard de pixel el público relacionado con dichos eventos
*Recordar: Facebook no funciona en tiempo real, por ende, para poder crear los públicos correctamente, se debe ejecutar al menos 1 vez cada evento definido para que facebook pueda detectarlo*


## Google Analytics
- Crear la propiedad en Google Analytics para obtener el identificador, ej.: 'UA-110796994-1'
- Instalar el paquete de node
```
yarn add / npm install angular-google-analytics
```

- Añadir el siguiente código al app.js general

```
app.use(
"/vendor/angular-google-analytics",
serveStatic(
path.join(__dirname, "node_modules/angular-google-analytics/dist")
)
);
```

- Añadir la siguiente linea al final del index del sitio (previo a la llamada a app.min.js)

```
script(src='/vendor/angular-google-analytics/angular-google-analytics.min.js', charset='utf-8')
```

- Añadir el siguiente código en app/site/src/js/main.js
```
.module("MainApp", [
//...
"angular-google-analytics",
//...
])
.config(AnalyticsProvider => {
	// Add configuration code as desired
	AnalyticsProvider.setAccount("<identificador de google analytics>");
})
```

- Añadir el siguiente atributo a las etiquetas sobre las que se desee realizar un trackeo
```
ga-track-event="['<identificador>', '<evento>', '<descripcion>']"
```
ej:
```
ga-track-event="['menu-lateral-web', 'click', 'Click en Alcance']"
```

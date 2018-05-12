
# Instrucciones para empezar con un proyecto de Rails 5.2.

- Inicializar el repo y configurarlo con el origin correspondiente (ya se sabe cómo).
- Crear la app de Rails
```
rails new . --database=postgresql
```

- Crear la base de datos
```
createdb <DBname>
```

- Configurar _config/database.yml_ para usar la DB que acabamos de crear:

```
default: &default
  adapter: postgresql
  encoding: utf8
  # For details on connection pooling, see Rails configuration guide
  # http://guides.rubyonrails.org/configuring.html#database-pooling
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  database: <DBname>
  username: <username>
  password: <password>

development:
  <<: *default
```

- Agregar las gems más usadas al Gemfile. Un ejemplo de algunas sería:

```
gem 'devise'
gem 'jquery-rails'
gem 'bootstrap-sass', '~> 3.3.7'
gem "haml"
gem 'bootstrap_form'

gem 'factory_bot_rails', group: [:test, :development]
gem 'faker', group: [:test, :development]
```

- Instalar usando bundler

```
bundle install
```

- Si los usamos, debemos importar los archivos correspondientes de Bootstrap y Boostrap Form en los archivos correspondientes de nuestra app. En _app/assets/javascript/application.js_:
```
//= require rails-ujs
//= require turbolinks
//= require_tree .
//= require jquery
//= require bootstrap-sprockets
//= require cocoon
```

- Además de agregar las líneas correspondientes, debemos renombrar este archivo para que sea .scss. En _app/assets/javascript/application.scss_:
```
 *= require_tree .
 *= require_self
 *= require rails_bootstrap_forms
 */

 // Custom bootstrap variables must be set or imported *before* bootstrap.
@import "bootstrap-sprockets";
@import "bootstrap";
```
En caso de que se tuviera un server corriendo, se lo deberá reiniciar para que estos cambien surtan efecto.

- Ahora podemos hacer un commit con los contenidos del proyecto
```
git add -A
git commit -m "feat(init): Commit inicial."
```


## Modelo de usuarios con Devise
- Instalamos Devise
```
rails generate devise:install
```
Luego veremos ciertas instrucciones que aparecen en pantalla, entre las cuales se pide que hagamos lo siguiente.
- Configuramos el mailer según requerimiento de Devise. En _config/environments/development.rb_
```
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

- Nos aseguramos de que haya una ruta a la homepage, En _routes.rb_
```
 root to: "inicio#index"
```

- Y ya que acabamos de hacer esto, crearemos el controlador con las vistas necesarias para que esta página exista. En _config/routes.rb_:
```
rails g controller Inicio index
```

- Siguiendo con la configuración para Devise, incluimos las notificaciones y flashes en todas las páginas (dentro del body) en _config/routes.rb_:
```
      <!-- Notificaciones de Devise -->
      <% if !notice.nil? %>
        <p class="alert alert-success"><%= notice %></p>
      <% end %>
      <% if !alert.nil? %>
        <p class="alert alert-danger"><%= alert %></p>
      <% end %>
```

- Copiamos las vistas de Devise, para configurarlas
```
rails g devise:views
```

- Creamos un modelo de usuario desde la línea de comando
```
rails generate devise <MODELO_USUARIO>
```
donde <MODELO_USUARIO> es el nombre del modelo a crear, como por ejemplo user.

- Devise provee muchas opciones para el modelo de usuario. Están todas escritas en _models/<MODELO_USUARIO>.rb_. Algunas de ellas están comentadas. Basándonos en la documentación, debemos seleccionar cuáles dejar sin comentar. Suponemos que además de las que vienen por defecto configuradas, incluiremos (o des-comentaremos) __:confirmable__ en _models/user.rb (models/<MODELO_USUARIO>.rb)_. Además, debemos des-comentar las líneas correspondientes a la propiedad __confirmable__ en db/migrate/<num_migracion>_devise_create_<MODELO_USUARIO>.rb.

- En el archivo de la migración, también podemos agregar las líneas que deseemos para el resto de los atributos que queramos darle al usuario (como por ejemplo, nombre, dirección, etc.). También se pueden agregar nuevos índices (al final del archivo) o descomentar los que correspondan.

- Posteriormente debemos hacer la migración.
```
rails db:migrate
```

- Aunque todavía no lo vayamos a utilizar, podemos aprovechar para crear los archivos para el seed de la DB, para lo que utilizaremos FactoryBot. En _test/factories/user.rb_:
```
FactoryBot.define do
  factory :user do
    nombre {Faker::FamilyGuy.character} # Suponiendo que le creamos un atributo llamado 'nombre'
    email {Faker::Internet.email}
    password Faker::Internet.password
    confirmed_at {Time.now - 1.days} # Para que los cree ya confirmados
  end
end
```
Nótese que incluimos una línea para un atributo nombre.

- Muy probablemente vayamos a querer editar las vistas relacionadas con los usuarios (para editarlos, crearlos, loguearse, etc.). Para esto debemos copiar las vistas de Devise al proyecto actual.
```
rails generate devise:views
```

## Producción
No es un mal momento para configurar desde este momento un ambiente de producción (aunque no vaya a ser el final) para detectar posibles problemas desde una etapa temprana, y ¿por qué no? para llevar una práctica de Integración Continua desde el comienzo del proyecto.

### Heroku
Vamos a usar Heroku como ambiente de producción.

- Primero hay que instalar Heroku, lo cual vamos a asumir que ya se hizo, siguiendo la documentación oficial.

- Después hay que loguearse y registrar las claves públicas locales en Heroku.
```
heroku login
heroku keys:add
```

- Creamos una aplicación desde la CLI, y le seteamos el nombre deseado (que supongamos que va a ser mi-app)
```
heroku create
heroku rename mi-app
```

- Después pusheamos a Heroku.
```
git push heroku master
```

- Una vez hecho el push, Heroku instala las dependencias y levanta la app en la URL correspondiente (https://mi-app.herokuapp.com en este caso).

- Ahora debemos configurar el mail para producción (para development ya está configurada desde que instalamos Devise). En _config/environments/production.rb_
```
config.action_mailer.default_url_options = { host: 'https://<nombre de la app>.herokuapp.com' }
```

#### Emails
- Usarmos Gmail para development y Sendgrid para producción. La integración de este último con Heroku es muy simple y se logra con:
```
heroku addons:create sendgrid:starter
```
Esto nos guardará el usuario y la contraseña de Sendgrid en variables de entorno en Heroku.

- Y tenemos que crear un archivo con la configuración de los servicios de mail, _config/initializers/email\_setup.rb_

```
if Rails.env.development?

ActionMailer::Base.delivery_method = :smtp
ActionMailer::Base.smtp_settings = {
  address:              'smtp.gmail.com',
  port:                 587,
  domain:               '<nombre de la app>.herokuapp.com',
  user_name:            ENV["GMAIL_USERNAME"],
  password:             ENV["GMAIL_PASSWORD"],
  authentication:       'plain',
  enable_starttls_auto: true
}

elsif Rails.env.production?

ActionMailer::Base.delivery_method = :smtp
ActionMailer::Base.smtp_settings = {
  address:              'smtp.sendgrid.net',
  port:                 587,
  domain:               'heroku.com',
  user_name:            ENV["SENDGRID_USERNAME"],
  password:             ENV["SENDGRID_PASSWORD"],
  authentication:       'plain',
  enable_starttls_auto: true
}

end
```

- En Gmail deberemos permitir el uso de aplicaciones menos seguras desde la configuración de Gmail. Si no lo hacemos previamente, la primera vez que intentemos enviar emails nos llegará a la casilla desde la cual intentamos enviarlos (y sus asociadas) un email notificándonos al respecto, desde donde se puede acceder a dicha configuración.

- Ahora falta cambiar la dirección desde la que queremos que aparezcan los emails como enviados. En _config/initializers/devise.rb_
```
config.mailer_sender = '<usuario>@<dominio>.com'
```

- Ya tendremos lista toda la configuración. Ahora podemos commitear, pushear y hacer la migración en el servidor de producción.
```
git ci -am "feat(emails): Configuramos usuarios y mails."
git push
git push heroku master
heroku run rails db:migrate
```

- Si queremos hacer un seed de la base, debemos usar la gem Faker también en producción (cambiar en el Gemfile). Después podemos hacer un seed.
```
heroku run rails db:seed
```

- Y en caso de querer hacer un reset de la base:
```
heroku pg:reset DATABASE --confirm <nombre de la app>
heroku run rails db:migrate
heroku run rails db:seed
```

#### Assets en producción con Heroku
##### Fonts
Para que Heroku precompile las fuentes que usamos, debemos incluirlas en el scss como se muestra a continuación (obsérvese que se usa la directiva font-url):
```
@font-face {
	font-family: "foundation-icons";
	src: font-url( asset-path("foundation-icons.eot") );
	src: font-url( asset-path("foundation-icons.eot?#iefix") ) format("embedded-opentype"),
		font-url( asset-path("foundation-icons.woff") ) format("woff"),
		font-url( asset-path("foundation-icons.ttf") ) format("truetype"),
		font-url( asset-path("foundation-icons.svg#fontcustom") ) format("svg");
	font-weight: normal;
	font-style: normal;
}
```

- Además, en deberemos agregar a la configuración que se precompilen los tipos de archivos correspondientes a las fuentes. En _application.rb_
```
config.assets.precompile << /\.(?:svg|eot|woff|ttf)$/
```

## Seed de la base de datos
Una práctica necesaria es crear ir populando la base de datos para tener información con la que ir testeando la aplicación. A esto se lo denomina hacer un seed de la base de datos. Para este fin tenemos los archivos de la carpeta test/factories, donde por lo general tendremos uno por modelo declarado, como ya vimos con el modelo de usuario.

- El archivo _db/seeds.rb_ es el que orquestrará la populación de la base de datos. Para comenzar con la generación de algunos usuarios podemos armar un plan como el siguiente:

```
p "Creando usuario Yoda con pass the-force"
FactoryBot.create :user, nombre: "Yoda", email: "yoda@yedi.com", password: "the-force"

p "Creando usuarios"
3.times do |u|
  FactoryBot.create :user
end
```

- Usando _test/factories/user.rb_ que ya tenemos creado, el seed se puede hacer mediante:
```
rails db:seed
```
Y para hacerlo en el servidor de producción podemos hacer:
```
heroku pg:reset DATABASE
heroku run rails db:migrate
heroku run rails db:seed
```

## Conclusión
Ahora ya tenemos un sistema con usuarios con todas las funcionalidades básicas para manejo de registraciones, sesiones, confirmaciones, etc. Además, hemos montado un entorno de desarrollo y otro de producción, cada uno con sus configuraciones correspondientes. Estamos listos para seguir adelante con la implementación de especificaciones más específicas del proyecto en particular.

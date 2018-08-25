A partir de Rails 5.2 se incorporó ActiveStorage como parte del core de Rails, que permite alojar los archivos tanto en el disco local (bueno para development, pero no siempre para producción), como en AWS S3 y otros servicios en la nube.

# Heroku (Bucketeer, AWS S3)
Como los dynos de Heroku no tienen almacenamiento duradero (se borra todo el almacenamiento ante nuevos deploys), se almacenan los assets en S3. La forma más sencilla de lograr esto es configurar un add-on de Heroku llamado [Bucketeer](https://devcenter.heroku.com/articles/bucketeer#using-with-ruby-rails).

## Bucketeer
Se puede agregar el add-on desde la CLI de Heroku:
```
heroku addons:create bucketeer:hobbyist
```
Después de esto hay que configurar la instancia de S3 para que acepte CORS. Para ello se necesita tener instalado en la máquina local la [CLI de AWS](https://aws.amazon.com/es/cli/). Primero configuramos las AWS CLI:
```
aws configure
```
Acto seguido, introducimos las credenciales que podemos ver con ```heroku config | grep BUCKETEER```.
Después creamos un archivo con el contenido de __.s3_cors_config.json__ de este mismo repositorio.
Luego, subimos dicho archivo a S3 usando el CLI:
```
aws s3api put-bucket-cors --bucket bucketeer-4cfae2bd-c107-4ef1-8b72-1f3d7556ae0b --cors-configuration file://.s3_cors_config.json
```
cambiando el ID del bucket correspondientemente, que es el
**BUCKETEER_BUCKET_NAME** de Heroku.

Además, debemos agregar la configuración de Amazon a **config/storage.yml**:
```
amazon:
  service: S3
  access_key_id: <%= ENV['BUCKETEER_AWS_ACCESS_KEY_ID'] %>
  secret_access_key: <%= ENV['BUCKETEER_AWS_SECRET_ACCESS_KEY'] %>
  region: <%= ENV['BUCKETEER_AWS_REGION'] %>
  bucket: <%= ENV['BUCKETEER_BUCKET_NAME'] %>
```

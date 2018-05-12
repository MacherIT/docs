# Problemas con las bases de datos y Rails

Rails se detiene al iniciar cuando se ejecutan muchas consultas simultaneas, se produce un Hard Lock cuando hay una Condicion Racing en la tabla de migraciones.

Por lo general se detecta cuando la consola imprime varias lineas como esta: SELECT "schema_migrations"."version" FROM "schema_migrations" ORDER BY "schema_migrations"."version" ASC

Posiblemente este problema solo ocurra en testing, aunque es posible colocar una _Gem_ para hacer __autovacuum__.

## Step-by-step guide
### Solución manual
- Ingresar a la consola SQL y seleccionar en la base de datos.
- Ejecutar simplemente:```VACUUM;```

### Solución automática
- Para habilitar el Auto VACUUM:
```
ALTER TABLE schema_migrations SET (autovacuum_enabled = true);
```

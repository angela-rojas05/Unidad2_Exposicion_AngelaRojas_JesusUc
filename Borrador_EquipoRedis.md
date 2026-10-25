# Redis: caché en memoria para una API

**Materia:** Fundamentos de Arquitectura de Software y Desarrollo Backend  
**Unidad:** II — Exposición: una herramienta de backend  
**Equipo:** Jesús Uc y Ángela Rojas  
**Tecnología elegida:** Redis

## 1. ¿Qué es Redis?

Redis es una herramienta que permite guardar y consultar datos rápidamente usando principalmente la memoria. En un backend se puede utilizar, por ejemplo, para conservar temporalmente resultados que se consultan muchas veces, evitando repetir constantemente la misma consulta a la base de datos.

Redis no tiene que reemplazar a MySQL. En muchos sistemas se usan juntos: MySQL conserva los datos principales y Redis guarda temporalmente algunos resultados que conviene recuperar con rapidez.

## 2. ¿Qué problema resuelve?

Cuando una API recibe muchas solicitudes iguales, puede terminar consultando la base de datos una y otra vez para devolver la misma información. Esto aumenta el trabajo de la base de datos y puede hacer que las respuestas tarden más.

Redis puede ayudar guardando temporalmente el resultado de una consulta. Si llega otra solicitud mientras ese dato sigue disponible en la caché, la API puede devolverlo desde Redis sin volver a consultar MySQL. Para que los datos no permanezcan desactualizados indefinidamente, normalmente se define un tiempo de expiración o una estrategia para actualizar o invalidar la caché.

Redis también puede utilizarse para otros casos, como contadores, sesiones o colas sencillas de trabajos, dependiendo de las necesidades del sistema.

## 3. ¿Cómo se utiliza? (pasos generales)

1. Un cliente envía una petición a la API, por ejemplo, para consultar los productos más vendidos.
2. La API busca en Redis una copia temporal del resultado.
3. Si el resultado existe y todavía es válido, la API lo devuelve.
4. Si no existe, la API consulta la información en MySQL.
5. La API guarda el resultado en Redis durante un periodo definido y después devuelve la respuesta al cliente.
6. Cuando los datos originales cambian, el sistema debe actualizar o invalidar la caché para reducir el riesgo de mostrar información antigua.

Este flujo es un ejemplo de uso de caché; la implementación exacta depende de la arquitectura de la aplicación.

## 4. Relación y diferencias con MySQL y Docker

**Redis y MySQL:** MySQL suele utilizarse como base de datos principal para conservar información de manera estructurada y duradera. Redis se utiliza con frecuencia para acceder rápidamente a datos temporales o a resultados que se consultan repetidamente. No son herramientas idénticas ni necesariamente compiten entre sí: pueden complementarse.

**Redis y Docker:** Redis es el servicio que almacena y entrega los datos; Docker permite empaquetar y ejecutar servicios en contenedores. Por ejemplo, un equipo podría ejecutar Redis en un contenedor Docker para facilitar su configuración y desarrollo. Docker no sustituye a Redis, ni Redis sustituye a Docker.

## 5. Ejemplo concreto relacionado con una API

Imaginemos una API de catálogo que tiene un endpoint `GET /productos/populares`. Cada vez que alguien abre esa sección, la API necesita obtener la lista de productos más consultados.

Sin caché, la API podría consultar MySQL en cada petición. Con Redis, el flujo sería:

- La API busca la clave `productos:populares` en Redis.
- Si encuentra una lista vigente, la devuelve.
- Si no la encuentra, consulta MySQL, obtiene la lista y la guarda en Redis, por ejemplo, durante 60 segundos.
- Las siguientes peticiones dentro de ese periodo pueden reutilizar el resultado guardado.
- Si cambian los productos o sus datos relevantes, la aplicación debe decidir si elimina o renueva esa clave antes de que expire.

El tiempo de 60 segundos es solo un valor ilustrativo, no una regla fija. En un sistema real se elegiría según la frecuencia con que cambian los datos y cuánto tiempo se puede aceptar que la información esté desactualizada.

## 6. Ventajas y limitaciones

### Ventajas

- Puede reducir el tiempo de respuesta en consultas repetidas.
- Puede disminuir la cantidad de consultas que recibe la base de datos principal.
- Permite guardar datos temporales útiles, como contadores o sesiones.
- Puede integrarse con una API existente sin reemplazar necesariamente su base de datos.

### Limitaciones

- La caché ocupa memoria, que puede ser un recurso limitado o costoso.
- Si no se actualiza o invalida correctamente, puede devolver datos desactualizados.
- Agrega otra pieza que configurar, monitorear y mantener.
- No todas las consultas se benefician de una caché; si los datos cambian constantemente o casi nunca se repiten las peticiones, el beneficio puede ser pequeño.

## 7. ¿Cuándo conviene usar Redis y cuándo no?

Conviene considerar Redis cuando hay información que se consulta con frecuencia, el resultado puede reutilizarse durante un tiempo y reducir la latencia o la carga de la base de datos representa una mejora útil.

No siempre conviene incorporarlo en una aplicación pequeña o con pocas consultas repetidas, especialmente si la complejidad adicional no aporta una mejora apreciable. También hay que evitar depender de la caché como única fuente de datos importantes sin evaluar las necesidades de persistencia y recuperación del sistema.

## 8. Apartados previstos para la exposición

1. Definición de Redis y propósito.
2. Problema que resuelve en un backend.
3. Componentes y flujo básico de una petición.
4. Comparación con MySQL y relación con Docker.
5. Ejemplo de una API que consulta productos populares.
6. Ventajas, limitaciones y casos en los que no conviene utilizarlo.
7. Conclusiones y fuentes consultadas.

## 9. Fuentes iniciales por consultar

- ¿Qué es Redis? | IBM: https://www.ibm.com/mx-es/think/topics/redis
- Redis | AWS: https://aws.amazon.com/es/elasticache/what-is-redis/
- Base de datos de Redis | Google Cloud: https://cloud.google.com/discover/what-is-redis-database?hl=es-419

#### Fuentes oficiales

- Redis Documentation, documentación oficial: https://redis.io/docs/latest/
- Redis, sitio oficial y descripción general: https://redis.io/
- Docker Docs, documentación oficial sobre contenedores: https://docs.docker.com/



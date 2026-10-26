
# Redis: caché en memoria para una API

Material de la exposición de la **Unidad II: Exposición de una herramienta de backend**, para la materia **Fundamentos de Arquitectura de Software y Desarrollo Backend**.

## Información general

**Integrantes:**
- Jesús Omar Uc Domínguez
- Ángela Yaritzi Rojas Brito
- Angel Abraham Lugo Saenz

**Grupo:** 4-B

**Institución:** Instituto Tecnológico de Software

**Tecnología:** Redis

---

## 1. Descripción

Este trabajo explica qué es Redis, cómo puede utilizarse como caché en memoria dentro de una aplicación backend y cómo puede complementar a MySQL para evitar consultas repetidas.

La exposición utiliza como ejemplo una API de catálogo que devuelve los productos más populares.

La primera vez, la API consulta la base de datos y guarda temporalmente el resultado en Redis. Las peticiones posteriores pueden reutilizarlo mientras siga vigente.

---

## 2. Objetivos

- Explicar el propósito de Redis y el problema que ayuda a resolver.
- Describir el flujo de una petición cuando se utiliza una caché.
- Relacionar Redis con una API, MySQL y Docker.
- Identificar sus ventajas, limitaciones y situaciones en las que puede convenir utilizarlo.

---

## 3. Contenido del repositorio

El repositorio contiene los siguientes archivos:

### 3.1. Borrador de investigación

**Archivo:** [Borrador_EquipoRedis.md](./Borrador_EquipoRedis.md)

Contiene el borrador inicial con los apartados principales de la investigación.

### 3.2. Reporte escrito

**Archivo:** [Reporte_EquipoRedis_AngelayJesus.pdf](./Reporte_EquipoRedis_AngelayJesus.pdf)

Contiene el reporte escrito donde se desarrolla el tema de Redis.

### 3.3. Diapositivas de la exposición

**Archivo:** [Diapositivas_EquipoRedis.pdf](./Diapositivas_EquipoRedis.pdf)

Presentación utilizada como material de apoyo para la exposición.

---

## 4. Ejemplo de funcionamiento

El caso de uso utiliza los siguientes elementos:

**Endpoint:**
`GET /productos/populares`

**Clave de caché:**
`productos:populares`

### 4.1. Flujo de una petición

1. Un cliente solicita la lista de productos a la API.

2. La API busca la clave `productos:populares` en Redis.

3. Si encuentra la información y sigue vigente (*cache hit*), reutiliza el resultado.

4. Si no encuentra la información (*cache miss*), consulta MySQL y obtiene los datos originales.

5. La API guarda temporalmente el resultado en Redis y responde al cliente.

6. Si los datos cambian, la aplicación debe actualizar o invalidar la caché cuando corresponda.

### 4.2. Tiempo de vida de la caché (TTL)

En las diapositivas se utiliza un TTL de **60 segundos**, únicamente como ejemplo ilustrativo.

En una aplicación real, el periodo se define según la frecuencia con la que cambian los datos y cuánto tiempo se puede aceptar que la información permanezca en caché.

> **Nota:** El ejemplo es conceptual y sirve para explicar el funcionamiento de Redis. Este repositorio no incluye una implementación funcional de la API.

---

## 5. Conceptos clave

| Término | Definición |
|---|---|
| **Redis** | Almacén de datos en memoria que puede utilizarse como caché, entre otros usos. |
| **Caché** | Copia temporal de información que puede reutilizarse para evitar trabajo repetido. |
| **MySQL** | Sistema de gestión de bases de datos relacionales que puede conservar los datos principales de la aplicación. |
| **Cache hit** | El dato solicitado está disponible en la caché. |
| **Cache miss** | El dato no está disponible en la caché y debe buscarse en la fuente original. |
| **TTL** | Tiempo de vida configurado para una entrada de caché antes de que expire. |
| **Invalidación de caché** | Eliminación o actualización de una entrada que ya no debe utilizarse. |
| **Docker** | Plataforma para ejecutar aplicaciones y servicios en contenedores; puede utilizarse para ejecutar Redis. |

---

## 6. Ventajas y limitaciones

### 6.1. Ventajas

- Puede acelerar el acceso a resultados consultados con frecuencia.
- Puede reducir algunas consultas repetidas a la base de datos principal.
- También puede utilizarse para casos como sesiones y contadores.

### 6.2. Limitaciones

- Los datos en caché consumen memoria.
- Una caché mal gestionada puede devolver información desactualizada.
- Añade configuración, monitoreo y mantenimiento.
- No todas las aplicaciones o consultas obtienen un beneficio significativo.

---

## 7. Fuentes de información

Para la investigación se utilizaron las siguientes fuentes oficiales:

1. [Redis Documentation](https://redis.io/docs/latest/)
2. [Sitio oficial de Redis](https://redis.io/)
3. [Docker Docs](https://docs.docker.com/)

---

## 8. Propósito académico

Este repositorio reúne los materiales de apoyo para la exposición de la Unidad II.

La presentación resume los puntos principales, mientras que el borrador y el reporte desarrollan la explicación con mayor detalle.

El propósito es comprender cómo Redis puede utilizarse como herramienta de backend para mejorar el acceso a información consultada frecuentemente.

# Gestor de incidencias - Docker Compose

## 1. Descripción del proyecto

Despliegue de una aplicación web de gestión de incidencias
mediante Docker Compose.

La arquitectura está formada por tres servicios:

- Frontend: interfaz web con Nginx.
- Backend: API de la aplicación.
- DB: base de datos PostgreSQL.

## 2. Arquitectura

Se utilizan dos redes:

- frontend: conecta frontend y backend.
- datos: conecta backend y PostgreSQL.

El backend pertenece a ambas redes.

Se utiliza un volumen llamado postgres_data para
conservar los datos de PostgreSQL.

## 3. Requisitos

- Docker Desktop.
- Docker Compose.
- Imágenes compatibles del frontend y backend.

## 4. Instalación

Clonar el repositorio y crear el archivo de configuración:

    cp .env.sample .env

Comprobar la configuración:

    docker compose config

Descargar las imágenes:

    docker compose pull

Arrancar la aplicación:

    docker compose up -d

Acceder desde el navegador:

    http://localhost:8080

## 5. Administración

Consultar servicios:

    docker compose ps

Consultar registros:

    docker compose logs

Detener servicios:

    docker compose stop

Arrancar servicios:

    docker compose start

Eliminar contenedores y redes:

    docker compose down

IMPORTANTE: no utilizar docker compose down -v
si queremos conservar los datos.

## 6. Preguntas del laboratorio

### 1. ¿Por qué el frontend no necesita conectarse a PostgreSQL?

Porque el frontend solamente muestra la interfaz al usuario.
Cuando necesita consultar o guardar información, envía
las peticiones al backend, que se comunica con la base
de datos.

### 2. ¿Por qué el backend pertenece a dos redes?

Porque necesita comunicarse con el frontend para recibir
peticiones y con PostgreSQL para consultar y guardar datos.

### 3. ¿Qué servicio necesita publicar un puerto en el anfitrión?

El frontend, porque necesitamos acceder a la aplicación
desde el navegador mediante localhost:8080.

### 4. ¿Por qué publicar PostgreSQL hacia fuera no es la solución
adecuada para un fallo de comunicación interno?

Porque los servicios Docker pueden comunicarse mediante
sus redes internas y nombres de servicio. Publicar PostgreSQL
no soluciona una configuración incorrecta de las redes
y expone innecesariamente la base de datos.

### 5. ¿Qué ventaja ofrece Docker Compose respecto a ejecutar
los tres contenedores manualmente?

Permite definir toda la aplicación en un único archivo,
incluyendo servicios, redes, volúmenes y dependencias.
Así podemos administrar los componentes con pocos comandos.

### 6. ¿Por qué hemos dividido la arquitectura en dos redes?

Para separar las comunicaciones entre los servicios.
El frontend no necesita acceder directamente a PostgreSQL.
Esto mejora el aislamiento y reduce las conexiones innecesarias.

### 7. ¿Por qué el backend utiliza db como dirección de PostgreSQL?

Porque db es el nombre del servicio definido en Compose.
Docker proporciona resolución DNS interna entre servicios
que comparten una red. No necesitamos utilizar una IP fija
ni localhost.

### 8. ¿Qué diferencia existe entre detener un proyecto
y eliminar sus volúmenes?

docker compose stop detiene los contenedores, pero los conserva.

docker compose down elimina los contenedores y las redes,
pero conserva por defecto los volúmenes con nombre.

docker compose down -v también elimina los volúmenes
y puede provocar la pérdida de los datos.

### 9. ¿Qué consecuencias tendría para la empresa perder
el volumen de PostgreSQL?

Podrían perderse todas las incidencias y demás información
almacenada en la base de datos. Esto afectaría al trabajo
diario de la empresa y podría provocar una pérdida importante
de información.

Por eso es necesario utilizar almacenamiento persistente
y realizar copias de seguridad periódicas.

## 7. Comprobaciones realizadas

- PostgreSQL iniciado correctamente.
- Healthcheck de PostgreSQL configurado.
- Volumen postgres_data creado.
- Tabla de prueba creada.
- Registro persistente comprobado después de recrear
  el contenedor.

Nota: el despliegue completo del frontend y backend queda
pendiente de disponer de imágenes compatibles con AMD64.

# Proyecto WordPress con Docker

Este proyecto permite desplegar un sitio WordPress con Docker, usando una base de datos MySQL. La configuración incluye una red personalizada con direcciones IP estáticas y volúmenes persistentes.

## Pre-requisitos

- Tener instalado [Docker](https://www.docker.com/get-started) y [Docker Compose](https://docs.docker.com/compose/install/).
- Tener una cuenta en [GitHub](https://github.com/).

## Despliegue Local

1. **Configurar una red Docker personalizada:**
    ```bash
    docker network create --subnet=172.18.0.0/16 mynet
    ```

2. **Configurar volúmenes persistentes para WordPress y MySQL:**
    ```bash
    docker volume create wp_data
    docker volume create mysql_data
    ```

3. **Lanzar el contenedor MySQL:**
    ```bash
    docker run --name mysql-container \
      --network=mynet \
      --ip=172.18.0.2 \
      -v mysql_data:/var/lib/mysql \
      -e MYSQL_ROOT_PASSWORD=root_password \
      -e MYSQL_DATABASE=wordpress \
      -e MYSQL_USER=wp_user \
      -e MYSQL_PASSWORD=wp_password \
      -d mysql:5.7
    ```

4. **Lanzar el contenedor WordPress:**
    ```bash
    docker run --name wordpress-container \
      --network=mynet \
      --ip=172.18.0.3 \
      -v wp_data:/var/www/html \
      -e WORDPRESS_DB_HOST=172.18.0.2:3306 \
      -e WORDPRESS_DB_USER=wp_user \
      -e WORDPRESS_DB_PASSWORD=wp_password \
      -d wordpress:latest
    ```
¡Gracias por usar este proyecto!

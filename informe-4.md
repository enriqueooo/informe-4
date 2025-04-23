# Práctica: Creación de contenedores MySQL y phpMyAdmin en Docker

## 1. Título  
**Creación de contenedores MySQL y phpMyAdmin utilizando Docker para la administración de bases de datos**

## 2. Tiempo de duración  
**60 minutos**

## 3. Fundamentos  

En esta práctica, aprenderé a crear y configurar contenedores Docker para gestionar bases de datos MySQL y administrarlas mediante la herramienta phpMyAdmin. Docker facilita la creación de entornos de trabajo portables y consistentes, y es ideal para simular servicios de bases de datos sin necesidad de instalaciones complejas en el sistema operativo anfitrión.

MySQL es un sistema de gestión de bases de datos relacional ampliamente utilizado. phpMyAdmin es una aplicación web que permite administrar MySQL desde un navegador de forma gráfica.

El objetivo es montar un entorno con dos contenedores: uno para MySQL y otro para phpMyAdmin, conectándolos a través de una red personalizada en Docker para que se comuniquen correctamente.

### Imagen relacionada con la teoría:
![Contenedor Docker con PostgreSQL](docker/8.png)  
*Figura 1-1. Volumenes.*

## 4. Conocimientos previos

Para realizar esta práctica, se deben tener nociones sobre:

- Fundamentos de Docker y contenedores.
- Comandos básicos de terminal.
- Uso de bases de datos MySQL.
- Concepto de redes internas en Docker.
- Configuración de variables de entorno para servicios en contenedores.

## 5. Objetivos a alcanzar

- Crear un contenedor de base de datos MySQL con credenciales personalizadas.
- Crear un contenedor de phpMyAdmin conectado a MySQL.
- Configurar una red personalizada para permitir la comunicación entre ambos contenedores.
- Acceder a phpMyAdmin desde el navegador y gestionar una base de datos de prueba.

## 6. Equipo necesario

- Un computador con **Docker** instalado o acceso a un entorno como **Docker Playground**.
- Conexión a internet.
- Navegador web para acceder a phpMyAdmin.

## 7. Material de apoyo

- Documentación oficial de Docker: [https://docs.docker.com/](https://docs.docker.com/)
- Documentación oficial de MySQL: [https://dev.mysql.com/doc/](https://dev.mysql.com/doc/)
- Documentación de phpMyAdmin: [https://www.phpmyadmin.net/docs/](https://www.phpmyadmin.net/docs/)

## 8. Procedimiento

### Parte 1: Creación del entorno de red y contenedores

**Paso 1: Crear una red personalizada en Docker**

```bash
docker network create mysql-phpmyadmin-net

```
![Contenedor Docker con PostgreSQL](docker/1.png)  

### Paso 2: Crear el contenedor de MySQL

Ejecuta el siguiente comando para crear el contenedor de MySQL conectado a la red personalizada:

```bash
docker run -d \
  --name mysql-container \
  --network mysql-phpmyadmin-net \
  -e MYSQL_ROOT_PASSWORD=rootpass \
  -e MYSQL_DATABASE=prueba_db \
  -e MYSQL_USER=usuario \
  -e MYSQL_PASSWORD=usuariopass \
  mysql:8.0
```
![Contenedor Docker con PostgreSQL](docker/1.png)  

### Paso 3: Crear el contenedor de phpMyAdmin

Ejecuta el siguiente comando para crear el contenedor de phpMyAdmin y conectarlo a la misma red que el contenedor de MySQL:

```bash
docker run -d \
  --name phpmyadmin-container \
  --network mysql-phpmyadmin-net \
  -e PMA_HOST=mysql-container \
  -e PMA_PORT=3306 \
  -e MYSQL_ROOT_PASSWORD=rootpass \
  -p 8080:80 \
  phpmyadmin/phpmyadmin
```
![Contenedor Docker con PostgreSQL](docker/1.png)  

### Parte 2: Acceso a la interfaz de phpMyAdmin

#### Paso 4: Acceder desde el navegador

- **En entorno local**: Accede a la siguiente URL en tu navegador:

```bash

http://localhost:8080

```
![Contenedor Docker con PostgreSQL](docker/1.png)  

# Paso 5: Crear una base de datos y tabla desde phpMyAdmin

```sql
CREATE DATABASE mi_prueba_db;

USE mi_prueba_db;

CREATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100),
    correo VARCHAR(100)
);

INSERT INTO clientes (nombre, correo) VALUES ('Ana López', 'ana@example.com');

```
![Contenedor Docker con PostgreSQL](docker/1.png) 

# 9. Resultados esperados

- phpMyAdmin debe conectarse a la base de datos MySQL usando `mysql-container` como host.
- Se podrá crear y visualizar bases de datos y tablas desde la interfaz web.
- Los datos se insertarán correctamente y estarán disponibles tras la inserción.

# 10. Bibliografía

- Docker Documentation. (2024). [https://docs.docker.com/](https://docs.docker.com/)
- MySQL Documentation. (2024). [https://dev.mysql.com/doc/](https://dev.mysql.com/doc/)
- phpMyAdmin Documentation. (2024). [https://www.phpmyadmin.net/docs/](https://www.phpmyadmin.net/docs/)

# 11. Enlace del audio 





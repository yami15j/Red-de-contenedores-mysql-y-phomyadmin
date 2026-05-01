# Práctica: Servidor de Base de Datos con Docker

## 1. Título
Despliegue de contenedores MySQL y phpMyAdmin con red personalizada en Docker usando WSL

## 2. Tiempo de duración
Aproximadamente 45 minutos

## 3. Objetivo
Implementar un entorno con Docker que permita la comunicación entre contenedores MySQL y phpMyAdmin mediante una red personalizada, y gestionar una base de datos desde una interfaz web.

## 4. Herramientas utilizadas
- Docker Desktop
- WSL (Ubuntu)
- Navegador web (Chrome o Edge)
- Terminal Linux

## 5. Fundamentos
Docker es una plataforma que permite crear, ejecutar y gestionar contenedores. Los contenedores son entornos ligeros que incluyen todo lo necesario para ejecutar una aplicación.

WSL (Windows Subsystem for Linux) permite ejecutar Linux dentro de Windows, facilitando el uso de herramientas como Docker.

Una red personalizada en Docker permite que los contenedores se comuniquen entre sí usando nombres en lugar de direcciones IP.

MySQL es un sistema gestor de bases de datos relacional que permite almacenar información estructurada.

phpMyAdmin es una herramienta web que permite administrar bases de datos MySQL mediante una interfaz gráfica.

## 6. Procedimiento

### Paso 1: Verificar Docker
Se verificó que Docker estuviera correctamente instalado ejecutando:

docker --version
docker ps

### Paso 2: Crear red personalizada

docker network create mi-red
docker network ls

### Paso 3: Crear contenedor MySQL

docker run -d \
  --name mysql-container \
  --network mi-red \
  -e MYSQL_ROOT_PASSWORD=root1234 \
  -e MYSQL_DATABASE=db_prueba \
  -p 3306:3306 \
  mysql:8.0

### Paso 4: Crear contenedor phpMyAdmin

docker run -d \
  --name phpmyadmin-container \
  --network mi-red \
  -e PMA_HOST=mysql-container \
  -e PMA_PORT=3306 \
  -p 8080:80 \
  phpmyadmin:latest

### Paso 5: Verificar contenedores

docker ps
:Se verificó que ambos contenedores estuvieran en estado activo.

### Paso 6: Verificar red

docker network inspect mi-red
:Se comprobó que ambos contenedores estaban conectados a la red.

### Paso 7: Acceder a phpMyAdmin

Se accedió desde el navegador a la siguiente dirección:

http://localhost:8080

Credenciales utilizadas:

Usuario: root
Contraseña: root1234

### Paso 8: Crear tabla y registros

Se ejecutó el siguiente código SQL:

CREATE TABLE usuarios (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  email VARCHAR(100) NOT NULL,
  edad INT
);

INSERT INTO usuarios (nombre, email, edad) VALUES
('Ana García', 'ana@gmail.com', 25),
('Carlos López', 'carlos@gmail.com', 30),
('María Pérez', 'maria@gmail.com', 22);

## 7. Resultados
Se crearon correctamente los contenedores MySQL y phpMyAdmin.
Se estableció comunicación entre ambos mediante una red personalizada.
Se accedió a phpMyAdmin desde el navegador.
Se creó una base de datos y una tabla con registros de prueba.
## 8. Conclusiones

En esta práctica se aprendió a utilizar Docker para desplegar servicios en contenedores. Se comprendió la importancia de las redes personalizadas para la comunicación entre servicios. Además, se logró implementar un entorno funcional con MySQL y phpMyAdmin, permitiendo la administración de bases de datos de manera sencilla mediante una interfaz web.

##	 9. Bibliografía
https://docs.docker.com
https://hub.docker.com/_/mysql
https://hub.docker.com/_/phpmyadmin
https://learn.microsoft.com/es-es/windows/wsl/

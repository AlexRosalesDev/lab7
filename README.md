
### Backend con Bases de Datos
Universidad
Universidad Adventista de Bolivia
Materia
Taller de Programación
Nombre del estudiante
Alex Rosales Quispe
Nombre del proyecto
### Backend con Bases de Datos
Descripción
Este proyecto implementa un sistema de operaciones CRUD (Create, Read, Update, Delete) utilizando el framework Laravel, con persistencia de datos en una base de datos MySQL gestionada mediante un contenedor Docker. Se utiliza Eloquent como ORM para interactuar con la base de datos de manera segura y eficiente, y se aplican migraciones para gestionar la estructura de la base de datos. El proyecto gestiona dos entidades principales: Gatos y Perros, con una API RESTful que permite realizar operaciones CRUD sobre ambas.
### Objetivos

Entender el flujo de un CRUD en Laravel (Create, Read, Update, Delete).
Implementar persistencia usando MySQL dentro de un contenedor Docker.
Aprender a usar Eloquent para interactuar con la base de datos de manera elegante y segura.

### Pasos para levantar el proyecto
Prerrequisitos

Tener una cuenta de GitHub
Tener PHP 8 o superior instalado
Tener Composer instalado
Tener Git instalado
Tener Docker instalado
Tener Visual Studio Code instalado

### Crear y clonar el repositorio
Crear el proyecto Laravel:
composer create-project laravel/laravel lab6
cd lab6

### Inicializar el repositorio Git y subirlo a GitHub:
echo "# lab6" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/AlexRosalesDev/lab6.git
git push -u origin main

Clonar el repositorio (si ya está creado):
git clone https://github.com/AlexRosalesDev/lab6.git
cd lab6

### Instalar dependencias
composer install

### Configurar el contenedor de MySQL con Docker
Crear un archivo docker-compose.yml en el directorio raíz del proyecto con el siguiente contenido:
version: "3.9"

services:
  mysql:
    image: mysql:8.0
    container_name: mysql_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: mydb
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:

Levantar el contenedor:
docker-compose up -d

Verificar la versión de MySQL:
docker exec -it mysql_db mysql -u root -p

Ingresar la contraseña rootpass y ejecutar:
SELECT VERSION();

### Configurar la base de datos
En el archivo .env, configurar la conexión a la base de datos:
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=mydb
DB_USERNAME=root
DB_PASSWORD=rootpass

### Crear la tabla gatos manualmente
Conectar a la base de datos MySQL:
docker exec -it mysql_db mysql -u root -p

Ejecutar:
USE mydb;
CREATE TABLE gatos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL
);
INSERT INTO gatos (nombre) VALUES ('Michi');

Verificar la tabla y los datos:
SHOW TABLES;
DESCRIBE gatos;
SELECT * FROM gatos;

### Crear la tabla dogs usando migraciones
Crear la migración para la tabla dogs:
php artisan make:migration create_dogs_table --create=dogs

Editar el archivo de migración generado en database/migrations para definir la estructura de la tabla dogs. Luego, ejecutar:
php artisan migrate

### Configurar la API
Instalar el soporte para API en Laravel:
php artisan install:api

Seleccionar no si se solicita ejecutar migraciones.
### Crear controladores y modelos
Crear el controlador para gatos:
php artisan make:controller CatController

Crear el modelo y controlador para perros:
php artisan make:model Dog -c

### Ejecutar el servidor de desarrollo
php artisan serve

### Probar la API
El servidor estará disponible en http://127.0.0.1:8000.
Ejemplo de endpoints para gatos:

Crear un gato:

curl -X POST http://127.0.0.1:8000/api/cats -d "nombre=Naranjo"


Listar todos los gatos:

curl http://127.0.0.1:8000/api/cats


Listar hasta 5 gatos:

curl http://127.0.0.1:8000/api/cats?limit=5


Obtener un gato por ID:

curl http://127.0.0.1:8000/api/cats/1


Actualizar un gato:

curl -X PUT http://127.0.0.1:8000/api/cats/1 -d "nombre=NuevoNombre"


Eliminar un gato:

curl -X DELETE http://127.0.0.1:8000/api/cats/1

Ejemplo de endpoints para perros:

Crear un perro:

curl -X POST http://127.0.0.1:8000/api/dogs \
  -H "Content-Type: application/json" \
  -d '{"nombre":"Rex"}'


Listar todos los perros:

curl http://127.0.0.1:8000/api/dogs


Listar hasta 5 perros:

curl http://127.0.0.1:8000/api/dogs?limit=5


Obtener un perro por ID:

curl http://127.0.0.1:8000/api/dogs/1


Actualizar un perro:

curl -X PUT http://127.0.0.1:8000/api/dogs/1 \
  -H "Content-Type: application/json" \
  -d '{"nombre":"NuevoNombre"}'


Eliminar un perro:

curl -X DELETE http://127.0.0.1:8000/api/dogs/1

### Acceder a los endpoints CRUD
Para gatos:

GET /api/cats - Listar todos los gatos (con parámetro opcional ?limit=N para limitar resultados)
GET /api/cats/{id} - Obtener un gato por ID (retorna 404 si no existe)
POST /api/cats - Crear un nuevo gato
PUT /api/cats/{id} - Actualizar un gato existente (retorna 404 si no existe)
DELETE /api/cats/{id} - Eliminar un gato (retorna 404 si no existe)

Para perros:

GET /api/dogs - Listar todos los perros (con parámetro opcional ?limit=N para limitar resultados)
GET /api/dogs/{id} - Obtener un perro por ID (retorna 404 si no existe)
POST /api/dogs - Crear un nuevo perro
PUT /api/dogs/{id} - Actualizar un perro existente (retorna 404 si no existe)
DELETE /api/dogs/{id} - Eliminar un perro (retorna 404 si no existe)

## Laboratorio 6 - Completado el mié 24 sep 2025 08:02:48 CST

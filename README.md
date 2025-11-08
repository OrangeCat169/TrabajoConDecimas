# TrabajoConDecimas
proyecto: crea un entorno de desarrollo local para **WordPress** y **MySQL** usando **Docker**, con persistencia de datos, red interna y configuración automática de base de datos y credenciales.

---

## ⚙️ Arquitectura general de mi conexion

- **WordPress** → contenedor principal del CMS (imagen oficial `wordpress:latest`)  
- **MySQL** → contenedor de base de datos (imagen oficial `mysql:latest`)  
- **Red interna** → `wp_network` para comunicación entre ambos servicios  
- **Volúmenes persistentes** → `db_data` y `wordpress_data` para evitar pérdida de información

## Servicios mysql
db:
    image: mysql:latest
    container_name: mysql_wp
    restart: always
- **image: mysql:latest: utiliza la versión más reciente de la imagen oficial de MySQL.**
- **container_name: asigna un nombre fijo (mysql_wp) al contenedor, útil para depuración.**
- **restart: always: asegura que el servicio se reinicie automáticamente ante un fallo o reinicio de Docker.**

## variables
    environment:
      MYSQL_DATABASE: TrabajoDecimas
      MYSQL_USER: Fernando
      MYSQL_PASSWORD: 1234
      MYSQL_ROOT_PASSWORD: root
- **Estas variables configuran la base de datos automáticamente**
- **Crea la base de datos TrabajoDecimas.**
- **Crea el usuario Fernando con contraseña 1234.**
- **Establece la contraseña del superusuario root como root.**

## Servicio WordPress
  wordpress:
    image: wordpress:latest
    container_name: wordpress_app
    depends_on:
      - db
- **depends_on: asegura que MySQL se inicie antes de WordPress.**
- **container_name: facilita el acceso y gestión manual del contenedor desde Docker CLI.**
## Puertos y reinicio
    ports:
      - "8082:80"
    restart: always
- **Expone el puerto 8082 del host al 80 del contenedor.
→ Acceso desde el navegador: http://localhost:8082**

## Variables de entorno
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: Fernando
      WORDPRESS_DB_PASSWORD: 1234
      WORDPRESS_DB_NAME: TrabajoDecimas
- **Indica a WordPress cómo conectarse a MySQL**

- **WORDPRESS_DB_HOST usa el nombre del servicio db (resuelto por Docker DNS) y el puerto 3306.**

- **Los demás valores coinciden con los del contenedor MySQL.**

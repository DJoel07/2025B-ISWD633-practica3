## Esquema para el ejercicio
![Imagen](esquema-ejercicio3.PNG)

### Crear red net-wp
```bash
docker network create net-wp
```

### Para que persista la información es necesario conocer en dónde mysql almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio carpeta del contenedor (a) es /var/lib/mysql

Ruta carpeta host: .../ejercicio3/db

### ¿Qué contiene la carpeta db del host?
La carpeta db está inicialmente vacía, no contiene ningún archivo.

### Crear un contenedor con la imagen mysql:8  en la red net-wp, configurar las variables de entorno: MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER y MYSQL_PASSWORD
```
docker run -d --name server-mysql --network net-wp -e MYSQL_ROOT_PASSWORD=rootpass123 -e MYSQL_DATABASE=wordpress_db -e MYSQL_USER=wp_user -e MYSQL_PASSWORD=wp_pass123 -v "C:\Users\doria\Documentos\Semestre 2025B\Construccion de Sw\Practica 3\ejercicio3\db":/var/lib/mysql mysql:8
```

### ¿Qué observa en la carpeta db que se encontraba inicialmente vacía?
La carpeta db ahora contiene múltiples archivos y subdirectorios creados por MySQL, incluyendo archivos como: auto.cnf, binlog, ca-key.pem, ca.pem, client-cert.pem, client-key.pem, ib_buffer_pool, ibdata1, ibtmp1, mysql, performance_schema, private_key.pem, public_key.pem, server-cert.pem, server-key.pem, sys, undo_001, undo_002, y la base de datos wordpress_db. Estos archivos representan el sistema de almacenamiento completo de MySQL.

### Para que persista la información es necesario conocer en dónde wordpress almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio la carpeta del contenedor (b) es /var/www/html

Ruta carpeta host: .../ejercicio3/www

### Crear un contenedor con la imagen wordpress en la red net-wp, configurar las variables de entorno WORDPRESS_DB_HOST, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD y WORDPRESS_DB_NAME (los valores de estas variables corresponden a los del contenedor creado previamente)
```bash
docker run -d --name server-wordpress --network net-wp -p 80:80 -e WORDPRESS_DB_HOST=server-mysql -e WORDPRESS_DB_USER=wp_user -e WORDPRESS_DB_PASSWORD=wp_pass123 -e WORDPRESS_DB_NAME=wordpress_db -v "C:\Users\doria\Documentos\Semestre 2025B\Construccion de Sw\Practica 3\ejercicio3\www":/var/www/html wordpress
```

### Personalizar la apariencia de wordpress y agregar una entrada

### Eliminar el contenedor y crearlo nuevamente, ¿qué ha sucedido?
Al eliminar y recrear el contenedor de WordPress con el mismo comando, todos los datos persisten: el tema personalizado, las entradas del blog, las páginas, los plugins instalados y todas las configuraciones. Esto se debe a que toda la información está almacenada en dos lugares:
1. La carpeta www del host contiene todos los archivos de WordPress (temas, plugins, uploads)
2. La carpeta db del host contiene toda la base de datos MySQL con el contenido y configuración

Esto demuestra que con bind mounts, los datos persisten completamente independientemente del ciclo de vida de los contenedores. 

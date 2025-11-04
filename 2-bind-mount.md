# BIND MOUNT
En un bind mount mapeamos (montar) un directorio o archivo específico del sistema de archivos del host con una parte del sistema de ficheros del contenedor.

```
docker run -d --name <nombre contenedor> -v <ruta carpeta host>:<ruta carpeta contenedor> <imagen> 
```
ó
```
docker run -d --name <nombre contenedor> --mount type=bind,source=<ruta carpeta host>,target=<ruta carpeta contenedor> <imagen>
```
- destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.
- source, src: El origen del montaje.
  
### En tu computador crear una carpeta llamada nginx y dentro de esta carpeta crea otra llamada html. Como se aprecia en la figura.
![Volúmenes](directorio.PNG)

### Crear un contenedor con la imagen nginx:alpine, mapear todos por puertos, para la ruta carpeta host colocar el directorio en donde se encuentra la carpeta html en tu computador y para la ruta carpeta contenedor: /usr/share/nginx/html (esta ruta se obtiene al revisar la documentación de la imagen)
![Volúmenes](volumen-host.PNG)
```
docker run -d --name server-nginx -p 80:80 --mount type=bind,source="C:\Users\doria\Documentos\Semestre 2025B\Construccion de Sw\Practica 3\nginx\html",target=/usr/share/nginx/html nginx:alpine
```

### ¿Qué sucede al ingresar al servidor de nginx?
Al ingresar a http://localhost, nginx muestra un error 403 Forbidden porque la carpeta html está vacía y no hay un archivo index.html para mostrar.

### ¿Qué pasa con el archivo index.html del contenedor?
El archivo index.html original del contenedor es reemplazado por el contenido de la carpeta host (que está vacía). El bind mount "sobrescribe" el contenido del contenedor con el contenido del host.

### Ir a https://html5up.net/ y descargar un template gratuito, descomprirlo dentro de tu computador en la carpeta html
### ¿Qué sucede al ingresar al servidor de nginx?
Al ingresar a http://localhost ahora se muestra el template descargado. Los cambios en la carpeta del host se reflejan inmediatamente en el contenedor sin necesidad de reiniciarlo, ya que están vinculados mediante bind mount.

### Eliminar el contenedor
```
docker rm -f server-nginx
```

### ¿Qué sucede al crear nuevamente un contenedor montado al directorio definidos anteriormente?
# COMPLETAR CON LA RESPUESTA A LA PREGUNTA
Al recrear el contenedor con el mismo bind mount, el template HTML sigue estando disponible porque los archivos se mantienen en la carpeta del host. Esto demuestra que los datos persisten independientemente del ciclo de vida del contenedor cuando se usa bind mount.


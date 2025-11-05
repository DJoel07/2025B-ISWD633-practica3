# MI APRENDIZAJE
Dorian Joel Alban Lucas

## Conocimientos Previos
Antes de realizar esta práctica, tenía conocimientos básicos sobre Docker y contenedores, pero no comprendía completamente cómo funcionaba la persistencia de datos. Sabía que los contenedores eran efímeros, pero no entendía las diferencias entre los distintos métodos de almacenamiento ni cómo aplicarlos en casos reales.

## Principales Aprendizajes

### 1. Tipos de Montaje en Docker
Aprendí que existen tres tipos principales de montaje:
- **Bind Mount**: Permite vincular directorios específicos del host con el contenedor, útil para desarrollo cuando necesito ver cambios en tiempo real.
- **Volúmenes Nombrados**: Administrados completamente por Docker, ideales para datos de producción que necesitan persistir.
- **Volúmenes Anónimos**: Útiles para datos temporales que persisten mientras existe el contenedor.

### 2. Persistencia de Datos
El concepto más importante que aprendí es que los datos dentro de un contenedor se pierden al eliminarlo, a menos que se configure correctamente un volumen o bind mount. Esto es crucial para aplicaciones reales como bases de datos o sitios web que necesitan mantener información.

### 3. Importancia de la Documentación
Descubrí que es fundamental revisar la documentación oficial de cada imagen en Docker Hub para conocer:
- Qué directorios se deben montar para persistir datos
- Qué variables de entorno se requieren
- Las rutas específicas donde cada servicio almacena su información (ej: `/var/lib/mysql` para MySQL, `/var/www/html` para WordPress)

### 4. Redes en Docker
Aprendí a crear y utilizar redes personalizadas para que los contenedores puedan comunicarse entre sí usando nombres en lugar de IPs, lo que facilita la configuración de aplicaciones multi-contenedor.

## Problemas Resueltos

### Problema 1: Rutas con Espacios en Windows
**Situación**: Al intentar crear un bind mount con una ruta que contenía espacios, Docker mostraba el error:
```
docker: invalid reference format: repository name must be lowercase
```

**Solución**: Encerrar la ruta completa entre comillas dobles:
```bash
docker run -d --name server-nginx -p 80:80 -v "C:\Users\doria\Documentos\Semestre 2025B\Construccion de Sw\Practica 3\nginx\html":/usr/share/nginx/html nginx:alpine
```

También aprendí una alternativa más robusta usando `--mount`:
```bash
docker run -d --name server-nginx -p 80:80 --mount type=bind,source="C:\Users\doria\Documentos\Semestre 2025B\Construccion de Sw\Practica 3\nginx\html",target=/usr/share/nginx/html nginx:alpine
```

### Problema 2: Verificación de Volúmenes
Utilicé comandos adicionales para verificar y administrar volúmenes:
```bash
# Listar todos los volúmenes
docker volume ls

# Inspeccionar un volumen específico
docker volume inspect vol-postgres

# Eliminar volúmenes no utilizados
docker volume prune
```

## Aplicación Práctica

### Caso WordPress + MySQL
La práctica del ejercicio 3 me permitió comprender cómo configurar una aplicación real con múltiples contenedores:
1. Crear una red personalizada para comunicación entre contenedores
2. Configurar bind mounts para persistir tanto la base de datos como los archivos de WordPress
3. Usar variables de entorno para conectar ambos servicios
4. Verificar que los datos persisten incluso después de eliminar y recrear los contenedores

Este conocimiento es directamente aplicable a proyectos profesionales donde necesito desplegar aplicaciones con bases de datos.

## Comandos Útiles Adicionales

Durante la práctica, encontré útiles estos comandos:
```bash
# Ver logs de un contenedor
docker logs server-mysql

# Acceder al shell de un contenedor
docker exec -it server-mysql bash

# Ver procesos en ejecución dentro de un contenedor
docker top server-mysql

# Copiar archivos entre host y contenedor
docker cp archivo.txt server-nginx:/usr/share/nginx/html/
```

## Conclusión
Esta práctica transformó mi comprensión de Docker de un nivel básico a uno más profesional. Ahora entiendo que la persistencia de datos es fundamental en cualquier aplicación real, y sé cómo implementarla correctamente según las necesidades del proyecto. Los conocimientos sobre bind mounts son especialmente valiosos para desarrollo, mientras que los volúmenes nombrados son esenciales para ambientes de producción.

El aprendizaje más valioso es que Docker no solo sirve para ejecutar contenedores, sino que proporciona un ecosistema completo para gestionar aplicaciones complejas con múltiples servicios interconectados y datos persistentes.

# Docker-compose
Introducción

En este trabajo he realizado el despliegue de una aplicación web utilizando Docker y Docker Compose.
El objetivo ha sido levantar un entorno completo con PHP, MySQL, phpMyAdmin y Nginx, y comprobar que tanto la base de datos como la página web funcionan correctamente dentro de contenedores.

Para ello, he seguido la guía proporcionada en el siguiente enlace:
https://punkymo.gitbook.io/miwiki/virtualizacion/contenedores/docker/docker-compose/importar-sitio-web-en-docker

Introducción

En este trabajo he realizado el despliegue de una aplicación web utilizando Docker y Docker Compose.
El objetivo ha sido levantar un entorno completo con PHP, MySQL, phpMyAdmin y Nginx, y comprobar que tanto la base de datos como la página web funcionan correctamente dentro de contenedores.

Para ello, he seguido la guía proporcionada en el siguiente enlace:
https://punkymo.gitbook.io/miwiki/virtualizacion/contenedores/docker/docker-compose/importar-sitio-web-en-docker

Configuración del entorno
<img width="1277" height="855" alt="1" src="https://github.com/user-attachments/assets/c6b68b1f-922c-4dbb-90c6-0302b1b9b20e" />

Aquí enseño la modificación del archivo docker-compose.yml.
En este archivo se definen todos los servicios que se iba a usar:
Un contenedor para PHP
Un contenedor para MySQL
Un contenedor para phpMyAdmin
Un contenedor para Nginx
También se configure:
Los puertos, para poder acceder desde el navegador.
Los volúmenes, para que los datos no se pierdan.
La red, para que los contenedores se comuniquen entre sí.
Gracias a este archivo, todos los servicios se levantan juntos con un solo comando.


Modificación del archivo conexion.php
<img width="646" height="487" alt="2" src="https://github.com/user-attachments/assets/3211aa62-15f9-474e-bc12-3b52af642ca0" />

Aquí muestro el archivo conexion.php, que es el encargado de realizar la conexión entre la página web y la base de datos.
El cambio más importante que realice es sustituir localhost por db en el nombre del servidor. Esto se debe a que, cuando se trabaja con Docker, cada servicio se ejecuta dentro de su propio contenedor. En este caso, el servidor web y la base de datos no están en el mismo contenedor.
Cuando utiliza localhost, la aplicación intenta conectarse a una base de datos que debería estar dentro del mismo contenedor donde se ejecuta PHP. Como MySQL se encuentra en otro contenedor diferente, la conexión falla y la página no se carga correctamente, mostrando un error al acceder desde el navegador.
Por este motivo utilice db, que es el nombre del servicio de MySQL definido en el archivo docker-compose.yml. Docker permite que los contenedores se comuniquen entre sí usando estos nombres, por lo que al usar db la aplicación sí puede encontrar el servidor de base de datos.
Además, se añade el nombre de la base de datos s21sec, que es la base de datos creada dentro del contenedor de MySQL. De esta forma, la aplicación sabe exactamente a qué base de datos debe conectarse.
Gracias a estos cambios, la conexión con la base de datos funciona correctamente y la página web se carga sin errores en el navegador.

Creación del archivo Dockerfile
<img width="1276" height="854" alt="3" src="https://github.com/user-attachments/assets/584f0ac5-1d6c-40f9-85fa-2ad2d74bebeb" />

Aquí se muestra la creación del archivo Dockerfile, que utilizo para configurar el contenedor de PHP.
La línea FROM php:8.2-fpm indica que el contenedor va a utilizar PHP en la versión 8.2, que es la encargada de ejecutar el código de la página web.
La línea RUN docker-php-ext-install mysqli pdo pdo_mysql sirve para instalar las extensiones necesarias para que PHP pueda comunicarse con la base de datos MySQL.
Sin estas extensiones, la aplicación no podría conectarse a la base de datos y la página web no funcionaría correctamente.

Uso del comando docker compose up -d --build
<img width="602" height="27" alt="4" src="https://github.com/user-attachments/assets/0e414cf5-0cdc-436c-8abe-704f25db7064" />

Aquí ejecuto el comando docker compose up -d --build, que es el encargado de poner en marcha todo el proyecto.
Este comando construye las imágenes necesarias, crea los contenedores definidos en el archivo docker-compose.yml y los inicia automáticamente. Además, gracias a la opción -d, los contenedores se ejecutan en segundo plano, permitiendo seguir usando la terminal sin que se bloquee.
Este es el comando principal para arrancar la aplicación web y todos sus servicios al mismo tiempo.

Base de datos funcionando en Docker
<img width="1280" height="854" alt="5" src="https://github.com/user-attachments/assets/a3f51c69-9e2e-49bc-98b0-f4a06366e083" />

Aquí enseño que la base de datos ya está creada y funcionando correctamente dentro del contenedor de MySQL. A través de phpMyAdmin se comprueba que la base de datos s21sec está activa y que contiene todas sus tablas.
Esto confirma que el contenedor de MySQL se ha iniciado correctamente y que la conexión entre phpMyAdmin y la base de datos funciona sin errores.

Página web funcionando correctamente
<img width="1281" height="854" alt="6" src="https://github.com/user-attachments/assets/ae1a22ca-8470-47ea-b5a0-74ec19a1063b" />

En esta imagen se muestra que la página web del proyecto se abre correctamente desde el navegador. Esto indica que el servidor web y PHP están funcionando bien dentro de los contenedores.
Además, demuestra que la aplicación puede acceder a la base de datos y cargar la información sin problemas, por lo que todo el entorno Docker está correctamente configurado.

¿Qué son los contenedores de Docker?

Los contenedores de Docker son una forma de ejecutar aplicaciones de manera aislada, es decir, cada aplicación funciona como si estuviera sola, sin afectar a otras.
Dentro de un contenedor se incluye todo lo necesario para que la aplicación funcione correctamente: el programa, sus librerías y su configuración.
La principal idea de Docker es que una aplicación funcione igual en cualquier ordenador, independientemente del sistema operativo o de lo que tenga instalado el usuario. Gracias a esto, se evitan muchos errores.

Diferencias entre contenedores Docker y LXC
Docker y LXC son dos tecnologías parecidas, pero se usan con objetivos distintos.
LXC está más pensado para crear sistemas completos, parecidos a máquinas virtuales ligeras. En cambio, Docker está pensado para ejecutar aplicaciones concretas de forma sencilla.
Docker es más fácil de usar, tiene más herramientas y es más popular actualmente. Además, Docker se centra más en el despliegue rápido de aplicaciones, mientras que LXC se usa más para administrar sistemas.

Diferencia entre una imagen y un contenedor en Docker
Una imagen es como una plantilla o un molde. Contiene todo lo necesario para crear un contenedor, pero no se ejecuta por sí sola.
Un contenedor es la imagen en funcionamiento. Es decir, cuando se ejecuta una imagen, se crea un contenedor que ya está activo y funcionando.

¿Qué pasa con los datos cuando se elimina un contenedor?
Cuando un contenedor se elimina, todos los datos que hay dentro se pierden, ya que el contenedor es temporal.
Para evitar esto, Docker permite usar volúmenes, que son carpetas compartidas entre el contenedor y el sistema. De esta forma, aunque el contenedor se borre, los datos se mantienen.
Por eso, para bases de datos siempre es recomendable usar volúmenes.

Ventajas de utilizar Docker
Docker tiene muchas ventajas, entre las más importantes:
- Permite ejecutar aplicaciones de forma rápida.
- Ahorra tiempo en configuraciones.
- Usa pocos recursos del sistema.
- Facilita el trabajo en equipo.
- Evita errores de compatibilidad.
- Permite levantar varios servicios a la vez con Docker Compose.
  
¿Qué tipo de aplicaciones se pueden desplegar con Docker?
Con Docker se pueden desplegar muchos tipos de aplicaciones, como por ejemplo:
- Páginas web
- Servidores web
- Bases de datos
- APIs
- Aplicaciones backend
- Aplicaciones frontend
- Sistemas completos con varios servicios
Docker es muy flexible y se adapta a muchos proyectos diferentes.

Otros tipos de contenedores además de Docker
Además de Docker, existen otros contenedores, como:
- LXC: contenedores más parecidos a sistemas completos
- Podman: similar a Docker, pero sin demonio central.
- containerd: usado internamente por Docker.
- CRI-O: usado junto con Kubernetes.
  
Guía de usuario: despliegue de una aplicación web con contenedores
1. Preparar el proyecto web en una carpeta.
2. Crear el archivo docker-compose.yml con los servicios necesarios.
3. Crear el Dockerfile para configurar PHP.
4. Configurar la conexión a la base de datos usando el nombre del servicio.
5. Ejecutar el comando docker compose up -d --build.
6. Acceder a phpMyAdmin e importar la base de datos.
7. Comprobar que la página web funciona correctamente.

Conclusión
Docker es una herramienta muy útil para desplegar aplicaciones web de forma rápida y ordenada. Gracias a los contenedores, se pueden ejecutar varios servicios sin conflictos y con una configuración clara.
El uso de Docker Compose facilita mucho el trabajo, ya que permite levantar todo el entorno con un solo comando.







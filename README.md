![logo](https://global.download.synology.com/download/Package/img/Docker/1.11.2-0270/thumb_256.png?style=centerme)
![logo](https://manishnamdeo.files.wordpress.com/2014/04/tikiwiki.gif)

# Proyectos de contenedores Dockers TikiWiki.
Proyecto para desplegar los servicios de la plataforma Tikwiki con Docker.

# Estructura del repositorio.

| Nombre   | Descripción                                                     |
|:--------:|:----------------------------------------------------------------|
| /conf    | Ficheros de configuración para los servicios.                   |
| /db      | Directorio para almacenar las base de datos (/var/lib/mysql).   |
| /doc     | Documentación adicional sobre el proyecto.                      |
| /env     | Fichero con variables de entorno para los servicios.            |
| /hooks   | Carpeta donde almacenar los scripts git hooks.                  |
| /php     | Dockerfile de la imagen tikiwiki-webphp                         |
| /src     | Directorio con los fuentes de tikiwiki compilados con composer. |
| /test    | Directorio donde almacenar los test linter, unicode, etc..

# Despliegue de la plataforma con docker-compose.
-   Clonamos el repositorio.
```
git clone -b develop https://github.com/westerus/stack-tikiwiki.git
```
```
cd stack-tikiwiki/
```
-   Inicializamos submodulo "/src" y clonamos el fuente de la tikiwiki.
```
git submodule update --init --recursive
```
-   Modificamos los permisos del directorio de publicación.
```
docker run --rm -ti -v $PWD/src:/var/www/html westerus/tikiwiki-webphp:dev sh -c 'echo f| sh setup.sh'
```
-   Iniciamos el stack.
```
docker-compose -f docker-compose.dev.yml up -d
```
-   Accedemos a la tikiwiki.
```
http://localhost
```

Para actualizar el fuente "src/" ejecutar:
```
git submodule update --remote
```
# Variables de entorno.
-   Fichero "env/db.env"

| Nombre                          | Descripción                              |
|:-------------------------------:|:-----------------------------------------|
| MYSQL_RANDOM_ROOT_PASSWORD=yes  | Genera un password aleatorio para root.  |
| MYSQL_DATABASE=tikidb           | Nombre de la base de datos.              |
| MYSQL_USER=tikiuser             | Usuario para la base de datos.           |
| MYSQL_PASSWORD=tikipass         | Password para el usuario de la db.       |

# Documentación:
[GitLab Community Edition documentation](https://gitlab.com/gitlab-org/gitlab-ce/blob/master/doc/README.md)

-   [CI/CD](https://gitlab.com/gitlab-org/gitlab-ce/blob/master/doc/ci/README.md)
-   [Learn how .gitlab-ci.yml works](https://gitlab.com/gitlab-org/gitlab-ce/blob/master/doc/ci/yaml/README.md)

-   [New CI build permissions model](https://git.virt.cga/help/user/project/new_ci_build_permissions_model.md)

-   [Triggering Builds through the API](https://git.virt.cga/help/ci/triggers/README)

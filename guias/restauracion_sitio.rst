Restauración del sitio
=========================

https://docs.moodle.org/all/es/Restauraci%C3%B3n_del_sitio

Si Usted ha seguido las instrucciones para Copia de seguridad del sitio y creó una copia de seguridad (resplado) de un sitio Moodle, Usted puede necesitar ahora conocer cómo restaurar el respaldo del sitio que creó.

Hay tres áreas que pueden restaurarse individualmente o juntas::

	Código de Moodle
	Archivos subidos o creados de Moodle
	Base de datos de Moodle - MySQL, PostgreSQL u otra

La localización y los nombres de estas áreas pueden encontrarse en el archivo de configuración config.php.


Restauración por línea de comando (Linux)
+++++++++++++++++++++++++++++++++++++++++++++

Aquí está un conjunto de pasos básicos para hacer el proceso de restauración:

1. Renombre el directorio original de Moodle a algo diferente (pero que Usted siga conservando) y copie el directorio respaldado de Moodle o un nuevo directorio de Moodle descargado en su lugar.

2. Si Usted está corriendo MySQL, un respaldo de la BasedeDatos deberá de ser un archivo .sql, .gz o .tar.gz . Si fuera un .tar.gz o .gz Usted necesitará extraerlo hasta que sea un archivo sql::

	tar -xzvf moodlesqlfile.tar.gz

3. Si Usted está corriendo MySQL, importe el archivo SQL de regreso dentro de una nueva BasedeDatos recién creada en el servidor MySQL. Tenga cuidado aquí, porque algunos respaldos tratan de importar de regreso otra vez hacia la misma BasedeDatos funcionando a la que Moodle está conectada. Esto causa problemas en la BasedeDatos que dañan una instalación de Moodle. Lo mejor que Usted puede hacer es construir una nueva BasedeDatos, restaurar la BasedeDatos restaurada hacia ella, y cambiar el archivo config.php de Moodle para que se conecte hacia esta nueva BasedeDatos (de esta manera Usted todavía tendrá la basedeDatos original).

Una vez que Usted haya creado la nueva BasedeDatos (new_database):::

	mysql -p new_database < moodlesqlfile.sql

Para otras BasesdeDatos, siga las instrucciones para restaurar un respaldo.

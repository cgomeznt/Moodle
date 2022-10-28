
Como hacer un respaldo de Moodle
++++++++++++++++++++++++++++++++++

https://docs.moodle.org/all/es/Respaldo_del_sitio

Una copia de seguridad (respaldo) del sitio web (backup) permite al administrador del mismo guardar todo lo relacionado al sitio Moodle. Estas copias pueden ser restauradas para devolver el sitio al punto en que estaba cuando la copia fue hecha.

Realizar copias de seguridad de forma regular es altamente recomendable para reducir el montón de información perdida en caso de un problema en el sitio y para acelerar el proceso de recuperación general.

¿De que es necesario hacer una copia de seguridad?
+++++++++++++++++++++++++++++++++++++++++++++++++++++

Un sistema con Moodle se compone de tres partes:

Los datos almacenados en la base de datos (Por ejemplo, un base de datos MySQL)
Los archivos cargados (Por ejemplo, los archivos de sitio y curso subidos por Moodle en moodledata)
El Moodle code (Por ejemplo, todo en server/htdocs/moodle)
Se puede confirmar donde están localizadas todas estas cosas en una instalación de Moodle comprobando el archivo de configuración config.php::

	$CFG->dbname muestra el nombre de la base de datos
	$CFG->prefix muestra el nombre del prefijo de la tabla de la base de datos
	$CFG->dataroot 3. controla donde se almacenan los archivos cargados upload files; y
	$CFG->wwwroot 4. apunta a donde el código es almacenado.

**Consejo**
En términos generales, la base de datos ("dbname y prefijo")y los archivos cargados (dataroot) son los más importantes que copiar de forma regular. Estos contienen la información que cambiará más a menudo.

El código Moodle (wwwroot) es menos importante como copia de seguridad frecuente, ya que solo cambiará cuando el código real cambie con las actualizaciones, addins y ajustes de código. Se puede obtener siempre una copia del código estándar de Moodle desde http://download.moodle.org así que solo es necesario copiar las partes que hemos añadido o cambiado nosotros mismos.


Creando una copia de seguridad de su sitio Moodle
====================================================

Base de datos
++++++++++++++

La forma correcta de hacer una copia de seguridad de la base de datos depende del sistema de base de datos que se esté usando. Las instrucciones siguientes son una manera de hacer una copia de una base de datos MySQL. Otra opción sería utilizar una herramienta como phpMyAdmin para hacer una copia manualmente. La documentación para su base de datos tendrá más opciones.

Hay muchas formas de hacer estas copias de seguridad. He aquí un esbozo de un pequeño script que puede ejecutarse en Unix para hacer un backup de la base de datos (funciona bien tener un script así corriendo diariamente a través de una tarea de cron)::

	cd /my/backup/directory
	mv moodle-database.sql.gz moodle-database-old.sql.gz
	mysqldump -h example.com -u myusername --password=mypassword -C -Q -e --create-options mydatabasename > moodle-database.sql
	gzip moodle-database.sql

Este fue el comando utilizado en la Granja, se le agrego otro argumento que fue el -S::

	mysqldump -S /mariadb/data/MOODLEUCC/mysql_moodleucc.sock -u usr_ucc --password=ucc2021 -C -Q -e --create-options ucc > moodle-database.sql

Codificación de caracteres
++++++++++++++++++++++++

Asegúrese de que la copia de seguridad de la base de datos utiliza la codificación correcta. En la mayoría de bases de datos se usa UTF-8.

Cuando se vuelca toda la base de datos de Moodle, comprobar si hay posibles problemas de codificación de caracteres. En algunas instancias, copias de seguridad creadas con mysqldump o phpMyAdmin pueden no codificar de forma correcta todos los datos. Esto resultará en caracteres no legibles cuando se restaure la base de datos.

Consejo: Una solución es usar MySQL Administrator 1.1 o otro que fuerce un volcado de los datos en UTF-8.

Herramientas para las copias de seguridad de la base de datos
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

**phpMyAdmin**

phpMyAdmin es la herramienta de elección en la mayoría de los proveedores de alojamiento web.

**MySQLDumper**

: MySQLDumper es un script de copia de seguridad para bases de datos MySQL, escrito en PHP y Perl. MySQLDumper utiliza una técnica propietaria para evitar la interrupción de la ejecución al ejecutar scripts PHP (el tiempo máximo de ejecución suele estar puesto en 30 segundos). MySQLDumper además se ocupa de los problemas de codificación antes mencionados. También trabaja con archivos comprimidos y permite configurar trabajos regulares de actualización cron y actualizar un sitio remoto FTP.

**AutoMySQLBackup**

AutoMySQLBackup es un script para respaldo para bases de datos MySQL, escrito en PHP. AutoMySQLBackup creará respaldos Diarios, Semanales y Mensuales de una o más de sus bases de datos MySQL de uno o más de sus servidores MySQL. Las características incluyen notificación por email, compresión y encriptación, rotación configurable del respaldo, respaldos incrementales de base de datos y más.

Archivos subidos (moodledata)
++++++++++++++++++++++++++++

A través del interfaz de Moodle, usuarios pueden cargar o crear archivos y carpetas. Estos están localizados en un directorio, a menudo llamado “moodledata”. Desde que estos son solo archivos y carpetas, hay muchas maneras diferentes de hacer un backup o copiar moodledata.

* Por ejemplo, utilizando un programa de transferencia de archivos, copie todo el directorio moodledata en un sitio diferente, disco u ordenador. Ejemplos de programas de transferencia de archivos incluyen: FTP, WinSP, wget, rsync.

* Puede utilizar un programa de compresión para crear archivos compactos (tar, zip, 7z, XZ, BZIP2, GZIP, y WIM son algunos formatos) de todo el directorio. Esto puede hacerse antes o después de la transferencia de archivos.

**Consejos**

* Por lo general no todos los archivos en moodledata cambian entre copias de seguridad periódicas/regulares. Un nuevo Administrador puede querer mirar en copias de seguridad incrementales o otro tipo de copias eficientes .

Código de Moodle*
++++++++++++++++++++++

Hacer una copia de seguridad del código de Moodle será similar a copiar moodledata. Vea herramientas para hacer copias de seguridad de los archivos del servidor.

**Consejo**
Es siempre una buena idea tener varias copias de los archivos del código de Moodle. Mientras que siempre se puede descargar una nueva copia del código base de Moodle desde http://download.moodle.org, puede que haya personalizado ese código. Es una buena idea crear copias de seguridad separadas de su código de Moodle antes de personalizar el código. Esto incluye instalar Contribuciones al código, Temas y actualizaciones.

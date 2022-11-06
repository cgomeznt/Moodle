Permisos de los directorios en Moodle
=======================================  

Simple el propietario debe ser apache::

  owner: apache user (apache, httpd, www-data, lo_que_sea)
  group: apache group (apache, httpd, www-data, lo_que_sea)
  perms: 700 en directorios, 600 en archivos


Considere estos permisos como los más paranoides. Debería estar suficientemente seguro con permisos menos restrictivos, tanto en el directorio de moodle como en el de moodledata (junto con todos sus subdirectorios).

Recuperar clave Admin
=========================


Login to your moodle database and find the user table mdl_user (moodle ver. 3.3)::

	# mysql -uroot -p
	Enter password: 
	Welcome to the MariaDB monitor.  Commands end with ; or \g.
	Your MariaDB connection id is 571
	Server version: 10.6.10-MariaDB MariaDB Server

	Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

	Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

	MariaDB [(none)]> use ucc;
	Reading table information for completion of table and column names
	You can turn off this feature to get a quicker startup with -A

	Database changed
	MariaDB [ucc]> select username,password from mdl_user;
	+----------------------------------------------+--------------------------------------------------------------+
	| username                                     | password                                                     |
	+----------------------------------------------+--------------------------------------------------------------+
	| guest                                        | $2y$10$5IelkU4CkUh6H2fGjBiB3uNud3eREh8qU/n/6a5ARsqeCLsmTHdue |
	| admin                                        | $2y$10$226qaQx/Z1eKxUy/irHbwuly9Fztokj786jdhsjk098sdkk81237i |
	| carlos.gomez                                 | $2y$10$226qaQx/Z1eKxUy/irHbwuly9Fztfj8va9aDyOFfZVgtyGZeFguri |

	+----------------------------------------------+--------------------------------------------------------------+
	3 rows in set (0,001 sec)

	MariaDB [ucc]> 


2. which admin password is stored using php function 'password_hash' so run the following code with your desired password (I have used 'admin' as password)::


	<?php
		     $pass=password_hash("admin", PASSWORD_BCRYPT);
		      echo "password: " . $pass;
	?>

output in the Browser::

	password: $2y$10$XUDprGHPmj9.kERhQujMMu0xa/8.uR4118GXs9T3OIB8WYxqkNRXq


3. copy the password and update the table with copied password.::

	update mdl_user set password="$2y$10$XUDprGHPmj9.kERhQujMMu0xa/8.uR4118GXs9T3OIB8WYxqkNRXq" where username="admin"

4. Check again and view the new hash::

	MariaDB [ucc]> select username,password from mdl_user;
	+----------------------------------------------+--------------------------------------------------------------+
	| username                                     | password                                                     |
	+----------------------------------------------+--------------------------------------------------------------+
	| guest                                        | $2y$10$5IelkU4CkUh6H2fGjBiB3uNud3eREh8qU/n/6a5ARsqeCLsmTHdue |
	| admin                                        | $2y$10$XUDprGHPmj9.kERhQujMMu0xa/8.uR4118GXs9T3OIB8WYxqkNRXq |
	| carlos.gomez                                 | $2y$10$226qaQx/Z1eKxUy/irHbwuly9Fztfj8va9aDyOFfZVgtyGZeFguri |

	+----------------------------------------------+--------------------------------------------------------------+
	3 rows in set (0,001 sec)


that's all 

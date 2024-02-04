# p6-oc
Files for the 6th project for Openclassrooms

Those files are to secure a GLPI installation.

## local_define.php

The "local_define.php" file ensure a minimum of security. It defines new locations at a safe place and can't be access from the apache web server. It must be placed at the following location: `/etc/glpi/loca_define.php`

```php
<?php
define('GLPI_VAR_DIR', '/var/lib/glpi');
define('GLPI_LOG_DIR', '/var/log/glpi');
```

## downstream.php

Like the "local_define.php" file, the "downstream.php" file defines new locations for the config files. Doing like so adds another layer of security. It must be placed at the following location: `/var/www/html/glpi/inc/local_define.php`

```php
<?php
define('GLPI_CONFIG_DIR', '/etc/glpi/');
if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
require_once GLPI_CONFIG_DIR . '/local_define.php';
}
```

## glpi.conf

This is the configuration file to apache2. This file must be downloaded at the /etc/apache2/sites-available location and enabled with the command `a2ensite glpi.conf`. In order to work properly, you need to change the "\__IP__" variable with the IP of the host machine. 

```
<VirtualHost *:80>  
	# ServerName glpi
	ServerAlias __IP__
	DocumentRoot /var/www/html
	Alias "/glpi" "/var/www/html/glpi/public"  
	<Directory /var/www/html/glpi>
		Require all granted  
		RewriteEngine On  
		RewriteCond %{REQUEST_FILENAME} !-f  
		RewriteRule ^(.*)$ index.php [QSA,L]  
	</Directory>  
</VirtualHost>
```
If an error occure, try to enable the rewrite module: `a2enmod rewrite`

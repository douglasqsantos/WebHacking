# Web Hacking and Penetration Testing

## Damn Vulnerable Web Application (DVWA)
- http://www.dvwa.co.uk/
- https://github.com/ethicalhack3r/DVWA


## DVWA Installing on Ubuntu 18.04

Let's update the repositories
```bash
apt update
```

Now let's install the dependencies packages
```bash
apt-get -y install apache2 mariadb-server php php-mysqli php-gd libapache2-mod-php git
```

Let's check the Apache Server
```bash
ccurl -I http://localhost
HTTP/1.1 200 OK
Date: Sat, 01 Aug 2020 19:54:04 GMT
Server: Apache/2.4.29 (Ubuntu)
Last-Modified: Sat, 01 Aug 2020 19:49:07 GMT
ETag: "2aa6-5abd632d89581"
Accept-Ranges: bytes
Content-Length: 10918
Vary: Accept-Encoding
Content-Type: text/html
```

Now let's access the document root of the apache
```bash
cd /var/www/html/
```

Cloning the Repo
```bash
git clone https://github.com/ethicalhack3r/DVWA.git
```

Let's add the permission to the directory
```bash
chown -R www-data:www-data /var/www/html/DVWA
```

Now let's change the document root of the Apache 
```bash
vim /etc/apache2/sites-enabled/000-default.conf
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html/DVWA
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Now let's restart the apache server
```bash
systemctl restart apache2
```

Let's check the Apache Server
```bash
curl -I http://localhost
HTTP/1.1 200 OK
Date: Sat, 01 Aug 2020 19:59:40 GMT
Server: Apache/2.4.29 (Ubuntu)
Set-Cookie: PHPSESSID=13cs527ujojmu9u1sfqfe80g5f; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Type: text/html; charset=UTF-8
```

Now we need to configure the Database
```mysql
mysql -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 41
Server version: 10.1.44-MariaDB-0ubuntu0.18.04.1 Ubuntu 18.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database dvwa;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create user dvwa@localhost identified by 'p@ssw0rd';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all on dvwa.* to dvwa@localhost;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> flush privileges;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> quit
Bye
```

Now let's configure the connection between the application and the database

Let's copy the configuration file example
```bash
cp /var/www/html/DVWA/config/config.inc.php.dist /var/www/html/DVWA/config/config.inc.php
```

Now let's configure the database 
```php
vim /var/www/html/DVWA/config/config.inc.php
[...]
$_DVWA[ 'db_server' ]   = '127.0.0.1';
$_DVWA[ 'db_database' ] = 'dvwa';
$_DVWA[ 'db_user' ]     = 'dvwa';
$_DVWA[ 'db_password' ] = 'p@ssw0rd';
[...]
```


Now let's configure the php.ini
```ini
vim /etc/php/7.2/apache2/php.ini
[...]
allow_url_fopen = On ; Allows for Remote File Inclusions (RFI)
[...]
allow_url_include = On ; Allows for Remote File Inclusions (RFI)
[...]
display_errors = Off ; (Optional) Hides PHP warning messages to make it less verbose
```

**PHP Options:**
- https://secure.php.net/manual/en/filesystem.configuration.php#ini.allow-url-include
- https://secure.php.net/manual/en/filesystem.configuration.php#ini.allow-url-fopen
- https://secure.php.net/manual/en/features.safe-mode.php
- https://secure.php.net/manual/en/security.magicquotes.php
- https://secure.php.net/manual/en/errorfunc.configuration.php#ini.display-errors

Now let's restart the Apache Server
```bash
systemctl restart apache2
```

Now open the url http://server_ip

**Default Credentials:**
- Username: admin
- Password: password

On the botton click on Create/Reset Database

Now after that step be happy ;)

## DVWA Installing on Debian 10

Let's update the repositories
```bash
apt update
```

Now let's install the dependencies packages
```bash
apt-get -y install apache2 mariadb-server php php-mysqli php-gd libapache2-mod-php git
```

Let's check the Apache Server
```bash
curl -I http://localhost
HTTP/1.1 200 OK
Date: Sat, 01 Aug 2020 19:53:43 GMT
Server: Apache/2.4.38 (Debian)
Last-Modified: Sat, 01 Aug 2020 19:48:41 GMT
ETag: "29cd-5abd63143664c"
Accept-Ranges: bytes
Content-Length: 10701
Vary: Accept-Encoding
Content-Type: text/html
```

Now let's access the document root of the apache
```bash
cd /var/www/html/
```

Cloning the Repo
```bash
git clone https://github.com/ethicalhack3r/DVWA.git
```

Let's add the permission to the directory
```bash
chown -R www-data:www-data /var/www/html/DVWA
```

Now let's change the document root of the Apache 
```bash
vim /etc/apache2/sites-enabled/000-default.conf
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html/DVWA
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Now let's restart the apache server
```bash
systemctl restart apache2
```

Let's check the Apache Server
```bash
curl -I http://localhost
HTTP/1.1 200 OK
Date: Sat, 01 Aug 2020 20:00:01 GMT
Server: Apache/2.4.38 (Debian)
Set-Cookie: PHPSESSID=8t6tmvu5vnhhen5nlj4g9bf30g; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Type: text/html; charset=UTF-8
```

Now we need to configure the Database
```mysql
mysql -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 49
Server version: 10.3.23-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database dvwa;
Query OK, 1 row affected (0.000 sec)

MariaDB [(none)]> create user dvwa@localhost identified by 'p@ssw0rd';
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> grant all on dvwa.* to dvwa@localhost;
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> flush privileges;
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> quit
Bye
```

Now let's configure the connection between the application and the database

Let's copy the configuration file example
```bash
cp /var/www/html/DVWA/config/config.inc.php.dist /var/www/html/DVWA/config/config.inc.php
```

Now let's configure the database 
```php
vim /var/www/html/DVWA/config/config.inc.php
[...]
$_DVWA[ 'db_server' ]   = '127.0.0.1';
$_DVWA[ 'db_database' ] = 'dvwa';
$_DVWA[ 'db_user' ]     = 'dvwa';
$_DVWA[ 'db_password' ] = 'p@ssw0rd';
[...]
```

Now let's configure the php.ini
```ini
vim /etc/php/7.3/apache2/php.ini
[...]
allow_url_fopen = On ; Allows for Remote File Inclusions (RFI)
[...]
allow_url_include = On ; Allows for Remote File Inclusions (RFI)
[...]
display_errors = Off ; (Optional) Hides PHP warning messages to make it less verbose
```

**PHP Options:**
- https://secure.php.net/manual/en/filesystem.configuration.php#ini.allow-url-include
- https://secure.php.net/manual/en/filesystem.configuration.php#ini.allow-url-fopen
- https://secure.php.net/manual/en/features.safe-mode.php
- https://secure.php.net/manual/en/security.magicquotes.php
- https://secure.php.net/manual/en/errorfunc.configuration.php#ini.display-errors

Now let's restart the Apache Server
```bash
systemctl restart apache2
```

Now open the url http://server_ip

**Default Credentials:**
- Username: admin
- Password: password

On the botton click on Create/Reset Database

Now after that step be happy ;)

## DVWA with Docker

**Note:** Please ensure you are using aufs due to previous MySQL issues. Run docker info to check your storage driver. If it isn't aufs, please change it as such. There are guides for each operating system on how to do that, but they're quite different so we won't cover that here.



If you don not have Docker install you can use the following
```bash
curl -L https://raw.githubusercontent.com/douglasqsantos/DevOps/master/Docker/install-docker.sh | bash
```

We can run the docker image with 
```bash
docker run --rm -it -p 80:80 vulnerables/web-dvwa
```

Or if you are using this repo use the docker-compose.yml, change the web por if you want
```
docker-compose up -d
```

Now access the http://docker_ip/

**Default Credentials:**
- Username: admin
- Password: password
# phpmyadmin
installing phpmyadmin on kali linux

so guys if you have any issue to install phpMyAdmin on kali linux do the following things:

### Step 1: Install PHP
``sudo apt-get install -y php php-tcpdf php-cgi php-mysqli php-pear php-mbstring php-gettext libapache2-mod-php php-common php-   phpseclib php-mysql
``
### Step 2: Install MariaDB Database Server

The next step is to install MariaDB database server. We already have comprehensive guides on this subject.

https://computingforgeeks.com/how-to-install-mariadb-10-3-on-debian-9-debian-8/

https://computingforgeeks.com/install-mariadb-10-on-ubuntu-18-04-and-centos-7/

### Step 3: Install Apache Web Server

` sudo apt-get -y install apache2
`

### Step 4: Download and Install latest phpMyAdmin on Ubuntu 18.04 / Debian 9
` export VER="4.8.5"
sudo apt-get install -y wget
cd /tmp
wget https://files.phpmyadmin.net/phpMyAdmin/${VER}/phpMyAdmin-${VER}-all-languages.tar.gz `

` cd /tmp
wget https://files.phpmyadmin.net/phpMyAdmin/${VER}/phpMyAdmin-${VER}-english.tar.gz
`

` tar xvf phpMyAdmin-${VER}-english.tar.gz`

OR
`tar xvf phpMyAdmin-${VER}-all-languages.tar.gz `

` rm *.tar.gz
sudo mv phpMyAdmin-* /usr/share/phpmyadmin`

` sudo mkdir -p /var/lib/phpmyadmin/tmp
sudo chown -R www-data:www-data /var/lib/phpmyadmin`

` sudo mkdir /etc/phpmyadmin/
`

`sudo cp   /usr/share/phpmyadmin/config.sample.inc.php  /usr/share/phpmyadmin/config.inc.php
`
`$cfg['blowfish_secret'] = 'H2OxcGXxflSd8JwrwVlh6KW6s2rER63i'; 
`
`$cfg['TempDir'] = '/var/lib/phpmyadmin/tmp';
`

### Step 5: Configure Apache web Server
`sudo vim /etc/apache2/conf-enabled/phpmyadmin.conf
`
###### And add below data

<pre style="background-color:black; color: white;">
  # phpMyAdmin default Apache configuration

Alias /phpmyadmin /usr/share/phpmyadmin

<Directory /usr/share/phpmyadmin>
    Options SymLinksIfOwnerMatch
    DirectoryIndex index.php

    <IfModule mod_php5.c>
        <IfModule mod_mime.c>
            AddType application/x-httpd-php .php
        </IfModule>
        <FilesMatch ".+\.php$">
            SetHandler application/x-httpd-php
        </FilesMatch>

        php_value include_path .
        php_admin_value upload_tmp_dir /var/lib/phpmyadmin/tmp
        php_admin_value open_basedir /usr/share/phpmyadmin/:/etc/phpmyadmin/:/var/lib/phpmyadmin/:/usr/share/php/php-gettext/:/usr/share/php/php-php-gettext/:/usr/share/javascript/:/usr/share/php/tcpdf/:/usr/share/doc/phpmyadmin/:/usr/share/php/phpseclib/
        php_admin_value mbstring.func_overload 0
    </IfModule>
    <IfModule mod_php.c>
        <IfModule mod_mime.c>
            AddType application/x-httpd-php .php
        </IfModule>
        <FilesMatch ".+\.php$">
            SetHandler application/x-httpd-php
        </FilesMatch>

        php_value include_path .
        php_admin_value upload_tmp_dir /var/lib/phpmyadmin/tmp
        php_admin_value open_basedir /usr/share/phpmyadmin/:/etc/phpmyadmin/:/var/lib/phpmyadmin/:/usr/share/php/php-gettext/:/usr/share/php/php-php-gettext/:/usr/share/javascript/:/usr/share/php/tcpdf/:/usr/share/doc/phpmyadmin/:/usr/share/php/phpseclib/
        php_admin_value mbstring.func_overload 0
    </IfModule>

</Directory>

# Authorize for setup
<Directory /usr/share/phpmyadmin/setup>
    <IfModule mod_authz_core.c>
        <IfModule mod_authn_file.c>
            AuthType Basic
            AuthName "phpMyAdmin Setup"
            AuthUserFile /etc/phpmyadmin/htpasswd.setup
        </IfModule>
        Require valid-user
    </IfModule>
</Directory>

# Disallow web access to directories that don't need it
<Directory /usr/share/phpmyadmin/templates>
    Require all denied
</Directory>
<Directory /usr/share/phpmyadmin/libraries>
    Require all denied
</Directory>
<Directory /usr/share/phpmyadmin/setup/lib>
    Require all denied
</Directory>
</pre>

You can restrict access from specific IP by adding line like below
`Require ip 127.0.0.1 192.168.18.0/24
`


### Step 6: Visit phpMyAdmin Web interface
`localhost/phpmyadmin`

### if have trouble to login with you mysql user and password create another one and that user will grant all privileges:

`CREATE USER "mehdi"@"localhost" IDENTIDIED BY "Your password";`

`GRANT ALL PRIVILEGES ON *.* TO 'mehdi'@'localhost;`



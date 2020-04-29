# Ubuntu and Debian

> The following instructions are for installing a specific sprint of TAO on Ubuntu 18.04 and can be easily adjusted for Debian Buster/Sid.

The following instructions are for Ubuntu 18.04 and utilize /var/www/html/tao as the working directory for installationinstallation and /var/www/html as the DocumentRoot. Should you choose another Operating System version, another Debian based flavor, directory location, or DocumentRoot you will need to adjust these directions as appropriate. These instructions make the assumption that you have access to the command line, if needed, refer to your hosting company for how to proceed if you do not have SSH access to your environment.

## Server Preparation
Make sure the server is up to date:
```
sudo apt update
sudo apt dist-upgrade
```

Once the system is up to date, you will need to install the required packages to create your LAMP stack as well as the packages necessary to complete your TAO installation if not already present.

```
sudo apt install apache2 \
php \
php-cli \
php-common \
mysql-server \
php-xml \
php-zip \
php-curl \
php-mbstring \
libapache2-mod-php \
php-mysql \
curl \
wget \
zip \
tidy \
unzip \
composer \
nodejs \
nodejs-dev \
libssl1.0-dev \
npm
```

Install components needed to build and install _php-mcrypt_
```bash
sudo apt install php-dev libmcrypt-dev php-pear
sudo pecl channel-update pecl.php.net
sudo pecl install mcrypt-1.0.1
```

Add mcrypt to the extensions section of _php.ini_
```
sudo nano /etc/php/7.2/cli/php.ini
extension=mcrypt.so
```

Create a new database and user for TAO.

```
sudo su -
mysql
create database <database>;
create user '<user>'@'localhost';
set password for '<user>'@'localhost' = PASSWORD('<password>');
grant all privileges on <database>.* to '<user>'@'localhost' identified by '<password>';
flush privileges;
quit
exit
```

## Configure Apache
Using the editor of your choice, you will need to configure the ServerName, a Virtual Host, as well as the directory you are installing in. If you are using multiple virtual hosts, the Apache instructions can be found [here](https://httpd.apache.org/docs/2.4/vhosts/examples.html).

```bash
sudo nano /etc/apache2/apache2.conf
```

Configure _ServerName_
```bash
ServerName <hostname or IP>
```

Configure Virtual Host
```bash
<VirtualHost *:80>
        ServerName <hostname or IP>
        DocumentRoot /var/www/html/tao
</VirtualHost>
```

Configure Directory
```bash
<Directory /var/www/html/tao>
        Options FollowSymLinks MultiViews
        AllowOverride all
        Allow from all
</Directory>
```

You will also want to configure Apache to serve PHP pages first by configuring _dir.conf_ to have _index.php_ first.

```bash
sudo nano /etc/apache2/mods-enabled/dir.conf
```

Turn on the _mod-rewrite_ module
```
bash
sudo a2enmod rewrite
```

Verify your Apache configuration and then restart for your changes to take effect.

```bash
sudo apache2ctl configtest
sudo service apache2 restart
```

## Install TAO
Download the TAO Package from website or GitHub and prepare to install

```
wget https://releases.taotesting.com/TAO_3.3.0-RC2_build.zip    
unzip TAO_3.3.0-RC2_build.zip
sudo mv TAO_3.3.0-RC2_build /var/www/html/tao
```

Or use Git to download the latest code from GitHub directly:
```
sudo git clone https://github.com/oat-sa/package-tao.git /var/www/html/tao
cd /var/www/html/tao
sudo git checkout release-3.3-rc02
```

You will need to utilize the TAO Community repository to attain the composer files for newer code. 

```
cd /var/www/html
sudo git clone https://github.com/oat-sa/tao-community.git
```

To find the list of sprints(tag) run the following and then checkout the desired tag. Keep in mind not all tags are fully functioning.

```
cd tao-community
sudo git tag
sudo git checkout <tag>
```

You will then need to overwrite the composer files in your tao directory.

```
sudo cp composer.* ../tao
```

Change ownership to the Apache user

```
sudo chown -R www-data:www-data /var/www/html/tao
```

Install TAO components on to the server utilizing composer and then change ownership of the newly created tao directory to the Apache user

```
cd /var/www/html/tao
sudo composer install
sudo chown -R www-data ../tao
```

Install MathJax on the server if needed.

```
sudo wget https://hub.taotesting.com/resources/taohub-articles/articles/third-party/MathJax_Install_TAO_3x.sh
sudo chmod u+x MathJax_Install_TAO_3x.sh
sudo ./MathJax_Install_TAO_3x.sh
```

You can now complete your installation on the command line with the following command:

```
sudo -u www-data php tao/scripts/taoInstall.php \
--db_driver pdo_mysql \
--db_host localhost \
--db_name <db_name> \
--db_user <user> \
--db_pass <password>\
--module_namespace http://<hostname or IP>/first.rdf \
--module_url http://<hostname or IP> \
--user_login <user> \
--user_pass <password> \
-e taoCe
```

Your TAO instance will be available at:
```
http://<hostname or IP>/tao 
```

Alternatively you can install TAO in your browser by going to `http://<hostname or IP>/tao` if you have followed the above instructions. If you did not follow the above instructions for your Apache configuration you will need to adjust the URL as appropriate. 

# CentOS, RedHat and Fedora

> The following instructions are for installing TAO on CentOS 7 and can be easily adjusted for RHEL7 and Fedora 19.

The following instructions are for CentOS 7 and utilize _/var/www/html/tao_ as the working directory for installation and _/var/www/html_ as the DocumentRoot. Should you choose another Operating System version, another Red Hat based flavor, directory location, or DocumentRoot you will need to adjust these directions as appropriate. These instructions make the assumption that you have access to the command line, if needed, refer to your hosting company for how to proceed if you do not have SSH access to your environment.

## Server Preparation
Make sure the server is up to date:

```
sudo yum update
```

Add the _Epel_ repository and install yum utilities.

```
sudo yum -y install epel-release yum-utils wget nano
```

Prepare server to install PHP 7.2 and MySQL
In order to install PHP 7.2, we will need to install a third party repository and then configure it for the correct version.
```
sudo yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
sudo yum-config-manager --disable remi-php54
sudo yum-config-manager --enable remi-php72
```

Add the MySQL community repositories in order to install MySQL 5.7 and then configure it for the correct version.
```
sudo yum -y install http://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/mysql-community-release-el7-7.noarch.rpm
```

Once the system is up to date, you will need to install the required packages to create your LAMP stack as well as the packages necessary to complete your TAO installation if not already present.

```
sudo yum install httpd \
php \
php-cli \
php-common \
mysql-community-server \
mysql-community-common \
php-xml \
php-zip \
composer \
php-curl \
php-mbstring \
libhttp2-mod-php \
php-mcrypt \
php-mysql \
curl \
wget \
zip \
tidy \
unzip \
php-dom \
php-mcrypt \
php-opcache \
php-tidy \
policycoreutils-python \
npm 
```

## Configure MySQL

During installation, a temporary root password will be set for MySQL. In order to retrieve it you will need to first start MySQL:

```
sudo systemctl start mysqld
```

And then locate the temporary password from the *mysqld.log*

```
grep 'A temporary password' /var/log/mysqld.log |tail -1
```

To finish the database configuration you will use the *mysql_secure_installation* script and the temporary password. You will be prompted at this time to change your root password as well as other configuration options. 

*Note: Your passwords must have 12-characters including at least one uppercase letter, one lowercase letter, one number and one special character*

You will need to do the following to run the script:

```
/usr/bin/mysql_secure_installation
```

At this point you can login to the database and create a new user and/or database for installing TAO if you wish.

Create a new database and user for TAO.

```
sudo su -
mysql -p
create database <database>;
create user '<user>'@'localhost' identified by '<password>';
grant all privileges on <database>.* to '<user>'@'localhost' with grant option;
flush privileges;
quit
exit
```


## Configure Apache
Using the editor of your choice, you will need to configure the ServerName, a Virtual Host, as well as the directory you are installing in. If you are using multiple virtual hosts, you will need to follow the Apache instructions which can be found here(https://httpd.apache.org/docs/2.4/vhosts/examples.html)

```
sudo nano /etc/httpd/conf/httpd.conf
```

Configure ServerName

```
ServerName <hostname or IP>
Listen <hostname or IP>:80
```
Configure Virtual Host
```bash
<VirtualHost *:80>
        ServerName <hostname or IP>
        DocumentRoot /var/www/html/tao
</VirtualHost>
```

Configure Directory

```
<Directory /var/www/html/tao>
     Options FollowSymLinks MultiViews
     AllowOverride all
     Order allow,deny
     Allow from all
</Directory>
```

You will also want to configure Apache to server PHP pages first by editing the _dir_module_ section to have index.php first.
```
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>
```

Verify your Apache configuration and then restart for your changes to take effect.

```
sudo apachectl configtest
sudo systemctl restart httpd
```

If you are setting up TAO for internal use/testing only, you can turn off the firewall otherwise you will need to open your _firewalld_ for the port you plan on accessing TAO on. 

```
sudo systemctl disable firewalld
sudo systemctl stop firewalld
```

## Install TAO

Download TAO Package from website or GitHub and prepare to install.

```
wget https://releases.taotesting.com/TAO_3.3.0-RC2_build.zip
unzip TAO_3.3.0-RC2_build.zip
sudo mv TAO_3.3.0-RC2_build /var/www/html/tao
```

Or use Git to download the latest release from GitHub directly.
```
sudo yum install git
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

Change ownership to the Apache user.

```
sudo chown -R apache:apache /var/www/html/tao
```

Install TAO components on to the server utilizing composer and then change ownership of the newly created tao directory to the Apache user.

```
cd /var/www/html/tao
sudo composer install
sudo chown -R apache tao
sudo chown -R apache config
```

Configure selinux to allow access to the necessary directories.

```
sudo semanage fcontext -a -t httpd_sys_content_t /var/www/html/tao/
sudo restorecon -R /var/www/html/tao/
sudo setsebool -P httpd_unified 1
sudo chmod a+w tao/views/locales/
sudo chmod a+w config
```

Install MathJax on the server if needed.

```
sudo wget --no-check-certificate  https://hub.taotesting.com/resources/taohub-articles/articles/third-party/MathJax_Install_TAO_3x.sh
sudo chmod u+x MathJax_Install_TAO_3x.sh
sudo ./MathJax_Install_TAO_3x.sh
```

You can now complete your installation either on the command line with the following command:

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

And your TAO instance will be available at:

```
http://<hostname or IP>/
```

Or you can install TAO in your browser by going to `http://<hostname or IP>/` if you have followed the above instructions. If you did not follow the above instructions for your Apache configuration you will need to adjust the URL as appropriate. 

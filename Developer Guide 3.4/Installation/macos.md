# MacOS

> The following instructions are for installing TAO on MacOS Mojave.

The following instructions are for MacOS Mojave and utilizes /Users/<user>/tao as the working directory and the DocumentRoot. Should you choose another Operating System version, directory location, or DocumentRoot you will need to adjust these directions as appropriate. These instructions make the assumption that you have access to the command line.

## Server Preparation
You will need to install _brew_ on your system if you have not done so previously in order to install various software packages such as http, php, and mariadb:

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Verify your brew installation:

```
brew --version
brew doctor
```

Install wget, composer and git in order to install TAO:

```
brew install wget composer git
```

Install and start PHP 7.2

```
brew install php@7.2
php -v
brew services start php
```

## Configure Apache

As MAcOS Mojave comes with a default Apache2, we will need to stop and unload it to install our own:

```
sudo apachectl stop
sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist 2>/dev/null
```

Now we are ready to install and start httpd:

```
brew install httpd
brew services start httpd
```


Using the editor of your choice, you will need to configure Apache. If you are using virtual hosts, you will need to follow the Apache instructions which can be found here(https://httpd.apache.org/docs/2.4/vhosts/examples.html)

```
nano /usr/local/etc/httpd/httpd.conf
```

Configure the port to 80 vs 8080

```
#Listen 8080
Listen 80

```

Configure ServerName

```
ServerName <hostname or IP>
```

Set user and group

```
User <user>
Group <group>
```

Change DocumentRoot and configure directory

```
DocumentRoot /Users/<user>/tao
<Directory /Users/<user>/tao>
   Options Indexes FollowSymLinks MultiViews
   AllowOverride All
   Require all granted
</Directory>
```

You will need to configure Apache to use the mod_rewrite and php@7.2 modules:

```
LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so
LoadModule php7_module /usr/local/opt/php@7.2/lib/httpd/modules/libphp7.so
```

You will also want to configure Apache to serve PHP pages first by editing the dir_module section to have index.php first.

```
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>

<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>

```

## Configure MariaDB

For the database, we will be using MariaDB which we will install with brew:

```
brew install mariadb
brew services start mariadb
```

At this point you can login to the database as <user> and create a new user and/or database for installing TAO if you wish. You might also want to secure your <user> login at this time.

Create a new database and user for TAO.

```
mysql 
create database <database>;
create user '<user>'@'localhost' identified by '<password>';
grant all privileges on <database>.* to '<user>@'localhost' with grant option;
flush privileges;
quit
```

## Install TAO

Download the Tao Package from the website or GitHub and prepare to install

```
wget https://releases.taotesting.com/TAO_3.3.0-RC2_build.zip    
unzip TAO_3.3.0-RC2_build.zip
sudo mv TAO_3.3.0-RC2_build /Users/<user>/tao
```

Or use Git to download the latest release from GitHub directly

```
git clone https://github.com/oat-sa/package-tao.git /Users/<user>/tao
cd /Users/<user>/tao
git checkout release-3.3-rc02
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

Verify your Apache configuration and restart Apache:

```
apachectl configtest
brew services restart httpd
```

Install TAO components on to the server utilizing composer and then change ownership of the newly created tao directory to the Apache user.

```
cd /Users/<user>/tao
composer install
```

Create directory for your data.

```
mkdir -p /opt/data/tao
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
http://<hostname or IP>
```

Or you can install TAO in your browser by going to `http://<hostname or IP>` if you have followed the above instructions. If you did not follow the above instructions for your Apache configuration you will need to adjust the URL as appropriate. 
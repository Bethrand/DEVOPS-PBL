
# **Project 1: WEB STACK IMPLEMENTATION *LAMP STACK* IN AWS**

## STEP 1 — *INSTALLING APACHE AND UPDATING THE FIREWALL*

`sudo apt update`

`sudo apt install apache2`

`sudo systemctl status apache2`

![Apache Satus](./images/apachestatus.PNG)

` curl http://localhost:80`
`http://18.212.238.148:80`
`curl -s http://169.254.169.254/latest/meta-data/public-ipv4`

![Apache2 UbuntuPage](./Images/Apache2-UbuntuPage.PNG)


## STEP 2 — *INSTALLING MYSQL*

`$ sudo apt install mysql-server`

`$ sudo mysql`

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

`mysql> exit`

`$ sudo mysql_secure_installation`

`$ sudo mysql -p`

`mysql> exit`

![MySQL Installed](./Images/MySQL-Connected.PNG)


## STEP 3 — *INSTALLING PHP*

`sudo apt install php libapache2-mod-php php-mysql`

`php -v`

![PHP Installed](./Images/PHP-Installed.PNG)

## STEP 4 — *CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE*

`sudo mkdir /var/www/projectlamp`

`sudo chown -R $USER:$USER /var/www/projectlamp`

`sudo vi /etc/apache2/sites-available/projectlamp.conf`

`<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>`

`sudo ls /etc/apache2/sites-available`

`sudo a2ensite projectlamp`

`sudo a2dissite 000-default`

`sudo apache2ctl configtest`

`sudo systemctl reload apache2`

`sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html`

![VirtualHost Created](./Images/VirtualHost-Created.PNG)

![Test VirtualHost](./Images/Test-Virtualhost.PNG)

[Website](http://18.212.238.148:80)

## STEP 5 — *ENABLE PHP ON THE WEBSITE*

`sudo vim /etc/apache2/mods-enabled/dir.conf`

`<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>`

`sudo systemctl reload apache2`

`vim /var/www/projectlamp/index.php`

`<?php
phpinfo();`

save

![PHP Running](./Images/PHP-Running.PNG)











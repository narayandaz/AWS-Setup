# INSTALL LAMP STACK ON AWS EC2. (UBUNTU)

Installing apache2 and its dependencies.
```bash
sudo apt update
sudo apt upgrade
sudo apt install lamp-server^
```
Update the Firewall 
```bash
sudo ufw app list

sudo ufw app info "Apache Full"
sudo ufw allow in "Apache Full"
```
<sub>*server is installed successfully. to check just copy ip address from ec2 and paste in browser - http://your_server_ip *</sub>

Install phpmyadmin

<sub>Warning: When the prompt appears, “apache2” is highlighted, but not selected. If you do not hit SPACE to select Apache, the installer will not move the necessary files during installation. Hit SPACE, TAB, and then ENTER to select Apache.</sub>
```bash
sudo apt install phpmyadmin php-mbstring php-php-gettext gettext
sudo phpenmod mbstring
sudo systemctl restart apache2
```
*check: hhtp://your_ip/phpmyadmin*

Setup MySQL
```bash
#step1
sudo mysql_secure_installation

#step2
sudo mysql
SELECT user,authentication_string,plugin,host FROM mysql.user;

# make sure replace the "narayan@123" with yours.
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'narayan@123';
FLUSH PRIVILEGES;

SELECT user,authentication_string,plugin,host FROM mysql.user;
exit
```
Change Index file priority to index.php first (if required)
```bash
sudo nano /etc/apache2/mods-enabled/dir.conf
```
code would be:
```bash
<IfModule mod_dir.c>
  DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>	
```
Restart the apache2 service
```bash
sudo systemctl restart apache2
sudo systemctl status apache2
```
Testing PHP Processing on your Web Server
```bash
sudo vim /var/www/html/info.php
```
code: 
```bash
<?php
  phpinfo();
?>
```	
The address you will want to visit is:
```bash
http://your_ip/info.php
```	
*You will get the php info page*

Set proper file permissions on  Directories and files.
```bash
sudo chown -R ubuntu:root /var/www/html
sudo find html -type d -exec chmod 775 {} \;
sudo find html -type f -exec chmod 664 {} \;
```
If *.htaccess* file not working
```bash
sudo vim /etc/apache2/apache2.conf
```
Change the setting as below : AllowOverride All
```bash
<Directory /var/www/html>
    Options Indexes FollowSymLinks MultiViews
    AllowOverride All
    Require all granted
</Directory>
```
then restart your services
```bash
sudo a2enmod rewrite
sudo service apache2 restart
```
check status
```bash
apachectl configtest
#Syntax OK

apachectl -t
#Syntax OK
```	


# A list of commands

It contains all commands that were used to configure the machine.

```
# Setup nginx
sudo su
apt-get install nginx

# Setup java8 (required for rundeck)
echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee /etc/apt/sources.list.d/webupd8team-java.list
echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886
apt-get update
apt-get install oracle-java8-installer

# Setup rundeck
wget http://dl.bintray.com/rundeck/rundeck-deb/rundeck_2.11.4-1-GA_all.deb
dpkg -i rundeck_2.11.4-1-GA_all.deb

# Setup nginx reverse proxy for rundeck (rundeck.web.local points to localhost:4440)
rm -rf /etc/nginx/sites-enabled/*   # Clear enabled site configurations
rm -rf /etc/nginx/sites-available/* # Clear existing site configurations
cp /vagrant/.conf/nginx/sites-available/* /etc/nginx/sites-available/ # Copy site configurations to nginx
ln -s /etc/nginx/sites-available/rundeck /etc/nginx/sites-enabled/rundeck # Enable rundeck config
ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default # Enable default config (catch-all for apache2)
service nginx restart

# Setup apache2
apt-get install apache2 -y
cp /vagrant/.conf/apache2/ports.conf /etc/apache2/ports.conf

rm -rf /var/www/html
rm -rf /etc/apache2/sites-enabled/*
rm -rf /etc/apache2/sites-available/*

cp /vagrant/.conf/apache2/sites-available/* /etc/apache2/sites-available/
a2ensite 00-default
a2ensite 01-adminer

service apache2 restart

# Setup adminer
cd /var/www
mkdir adminer
cd adminer
wget https://github.com/vrana/adminer/releases/download/v4.6.2/adminer-4.6.2-mysql.php
mv adminer-4.6.2-mysql.php index.php

# Setup php7.2
add-apt-repository ppa:ondrej/php
apt-get update
apt-get install php php-mysql -y

# Setup mysql-server
apt-get install mysql-server -y

# Setup certbot for ssl
apt-get update
apt-get install software-properties-common
add-apt-repository ppa:certbot/certbot
apt-get update
apt-get install python-certbot-nginx 

# See official documentation: https://certbot.eff.org/lets-encrypt/ubuntutrusty-nginx
# Only works when site is online
```
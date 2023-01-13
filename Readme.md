# Domain Create

## 1. install apache server and php

```
sudo apt update
sudo apt upgrade
sudo reboot
sudo apt-get update
sudo apt-get install apache2 php
```
## 2. Create a folder which you want to use as root directory of your server

```
sudo mkdir -p /home/mark/www
sudo chmod -R 755 /home/mark/www
```
## 3. Create domain name in your hosts file under ‘/etc/hosts’

```
sudo nano /etc/hosts
```
Enter your domain name in front of localhost IP as given in the figure. Here we are using mark.com, so we are writing ‘127.0.1.1 mark.com’. 

```
127.0.0.1 localhost
127.0.1.1 server
127.0.1.1 mark.com
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

## 4. Copy default apache2 configuration file for your new domain name configuration
```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/mark.com.conf
```

## 5. Add entries to our configuration file
```
sudo nano /etc/apache2/sites-available/mark.com.conf
```

```
<VirtualHost *:80>
	# The ServerName directive sets the request scheme, hostname and port that
	# the server uses to identify itself. This is used when creating
	# redirection URLs. In the context of virtual hosts, the ServerName
	# specifies what hostname must appear in the request's Host: header to
	# match this virtual host. For the default virtual host (this file) this
	# value is not decisive as it is used as a last resort host regardless.
	# However, you must set it for any further virtual host explicitly.
	ServerName mark.com

	#ServerAdmin webmaster@localhost
	DocumentRoot /home/mark/www/

	# Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
	# error, crit, alert, emerg.
	# It is also possible to configure the loglevel for particular
	# modules, e.g.
	#LogLevel info ssl:warn

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

	# For most configuration files from conf-available/, which are
	# enabled or disabled at a global level, it is possible to
	# include a line for only one particular virtual host. For example the
	# following line enables the CGI configuration for this host only
	# after it has been globally disabled with "a2disconf".
	#Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

## 6. Disable the default configuration and enable our new configuration
```
sudo a2dissite 000-default.conf
sudo a2ensite mark.com.conf
sudo systemctl reload apache2
```
## 7. Update apache2 config file and reload the apache2 service
```
sudo nano /etc/apache2/apache2.conf
```
Add these lines to your apache2.conf file
```
<Directory /home/mark/www/>
	Options Indexes FollowSymLinks
	AllowOverride None
	Require all granted
</Directory>
```
Reload the apcahe2 service
```
sudo systemctl reload apache2
```

## 8. You are ready now check by typing your URL to the browser. You can test by writing a simple PHP script in www folder.

```
<!DOCTYPE html>
<html>
  <head>
    <title>Home Page</title>
  </head>
  <body>
    <div>
      <?php echo phpinfo(); ?>
    </div>
  </body>
</html>
```

# Apache Subdomain create
## 1. Create sub domain hosts
```
sudo nano /etc/hosts
```

Please add the line in the  /etc/hosts
```
127.0.1.1 subdomain.mark.com
```

## 2. Create your sub-domain document root directories
```
sudo mkdir -p /var/www/subdomains/apache
sudo chmod -R 777 /var/www/subdomains/
```
## 3. Now create virtual hosts in Apache configuration file
```
cd /etc/apache2/sites-available/
sudo cp mark.com.conf apache.mark.com.conf
sudo nano apache.mark.com.conf
```
Edit apache.mark.com.conf file like following
```
<VirtualHost *:80>
        ServerName apache.mark.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/subdomains/apache/

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
## 4. Update apache2 config file and reload the apache2 service
```
sudo nano /etc/apache2/apache2.conf
```
Add these lines to your apache2.conf file
```
<Directory /var/www/subdomains/apache/>
	Options Indexes FollowSymLinks
	AllowOverride None
	Require all granted
</Directory>
```
Reload the apcahe2 service
```
sudo systemctl reload apache2
```
## 5. Enable our new configuration
```
sudo a2ensite apache.mark.com.conf
```
# Reference
## Domain Create
https://www.geeksforgeeks.org/creating-custom-domain-name-instead-of-localhost-in-ubuntu/
## Subdomain Create
https://www.binarytides.com/create-localhost-sub-domains-apache-ubuntu/
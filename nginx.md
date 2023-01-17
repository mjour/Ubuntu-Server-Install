# 1. Installing Nginx
```
sudo apt update
sudo apt install nginx
```

# 2. Setting up web directories for nginx domain and subdomain
## 1. Create web directories for all domains and subdomains.
```
sudo mkdir /var/www/html/mark.com
sudo mkdir /var/www/html/web1.mark.com
sudo mkdir /var/www/html/web2.mark.com
```
## 2. Change the ownership of each directory
```
sudo chown -R www-data:www-data /var/www/html/mark.com
sudo chown -R www-data:www-data /var/www/html/web1.mark.com
sudo chown -R www-data:www-data /var/www/html/web2.mark.com
```
## 3. Create a file named index.html inside your domain's primary web directory
```
<html>
	<head>
	  <title>Welcome to mark.com!</title>
  </head>
	  <body>
	    <h1>Congratulations! The mark.com website is working!</h1>
    </body>
</html>
```
# 3. Setting up virtual host for nginx domain and subdomain
## 1. Create an NGINX virtual host configuration file named mark.com in the /etc/nginx/sites-available/ directory.
```
server {
    # Binds the TCP port 80.
    listen 80; 

    # Root directory used to search for a file
    root /var/www/html/mark.com;
    # Defines the file to use as index page
    index index.html index.htm;
    # Defines the domain or subdomain name. 
    # If no server_name is defined in a server block then 
    # Nginx uses the 'empty' name
    server_name mark.com;

    location / {
        # Return a 404 error for instances when the server receives 
        # requests for untraceable files and directories.
        try_files $uri $uri/ =404;
    }
}
```
## 2. Next, run the following nginx command to check (-t) the NGINX configuration file for any syntax error.
```
sudo nginx -t
```
## 3. Create a symbolic link from the /etc/nginx/sites-available to the /etc/nginx/sites-enabled/ directory. 
```
sudo ln -s /etc/nginx/sites-available/mark.com /etc/nginx/sites-enabled/
```
## 4. Repeat steps one to three to create NGINX virtual host configuration files named web1.mark.com and web2.mark.com.
・Replace the line root /var/www/html/mark.com with the webroot directory of each subdomain (root /var/www/html/web1.mark.com and root /var/www/html/web2.mark.com).

・Replace the line server_name mark.com with the name of each subdomain (server_name web1.mark.com and server_name web2.mark.com).

```
sudo ln -s /etc/nginx/sites-available/web1.mark.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/web2.mark.com /etc/nginx/sites-enabled/
```

## 5. Restart the nginx
```
sudo systemctl restart nginx
```

## Reference
https://adamtheautomator.com/nginx-subdomain/
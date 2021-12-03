# How-To-Install-WordPress-On-Ubuntu-18.04-With-LAMP
In this tutorial, I will show you how to install WordPress with LAMP on Ubuntu 18.04

# Prerequisites for installing WordPress on Ubuntu 18.04

Before we get started, you'll need to have the following set up:

> LAMP stack: 
- LAMP stands for Linux
- Apache 
- MySQL 
- PHP
- WordPress is both a front end and a backend system, so it requires a web server, a database engine, and PHP for serving dynamic content—that’s what LAMP does.

# SSH access to the server

# Create a database for WordPress user
```
mysql -u root -p
```
```
mysql> CREATE DATABASE wordpressdb;
```
```
mysql> GRANT ALL ON wordpress.* TO 'admin-suser'@'localhost' IDENTIFIED BY 'PASSWORD';
```
```
mysql> FLUSH PRIVILEGES;
```
```
mysql>   EXIT;
```

Install additional PHP extensions

With that in mind, we're now going to install additional PHP extensions for WordPress.
First, update the system:  	
```
sudo apt update
```	
  
Next, install the additional PHP extensions:
```
sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-  soap php-intl php-zip
```
To load these extensions, restart Apache web server by running the following command:
```
sudo systemctl restart apache2
```

# Download WordPress
With all the prerequisites in place, let’s go ahead and download WordPress.
For security reasons, I recommend always downloading WordPress from its official repository:
First Navigate to /var/www/ directory

```
cd  /var/www/```
```

Then download the zipped folder using the command
             	    
```
curl -O https://wordpress.org/latest.tar.gz
```
Extract the tarball file
```			
tar -xvf latest.tar.gz
```

Configure the WordPress directory
Before we proceed to the next step, we need to adjust ownership and file permissions of the WordPress directory.
Let’s assign file ownership to all the files in the WordPress directory using the


```
sudo chown -R www-data:www-data /var/www/wordpress
```

We also need to rename the sample configuration file in the WordPress directory to a filename it can read from:

```
cd /var/www/wordpress
```
```
mv wp-config-sample.php wp-config.php                             
```

Next, we will open the wp-config.php file using the default text editor Vim.
```                         
 vim  wp-config.php
```
 
Now scroll down and locate the database settings as shown below. Be sure to fill in the WordPress database name, database user, database password and hostname.
- // ** MySQL settings - You can get this info from your web host ** //
- /** The name of the database for WordPress */
define('DB_NAME', 'wordpressdb');
- /** MySQL database username */
define('DB_USER', 'admin-user');
- /** MySQL database password */
define('DB_PASSWORD', 'StrongPassword');
- /** MySQL hostname */
define('DB_HOST', 'localhost');
- /** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');
- /** The Database Collate type. Don't change this if in doubt. */
- define('DB_COLLATE', '');

# Save and exit the configuration file.

# Modify Apache configuration
In this step, we need to make a few adjustments to the default configuration file 000-default.conf in the path /etc/apache2/sites-available.
Start by opening the default configuration file

```
vim  /etc/apache2/sites-available/000-default.conf
```

Next, locate the DocumentRoot attribute and change it from /var/www/html to /var/www/wordpress.
In the same file, copy and paste the following lines inside the Virtual Host block.
```
vim  /etc/apache2/sites-available/000-default.conf
```
Next, locate the DocumentRoot attribute and change it from /var/www/html to /var/www/wordpress.
In the same file, copy and paste the following lines inside the Virtual Host block.

```
<Directory /var/www/wordpress/>
			AllowOverride All
			</Directory>
```
# Save and exit the configuration file.
Next, you need to enable the mod_rewrite so that you can use WordPress Permalink feature.

```
sudo a2enmod rewrite
```

To verify that all went well, execute the command.
```
sudo apache2ctl configtest
```

Output: Ok

To implement the changes, restart Apache web server.


```
sudo systemctl restart apache2
```
Run WordPress installation using the web browser
At this point, you’ve finished all the server configurations for your WordPress installation.
The final step is to complete the installation via a web browser.
To do this, launch your web browser and browser your server’s IP address or domain name
http://server_IP_address or http://YOUR-DOMAIN
The first page will prompt you to select the language.

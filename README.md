# Hosted-WordPress-Website-On-Amazon-Aws
Provides a step-by-step guide and configuration files for deploying a WordPress website on an Amazon Web Services (AWS) Elastic Compute Cloud (EC2) instance with custom domain name (kiransinghverma.online)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
#Step 1: Launch an instance
AMI — Ubuntu Server
Instance Type — t2.micro
Key Pair — ppk file format
Firewall (security groups) — ssh, http, https
Configuration Storage — 8 GB, gp2
No. of Instance — 01
→ Launch Instance
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Step 2: Associate Elastic IP address

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Step 3: Connect via SSH Client (Putty)

I have connected to virtual computer…
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Step 4: Installing and Configuring mysql server:
# Install Apache server on Ubuntu
sudo apt install apache2 -y

Install php runtime because WordPress build on php

# Install php runtime and php mysql connector
sudo apt install php libapache2-mod-php php-mysql -y
# Install MySQL server
sudo apt install mysql-server -y
# Login to MySQL server
sudo mysql -u root
# Change authentication plugin to mysql_native_password (change the password to something strong)
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'admin@123';

# Create a new database user for wordpress (change the password to something strong)
CREATE USER 'wp_user'@localhost IDENTIFIED BY 'admin@123';

# Create a database for wordpress
CREATE DATABASE wp;

# Grant all privilges on the database 'wp' to the newly created user
GRANT ALL PRIVILEGES ON wp.* TO 'wp_user'@localhost;

Exit or ctrl-D
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Step 5: Download, Configure & Install Wordpress:
cd /tmp
wget https://wordpress.org/latest.tar.gz
# Unzip
tar -xvf latest.tar.gz

# Move wordpress folder to apache document root
sudo mv wordpress/ /var/www/html


ip-address/wordpress

It threw an error, doesn't matter copy the Configuration rules. create the wp-config.php file manually and paste the following text into it.

cd /var/www/html/wordpress
vi wp-config.php


# ip-address followed by wordpress
34.239.124.228/wordpress/

I dont want my website go to the subpath (34.239.124.228/wordpress) like this. I want it to serve on root directory like this (34.239.124.228) like this, as of now root directory serve the apache root default page, but I want Wordpress website to the server at this root path. for that I need to modify apache configuration, so I go back to my terminal rest follow below commands.
cd /etc/apache2/sites-available/
vi 000-default.conf

Change document root to /var/www/html/wordpress
sudo systemctl restart apache2

http://34.239.124.228/ # hit on browser
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Step 6: Link Domain name to our website
I purchased a domain called skyvaults.tech from Hostinger

Here in Points to — ec2 instance public ip address
Add Record
cd /etc/apache2/sites-available/
vi 000-default.conf

sudo systemctl restart apache2

skyvaults.tech # hit on browser probably it takes 15-20 minutes to reflect wait until 
Now I can access WordPress website by my domain name.
Step 7: Secure Connection (HTTPS)
# install certbot
sudo apt-get update
sudo apt install certbot python3-certbot-apache -Y
# Request and install ssl on your site with certbot
sudo certbot --apache

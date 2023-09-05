# Hosted-WordPress-Website-On-Amazon-Aws
Provides a step-by-step guide and configuration files for deploying a WordPress website on an Amazon Web Services (AWS) Elastic Compute Cloud (EC2) instance with custom domain name (kiransinghverma.online)

![aws](https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/ea4376b8-5a12-4c6f-9cc9-c24f9dd63a15)

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Step 1: Launch an instance

AMI — Ubuntu Server
Instance Type — t2.micro
Key Pair — ppk file format
Firewall (security groups) — ssh, http, https
Configuration Storage — 8 GB, gp2
No. of Instance — 01
→ Launch Instance
<img width="794" alt="Screenshot 2023-09-05 023648" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/43a6c184-b22f-4e80-b5c2-bfc1a2f631fd">

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Step 2: Associate Elastic IP address

<img width="794" alt="Screenshot 2023-09-05 023628" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/184f1156-4366-4112-b8e4-e070d249e922">
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Step 3: Connect via SSH Client (Putty)


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Step 4: Installing and Configuring mysql server:

# Install Apache server on Ubuntu
sudo apt install apache2 -y

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
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Step 5: Download, Configure & Install Wordpress:
cd /tmp
wget https://wordpress.org/latest.tar.gz
# Unzip
tar -xvf latest.tar.gz

# Move wordpress folder to apache document root
sudo mv wordpress/ /var/www/html





# create the wp-config.php
cd /var/www/html/wordpress
vi wp-config.php


# ip-address followed by wordpress
3.228.200.37/wordpress/

I want Wordpress website to the server at this root path. for that I need to modify apache configuration, so I go back to my terminal rest follow below commands.

cd /etc/apache2/sites-available/
vi 000-default.conf

# Change document root to /var/www/html/wordpress
sudo systemctl restart apache2

http://3.228.200.37/ # hit on browser
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Step 6: Link Domain name to our website

I purchased a domain called kiransinghverma.online from Hostinger

Here in Points to — ec2 instance public ip address
Add Record
cd /etc/apache2/sites-available/
vi 000-default.conf

sudo systemctl restart apache2

kiransinghverma.online # hit on browser probably it takes 15-20 minutes to reflect wait until 
Now I can access WordPress website by my domain name.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Step 7: Secure Connection (HTTPS)

# install certbot
sudo apt-get update
sudo apt install certbot python3-certbot-apache -Y
# Request and install ssl on your site with certbot
sudo certbot --apache
# This will secure the connection #

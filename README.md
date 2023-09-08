# Hosted-WordPress-Website-On-Amazon-Aws
Provides a step-by-step guide and configuration files for deploying a WordPress website on an Amazon Web Services (AWS) Elastic Compute Cloud (EC2) instance with custom domain name (kiransinghverma.online)

![aws](https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/ea4376b8-5a12-4c6f-9cc9-c24f9dd63a15)

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Step 1: Launch an instance

-> AMI — Ubuntu Server
-> Instance Type — t2.micro
-> Key Pair — ppk file format
-> Firewall (security groups) — ssh, http, https
-> Configuration Storage — 8 GB, gp2
-> No. of Instance — 01
→ Launch Instance
<img width="794" alt="Screenshot 2023-09-05 023648" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/43a6c184-b22f-4e80-b5c2-bfc1a2f631fd">

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Step 2: Associate Elastic IP address

<img width="794" alt="Screenshot 2023-09-05 023628" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/184f1156-4366-4112-b8e4-e070d249e922">


# Step 3: Connect via SSH Client (Putty)
<img width="513" alt="Screenshot 2023-09-05 115832" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/a264c96d-39c1-4734-8e2c-ace3307ecf8c">


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Step 4: Installing and Configuring mysql server:
<img width="533" alt="Screenshot 2023-09-05 115850" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/4d8dd582-aa1e-48e3-96a4-cdc99a3d5620">

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

# Step 5: Download, Configure & Install Wordpress:
<img width="527" alt="Screenshot 2023-09-05 120019" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/f76e8c6a-7158-4000-8976-6e3ece8a86cf">

cd /tmp
wget https://wordpress.org/latest.tar.gz
# Unzip
tar -xvf latest.tar.gz

# Move wordpress folder to apache document root
sudo mv wordpress/ /var/www/html

<img width="528" alt="Screenshot 2023-09-05 120035" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/7542104f-29fe-4031-be74-b0ed60388e4e">

<img width="518" alt="Screenshot 2023-09-05 120104" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/c213c553-baf6-473e-a43a-6df22b3013c9">

# create the wp-config.php
cd /var/www/html/wordpress
vi wp-config.php


# ip-address followed by wordpress
3.89.166.68/wordpress/

I want Wordpress website to the server at this root path. for that I need to modify apache configuration, so I go back to my terminal rest follow below commands.

cd /etc/apache2/sites-available/
vi 000-default.conf

<img width="520" alt="Screenshot 2023-09-05 120730" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/1348cb06-3a53-4735-8387-9dd32f557a50">

<img width="517" alt="Screenshot 2023-09-05 120755" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/5eeb3714-d249-4636-8701-830e54e8bd03">

# Change document root to /var/www/html/wordpress
sudo systemctl restart apache2

http://3.89.166.68/ # hit on browser

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Step 6: Link Domain name to our website

<img width="680" alt="Screenshot 2023-09-05 123415" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/73c38b34-cb4c-4450-a2e8-31f8cd8b5842">

I purchased a domain called kiransinghverma.online from Hostinger

Here in Points to — ec2 instance public ip address
Add Record
cd /etc/apache2/sites-available/
vi 000-default.conf

sudo systemctl restart apache2

kiransinghverma.online # hit on browser probably it takes 15-20 minutes to reflect wait until 
Now I can access WordPress website by my domain name.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Step 7: Secure Connection (HTTPS)

<img width="364" alt="Screenshot 2023-09-05 123521" src="https://github.com/Kiran090303/Hosted-WordPress-Website-On-Amazon-Aws/assets/98480971/4d9817e3-04cd-4f5e-926b-13c5af385472">

# install certbot
sudo apt-get update
sudo apt install certbot python3-certbot-apache -Y
# Request and install ssl on your site with certbot
sudo certbot --apache
# This will secure the connection #

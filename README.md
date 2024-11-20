# BrainSt-Assignment

# Task:
Your task is to set up an automated deployment process for a WordPress website
using Nginx as the web server, LEMP (Linux, Nginx, MySQL, PHP) stack, and GitHub
Actions as the CI/CD automation tool. The deployment process should follow security
best practices and ensure optimal performance of the website.

# 1. Provision a Virtual Private Server (VPS) on AWS:
![image](https://github.com/user-attachments/assets/d839b0a1-0e2d-4a53-b752-d668035404a3)
![image](https://github.com/user-attachments/assets/bef6e5d6-60ec-404f-9ab4-151c76d6b253)
![image](https://github.com/user-attachments/assets/8a907c52-a194-4743-8134-6c8672738bc0)
![image](https://github.com/user-attachments/assets/836b5b9d-6d2e-404b-ba24-46c18a4dd7d6)

# Connect to the Instance:

# Update the System:
sudo apt update && sudo apt upgrade -y
![image](https://github.com/user-attachments/assets/b56218c3-ea6c-4c35-8536-f9e44803590b)

# 2. Install the LEMP Stack
# Install Nginx:
sudo apt install nginx -y 


# Install MySQL/MariaDB:
sudo apt install mysql-server -y 
sudo mysql_secure_installation
![image](https://github.com/user-attachments/assets/537bac01-5f9d-400b-880c-6f699d710dff)
![image](https://github.com/user-attachments/assets/d79f4b91-86b1-44ed-8e87-55c091565cc1)

Set a strong root password and follow the prompts for security.

# 2. Install the LEMP Stack
Install Nginx:
sudo apt install nginx -y
![alt text](image-6.png)
![alt text](image-17.png)

# Install PHP:
Configure Nginx to Use PHP:

sudo apt install php-fpm php-mysql -y

![image](https://github.com/user-attachments/assets/fa0e6114-f2f2-4d80-bd21-b96a5336e577)

# Create a new configuration file for WordPress:

sudo nano /etc/nginx/sites-available/wordpress
![image](https://github.com/user-attachments/assets/7aee093a-efed-44fd-9660-b2477649b317)


Add the following configuration:

server {
    listen 80;
    server_name ap4ashutosh.xyz www.ap4ashutosh.xyz;
    root /var/www/wordpress;

    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}


sudo ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx

![image](https://github.com/user-attachments/assets/d91ad831-b35f-4e36-93b4-616539575c31)
![image](https://github.com/user-attachments/assets/7ab29218-46c6-4ede-afaf-2b11574c36d4)



# 3. Set Up WordPress
Download WordPress:

cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xvf latest.tar.gz
sudo mv wordpress /var/www/

![image](https://github.com/user-attachments/assets/ca84674a-c75b-4563-b9c2-9ae833c5d06b)


# Set Permissions:

sudo chown -R www-data:www-data /var/www/wordpress 
sudo chmod -R 755 /var/www/wordpress


Create a Database for WordPress:
![image](https://github.com/user-attachments/assets/411f28bd-5f50-4462-8d98-9f0ed40aa6da)


# Create a Database for WordPress:

sudo mysql -u root -p


# Run the following MySQL commands:

CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'Subham@966';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;

![image](https://github.com/user-attachments/assets/abb65c90-7577-49ae-b2db-931ea77e59fd)


# Copy the sample configuration file:

cp /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php

# Edit the configuration file:

sudo nano /var/www/wordpress/wp-config.php

![image](https://github.com/user-attachments/assets/0cb72ab7-6162-459b-9a01-d7b723da8848)
![image](https://github.com/user-attachments/assets/0090d70c-9771-4bd4-8db7-58365ca9ea65)


# Update the database details:

define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpressuser');
define('DB_PASSWORD', 'Subham@966');

![image](https://github.com/user-attachments/assets/2b6a6bb6-c8d4-4207-9712-2d971ffc2d27)

# Navigate to http://your-ec2-public-ip to complete the WordPress setup.

![alt text](image-18.png)

# Installing Additional PHP Extensions
sudo apt update
sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
sudo systemctl restart apache2

# Enabling .htaccess Overrides:
sudo nano /etc/apache2/sites-available/wordpress.conf

<VirtualHost *:80>
. . .
    <Directory /var/www/wordpress/>
        AllowOverride All
    </Directory>
. . .
</VirtualHost>


![alt text](3.png)
![alt text](2.png)

sudo a2enmod rewrite
sudo apache2ctl configtest
sudo systemctl restart apache2

# Downloading WordPress:
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz
touch /tmp/wordpress/.htaccess
cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
mkdir /tmp/wordpress/wp-content/upgrade
sudo cp -a /tmp/wordpress/. /var/www/wordpress

# Navigate to http://your-ec2-public-ip to complete the WordPress setup.
![alt text](image-20.png)

# 4. Secure the Website
Install Certbot for SSL/TLS:

sudo apt install certbot python3-certbot-nginx -y 
![alt text](image-19.png)

sudo certbot --nginx -d ap4ashutosh.xyz -d www.ap4ashutosh.xyz

Follow the prompts to obtain and configure the SSL certificate.

# Set Up Firewall:

sudo ufw allow OpenSSH 
sudo ufw allow 'Nginx Full' 
sudo ufw enable

OR,

# ACM with LoadBalancer with Route53 :
![alt text](image-23.png)
![alt text](image-24.png)
![alt text](image-26.png)
![alt text](image-25.png)
![alt text](image-28.png)
![alt text](image-27.png)

# Hit the LB endpoint:
![alt text](image-30.png)
![alt text](image-29.png)

# 5. Automate Deployment with GitHub Actions
Set Up GitHub Repository:

# Create a new repository and clone it locally:

git clone https://github.com/your-username/wordpress-deployment.git

Add your WordPress files and commit:

cd wordpress-deployment 
git add . 
git commit -m "Initial commit" 
git push origin main


# Create a GitHub Actions Workflow:

Add a .github/workflows/deploy.yml file:

name: Deploy WordPress

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up SSH
      uses: webfactory/ssh-agent@v0.5.3
      with:
        ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

    - name: Sync files to server
      run: |
        rsync -avz --delete ./ ubuntu@your-ec2-public-ip:/var/www/wordpress
        ssh ubuntu@your-ec2-public-ip "sudo systemctl restart nginx"

# Add SSH_PRIVATE_KEY in the repository's Settings > Secrets and variables > Actions.

# Test the Workflow:

# Push changes to the main branch and verify the deployment.


# 


# 
# 

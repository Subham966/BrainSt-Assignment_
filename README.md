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
![image](https://github.com/user-attachments/assets/b56218c3-ea6c-4c35-8536-f9e44803590b)

# 2. Install the LEMP Stack
# Install Nginx:
sudo apt install nginx -y 


# Install MySQL/MariaDB:
sudo apt install mysql-server -y 
sudo mysql_secure_installation
Set a strong root password and follow the prompts for security.

===========================

# BrainSt-Assignment

# Task:
Your task is to set up an automated deployment process for a WordPress website
using Nginx as the web server, LEMP (Linux, Nginx, MySQL, PHP) stack, and GitHub
Actions as the CI/CD automation tool. The deployment process should follow security
best practices and ensure optimal performance of the website.

1. Provision a Virtual Private Server (VPS) on AWS:
![alt text](image.png)
![alt text](image-1.png)
![alt text](image-2.png)
![alt text](image-3.png)

# Update the System:
sudo apt update && sudo apt upgrade -y
![alt text](image-5.png)
![alt text](image-4.png)

# 2. Install the LEMP Stack
Install Nginx:
sudo apt install nginx -y
![alt text](image-6.png)
![alt text](image-17.png)

# Install MySQL/MariaDB:

sudo apt install mysql-server -y 
sudo mysql_secure_installation

![alt text](image-7.png)
![alt text](image-8.png)


# Install PHP:
Configure Nginx to Use PHP:

sudo apt install php-fpm php-mysql -y
![alt text](image-9.png)



# Create a new configuration file for WordPress:

sudo nano /etc/nginx/sites-available/wordpress
![alt text](image-10.png)
Add the following configuration:

server {
    listen 80;
    server_name your-domain.com www.your-domain.com;
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
![alt text](image-11.png)
![alt text](image-12.png)


# 3. Set Up WordPress
Download WordPress:

cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xvf latest.tar.gz
sudo mv wordpress /var/www/
![alt text](image-14.png)

# Set Permissions:

sudo chown -R www-data:www-data /var/www/wordpress 
sudo chmod -R 755 /var/www/wordpress
![alt text](image-13.png)

Create a Database for WordPress:

sudo mysql -u root -p


# Run the following MySQL commands:

CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'Subham@966';
![alt text](image-15.png)
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
![alt text](image-16.png)

# Copy the sample configuration file:

cp /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php

# Edit the configuration file:

sudo nano /var/www/wordpress/wp-config.php


# Update the database details:

define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpressuser');
define('DB_PASSWORD', 'Subham@966');
![alt text](image-18.png)

# Navigate to http://your-ec2-public-ip to complete the WordPress setup.


# 4. Secure the Website
Install Certbot for SSL/TLS:

sudo apt install certbot python3-certbot-nginx -y 
![alt text](image-19.png)

sudo certbot --nginx -d your-domain.com -d www.your-domain.com

Follow the prompts to obtain and configure the SSL certificate.

# Set Up Firewall:

sudo ufw allow OpenSSH 
sudo ufw allow 'Nginx Full' 
sudo ufw enable


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

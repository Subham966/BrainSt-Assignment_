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
```bash
sudo apt update && sudo apt upgrade -y
```
![image](https://github.com/user-attachments/assets/b56218c3-ea6c-4c35-8536-f9e44803590b)

# 2. Install the LEMP Stack
# Install Nginx:
```bash
sudo apt install nginx -y 
```

# Install MySQL/MariaDB:
```bash
sudo apt install mysql-server -y 
sudo mysql_secure_installation
```
![image](https://github.com/user-attachments/assets/537bac01-5f9d-400b-880c-6f699d710dff)
![image](https://github.com/user-attachments/assets/d79f4b91-86b1-44ed-8e87-55c091565cc1)

Set a strong root password and follow the prompts for security.

# Install PHP:
Configure Nginx to Use PHP:
```bash
sudo apt install php-fpm php-mysql -y
```
![image](https://github.com/user-attachments/assets/fa0e6114-f2f2-4d80-bd21-b96a5336e577)

# Create a new configuration file for WordPress:
```bash
sudo nano /etc/nginx/sites-available/wordpress
![image](https://github.com/user-attachments/assets/7aee093a-efed-44fd-9660-b2477649b317)


Add the following configuration:
```bash
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

```

```bash
sudo ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```
![image](https://github.com/user-attachments/assets/d91ad831-b35f-4e36-93b4-616539575c31)
![image](https://github.com/user-attachments/assets/7ab29218-46c6-4ede-afaf-2b11574c36d4)



# 3. Set Up WordPress
Download WordPress:
```bash
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xvf latest.tar.gz
sudo mv wordpress /var/www/
```
![image](https://github.com/user-attachments/assets/ca84674a-c75b-4563-b9c2-9ae833c5d06b)


# Set Permissions:
```bash
sudo chown -R www-data:www-data /var/www/wordpress 
sudo chmod -R 755 /var/www/wordpress
```

Create a Database for WordPress:
![image](https://github.com/user-attachments/assets/411f28bd-5f50-4462-8d98-9f0ed40aa6da)


# Create a Database for WordPress:
```bash
sudo mysql -u root -p
```

# Run the following MySQL commands:
```bash
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'Subham@966';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
![image](https://github.com/user-attachments/assets/abb65c90-7577-49ae-b2db-931ea77e59fd)


# Copy the sample configuration file:
```bash
cp /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php
```
# Edit the configuration file:
```bash
sudo nano /var/www/wordpress/wp-config.php
```
![image](https://github.com/user-attachments/assets/0090d70c-9771-4bd4-8db7-58365ca9ea65)


# Update the database details:
```bash
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpressuser');
define('DB_PASSWORD', 'Subham@966');
```
![image](https://github.com/user-attachments/assets/2b6a6bb6-c8d4-4207-9712-2d971ffc2d27)

# Installing Additional PHP Extensions:
```bash
sudo apt update
sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
sudo systemctl restart apache2
```

# Enabling .htaccess Overrides:
```bash
sudo nano /etc/apache2/sites-available/wordpress.conf
```
```bash
<VirtualHost *:80>
. . .
    <Directory /var/www/wordpress/>
        AllowOverride All
    </Directory>
. . .
</VirtualHost>
```

![image](https://github.com/user-attachments/assets/b18eaf7a-0b30-4905-b54d-579867b63e57)

![image](https://github.com/user-attachments/assets/a74cba7f-fd0f-4b8f-865f-38e0faf8f2db)

```bash
sudo a2enmod rewrite
sudo apache2ctl configtest
sudo systemctl restart apache2
```

# Downloading WordPress:
```bash
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz
touch /tmp/wordpress/.htaccess
cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
mkdir /tmp/wordpress/wp-content/upgrade
sudo cp -a /tmp/wordpress/. /var/www/wordpress
```
# Navigate to http://your-ec2-public-ip to complete the WordPress setup.
![image](https://github.com/user-attachments/assets/4a95b73d-f933-46ac-9844-21dea22f54ff)


# 4. Secure the Website
Install Certbot for SSL/TLS:
```bash
sudo apt install certbot python3-certbot-nginx -y 
![alt text](image-19.png)
```
```bash
sudo certbot --nginx -d ap4ashutosh.xyz -d www.ap4ashutosh.xyz
```
Follow the prompts to obtain and configure the SSL certificate.

!! OR !!

# ACM with LoadBalancer with Route53 :
![image](https://github.com/user-attachments/assets/07537834-215d-444b-bdd4-c03013036a9e)

![image](https://github.com/user-attachments/assets/1ec85c4c-adf3-4553-b4bc-d07666f6c7f7)

![image](https://github.com/user-attachments/assets/292ec8a1-3242-4557-a38e-7e8fd68f7a69)

![image](https://github.com/user-attachments/assets/86e50cb1-a132-4b59-94e5-e0a387d82e2d)

![image](https://github.com/user-attachments/assets/97887feb-b107-4ecc-b392-8222c8b6b123)

![image](https://github.com/user-attachments/assets/75580048-20a9-45e4-8349-1c663a7eb85e)


# Hit the LB endpoint:
![image](https://github.com/user-attachments/assets/0e4ba8f4-2794-4421-9a0f-bc84e4782f11)

![image](https://github.com/user-attachments/assets/1c86f94d-37b5-465e-817b-9ab104986eb3)


# 5. Automate Deployment with GitHub Actions
Set Up GitHub Repository:

# Create a new repository and clone it locally:
```bash
git clone https://github.com/Subham966/BrainSt-Assignment_.git
cd BrainSt-Assignment_
cp -r /path/to/wordpress/* .  
git add .
git commit -m "Initial commit with WordPress files"
git push origin main
```

# Create a GitHub Actions Workflow:

Add a .github/workflows/deploy.yml file:

```bash
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

```
![image](https://github.com/user-attachments/assets/1c86f94d-37b5-465e-817b-9ab104986eb3)

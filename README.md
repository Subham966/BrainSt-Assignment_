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


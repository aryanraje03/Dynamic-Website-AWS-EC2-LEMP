# Dynamic Website Deployment on AWS EC2 (LEMP Stack)

##  Project Overview
A fully functional dynamic website where users submit
a signup form and data gets stored in MariaDB database
— hosted live on AWS EC2.

##  Tech Stack
| Technology | Purpose |
|------------|---------|
| AWS EC2 (Amazon Linux) | Cloud Server |
| Nginx | Web Server |
| PHP + FPM | Backend Processing |
| MariaDB 10.5 | Database |
| php8.5-mysqlnd | PHP-MySQL Connector |

##  Installation Steps

### 1. Install LEMP Stack
sudo yum install nginx mariadb105-server php php-fpm -y

### 2. Enable Services
sudo systemctl enable nginx mariadb php-fpm
sudo systemctl start nginx mariadb php-fpm

### 3. Deploy Files
cd /var/www/share/nginx/html
# Place signup.html and submit.php here

### 4. Setup Database
sudo mysql -u root -p
CREATE DATABASE FCT;
USE FCT;
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  website VARCHAR(200),
  gender VARCHAR(20),
  comment TEXT
);

### 5. Install PHP-MySQL Connector (Critical Step!)
sudo yum install php-mysqlnd.x86_64 -y

### 6. Restart All Services
sudo systemctl restart nginx mariadb php-fpm

##  Result
- Nginx: active (running)
- MariaDB: active (running)
- Form data successfully stored in database

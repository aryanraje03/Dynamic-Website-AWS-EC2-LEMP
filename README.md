# Dynamic Website Deployment on AWS EC2  
## Using NGINX, PHP & MariaDB (LEMP Stack)

---

## Project Overview

This project demonstrates the deployment of a **Dynamic Signup Form** web application on an Amazon EC2 instance using Amazon Linux.

The application allows users to submit their details through a web form, and the entered data is stored permanently in a **MariaDB** database using a **PHP** backend.

This project was completed as part of my **Cloud Learning Journey** to gain practical, hands-on experience in:

- Cloud server deployment on AWS
- Linux server administration
- Web server configuration with NGINX
- Backend integration using PHP
- Database setup and management with MariaDB

---

## Technologies Used

| Technology | Purpose |
|---|---|
| AWS EC2 (Amazon Linux) | Cloud Virtual Server |
| NGINX | Web Server |
| PHP + PHP-FPM | Backend Processing |
| MariaDB | Relational Database |
| php-mysqlnd | PHP to MariaDB Connector |
| HTML | Frontend Form |

---

## Project Architecture

```
User Browser
     ↓
NGINX Web Server
     ↓
PHP-FPM Backend
     ↓
MariaDB Database
```

> <img src="/Screenshots/Architecture Diagram.png" alt="📸 *Screenshot: EC2 instance running and SSH connection established" width="1000">

---

## Implementation Steps

---

### Step 1 — Launch EC2 Instance

Created an Amazon Linux EC2 instance on AWS.  
Configured the Security Group to allow inbound traffic on **Port 22 (SSH)** and **Port 80 (HTTP)**.  
Connected to the instance using SSH:

```bash
ssh -i "your-key.pem" ec2-user@your-ec2-public-ip
```

> <img src="/Screenshots/SSH connection established.PNG" alt="📸 *Screenshot: EC2 instance running and SSH connection established" width="1000">

---

### Step 2 — Install Required Packages

Installed NGINX, MariaDB, PHP, and PHP-FPM on the server:

```bash
sudo yum install nginx mariadb105-server php php-fpm -y
```

> <img src="/Screenshots/Packages installing successfully.PNG" alt="*Screenshot: Packages installing successfully*" width="1000">

---

### Step 3 — Start and Enable Services

Started the services and enabled them to auto-start on reboot:

```bash
sudo systemctl enable nginx mariadb php-fpm
sudo systemctl start nginx mariadb php-fpm
```

Verified all services are running:

```bash
sudo systemctl status nginx mariadb php-fpm
```

> <img src="/Screenshots/Packages installing successfully.PNG" alt="*Screenshot: All three services showing `active (running)` in green*" width="1000"> 


---

### Step 4 — Create Frontend (HTML Form)

Navigated to the NGINX web directory and created the signup form:

```bash
cd /usr/share/nginx/html/
sudo vim signup.html
```

Pasted the frontend HTML code for the **User Signup Form**.

**Form Fields:**
- Name
- Email
- Website
- Gender
- Comment
- Submit Button

> <img src="/Screenshots/Singup from.jpeg" alt="*Screenshot: signup.html file created with form code*" width="1000">

---

### Step 5 — Configure Database

Logged into MariaDB and secured the root account:

```bash
sudo mysql
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
```

Created the database and table:

```sql
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
```

### Step 6 — Configure Backend (PHP)

Created the PHP backend file in the NGINX web directory:

```bash
cd /usr/share/nginx/html/
sudo vim submit.php
```

The PHP script handles:
- Receiving form data submitted by the user
- Connecting to the MariaDB database
- Inserting the submitted data into the `users` table

> <img src="/Screenshots/Database Submitted.jpeg" alt="*Screenshot: submit.php file created with backend code*" width="1000">

---

### Step 7 — Install PHP MySQL Connector ⚠️ (Critical Step)

This is the **most important step**. Without this connector, PHP cannot communicate with MariaDB at all.

```bash
sudo yum install php-mysqlnd.x86_64 -y
```

Restarted all services to apply the changes:

```bash
sudo systemctl restart nginx mariadb php-fpm
```

### Step 8 — Access the Application

Opened the browser and navigated to the EC2 public IP:

```
http://your-ec2-public-ip/signup.html
```

The **User Signup Form** loaded successfully in the browser.

---

### Step 9 — Submit Form and Verify Data

Filled the form and submitted it.  
Then verified the data was stored in the database:

```bash
sudo mysql -u root -p
```

```sql
USE FCT;
SELECT * FROM users;
```

The submitted record appeared in the database table confirming end-to-end functionality.

---

## ✅ Project Output

- User Signup Form deployed successfully on AWS EC2
- Form accessible via EC2 Public IP in the browser
- Form data stored permanently in MariaDB database
- All services (NGINX, PHP-FPM, MariaDB) running successfully
- Full cloud deployment completed end-to-end

---

## 🎯 Learning Outcomes

Through this project I learned:

- How to launch and configure a Linux server on AWS EC2
- How NGINX works as a web server and how to configure it
- How PHP-FPM processes requests forwarded by NGINX
- How to create a database and table in MariaDB
- Why the `php-mysqlnd` connector is critical — PHP cannot talk to the database without it
- How to manage Linux services using `systemctl`
- How all the layers of a web application connect together as one system

---

## Author

**Aryanraje Dhokale**  
Cloud and DevOps Learner

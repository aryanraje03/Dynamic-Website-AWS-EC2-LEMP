# Dynamic Website Deployment on AWS EC2 using LEMP Stack

## Project Overview

I built this project to understand how a real dynamic website works end to end on the cloud. This is not just a static HTML page — it is a fully functional web application where a user fills a signup form, the data gets processed by PHP in the backend, and gets stored permanently in a MariaDB database. Everything runs live on an AWS EC2 instance.

This was one of the most hands-on and practical projects I have done so far in my cloud learning journey. Every step taught me something new — from setting up a Linux server on AWS to understanding how a web server, backend language, and database work together in a real environment.

---

## Architecture

```
User (Browser)  -->  Nginx (Web Server)  -->  PHP + FPM (Backend)  -->  MariaDB (Database)
```

The user opens the signup form in a browser. Nginx receives the request and serves the HTML page. When the user submits the form, Nginx forwards the request to PHP-FPM which processes the form data and inserts it into the MariaDB database. The database stores all the submitted records permanently.

I have also created a detailed architecture diagram using draw.io. You can view it in the `/diagram` folder of this repository.

---

## Tech Stack

| Technology | Purpose |
|---|---|
| AWS EC2 (Amazon Linux) | Cloud server to host the entire application |
| Nginx | Web server to handle HTTP requests and serve files |
| PHP + FPM | Backend scripting to process form data |
| MariaDB | Relational database to store user submissions |
| php-mysqlnd | Connector that allows PHP to communicate with MariaDB |

---

## What I Learned

Before this project, I only knew the theory of how websites work. After doing this project I now understand:

- How to launch and configure a Linux server on AWS EC2
- How Nginx works as a web server and how to configure it
- How PHP-FPM processes requests from Nginx
- How to create a database and table in MariaDB
- Why the php-mysqlnd connector is critical — without it PHP cannot talk to the database at all
- How to manage Linux services using systemctl
- How all the layers of a web application connect together

---

## Installation Steps

### Step 1 — Launch EC2 Instance

Launch an Amazon Linux instance on AWS EC2. Configure the security group to allow inbound traffic on port 80 (HTTP) and port 22 (SSH). Connect to the instance using SSH.

```bash
ssh -i your-key.pem ec2-user@your-ec2-public-ip
```

### Step 2 — Install LEMP Stack

Install Nginx, MariaDB, PHP, and PHP-FPM on the server.

```bash
sudo yum install nginx mariadb105-server php php-fpm -y
```

### Step 3 — Enable and Start All Services

Enable the services so they start automatically on reboot, then start them.

```bash
sudo systemctl enable nginx mariadb php-fpm
sudo systemctl start nginx mariadb php-fpm
```

### Step 4 — Verify Services Are Running

Check that all three services are active and running correctly.

```bash
sudo systemctl status nginx mariadb php-fpm
```

Both Nginx and MariaDB should show `active (running)` in green.

### Step 5 — Deploy the Website Files

Navigate to the Nginx HTML directory and place your project files there.

```bash
cd /var/www/share/nginx/html
```

Place `signup.html` and `submit.php` in this folder. These are the frontend form and the backend PHP handler respectively.

### Step 6 — Setup the MariaDB Database

Login to MariaDB and create the database and table.

```bash
sudo mysql -u root -p
```

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

### Step 7 — Install the PHP-MySQL Connector (Most Important Step)

This is the most critical step. Without this connector, PHP cannot communicate with MariaDB at all. I learned this the hard way.

```bash
sudo yum install php-mysqlnd.x86_64 -y
```

### Step 8 — Restart All Services

After all configuration is done, restart all services to apply the changes.

```bash
sudo systemctl restart nginx mariadb php-fpm
```

### Step 9 — Test the Application

Open your browser and go to:

```
http://your-ec2-public-ip/signup.html
```

Fill the form and submit it. Then verify the data was stored in the database:

```bash
sudo mysql -u root -p
USE FCT;
SELECT * FROM users;
```

You should see the submitted record in the table.

---

## Project Structure

```
Dynamic-Website-AWS-EC2-LEMP/
|
|-- frontend/
|   |-- signup.html         # User signup form
|
|-- backend/
|   |-- submit.php          # PHP script to handle form and insert data to DB
|
|-- diagram/
|   |-- architecture.drawio # draw.io architecture diagram
|   |-- architecture.png    # Exported diagram image
|
|-- screenshots/
|   |-- signup_form.png        # Live signup form on browser
|   |-- form_submitted.png     # Success page after form submission
|   |-- database_record.png    # MariaDB showing stored data
|   |-- server_status.png      # Nginx and MariaDB active status
|
|-- README.md
```

---

## Screenshots

### Signup Form
The HTML form running live on the EC2 server accessed via the public IP.

### Form Submitted Successfully
After filling and submitting the form, the PHP script processes the data and shows a success message with the submitted details.

### Data Stored in MariaDB
After submission, the record is visible in the MariaDB database using `SELECT * FROM users` query.

### Server Status
Both Nginx and MariaDB services showing `active (running)` status confirming the deployment is working.

---

## Summary

This project gave me real hands-on experience with cloud infrastructure and full stack web deployment. I started from scratch — launching a server on AWS, installing and configuring all the required services, writing the backend PHP code, setting up the database, and finally testing the complete flow end to end.

The most important thing I understood from this project is that deploying a website is not just about writing HTML or PHP. It is about understanding how all the components — the cloud server, web server, backend language, database, and their connectors — work together as one system.

I am happy that I was able to complete this project successfully and I am looking forward to building more complex projects on top of this foundation.

---

## Author

**Aryanraje Dhokale**
Cloud and DevOps Learner

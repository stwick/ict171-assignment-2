# Assignment 2: Cloud Project & Video Explainer

**Student ID:** 34863009  
**Student Name:** Stanley Musarurwa

**Domain:** http://bmbsta.xyz  
**Server IP:** 52.62.54.48  
**Video Explainer:** https://drive.google.com/file/d/1hFoh-GbLbSpZsirmx6Fojt8VK1_urWJD/view?usp=sharing

## Project Description
This project demonstrates the deployment of a WordPress website on Amazon EC2 using Infrastructure as a Service (IaaS). The server runs Ubuntu with a LAMP stack (Linux, Apache, MySQL, PHP) and is accessible via a custom domain name.

## Server Specifications
- **Cloud Provider:** Amazon Web Services (AWS) EC2
- **Instance Type:** t2.micro (Free Tier)
- **Operating System:** Ubuntu 24.04 LTS
- **Public IP Address:** 52.62.54.48
- **Domain:** bmbsta.xyz (registered w/ Namecheap)

## WordPress Installation Steps
*Connect to EC2 instance via AWS Console -> EC2 Instance Connect*

### 1. Update System
```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Install Apache Web Server
```bash
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
```

### 3. Install MySQL Database
```bash
sudo apt install mysql-server -y
sudo mysql_secure_installation
```

### 4. Install PHP
```bash
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip -y
```

### 5. Configure MySQL Database
```bash
sudo mysql -u root -p
```

```sql
CREATE DATABASE wordpress_db;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'wp_password123';
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 6. Download and Install WordPress
```bash
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz
sudo cp -R /tmp/wordpress/* /var/www/html/
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
```

### 7. Configure WordPress
```bash
sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
sudo nano /var/www/html/wp-config.php
```

Edit the database settings:
```php
define( 'DB_NAME', 'wordpress_db' );
define( 'DB_USER', 'wp_user' );
define( 'DB_PASSWORD', 'wp_password123' );
define( 'DB_HOST', 'localhost' );
```

### 8. Domain Configuration
- Purchased domain bmbsta.xyz from Namecheap
- Added A Record pointing to server IP: 52.62.54.48

## Scripting Components

### Server Monitoring Script

**Create the script directory and file**:
```bash
mkdir -p /home/ubuntu/scripts
cd /home/ubuntu/scripts
nano server-monitor.sh
```

**Script:**
```bash
#!/bin/bash

echo "=== ICT171 Server Health Report ==="
echo "Generated on: $(date)"
echo "Domain: bmbsta.xyz"
echo "IP Address: $(hostname -I)"
echo ""

echo "=== System Information ==="
echo "Uptime: $(uptime)"
echo "Disk Usage:"
df -h /
echo ""

echo "=== Web Server Status ==="
sudo systemctl is-active apache2
echo "Apache processes: $(pgrep apache2 | wc -l)"
echo ""

echo "=== Database Status ==="
sudo systemctl is-active mysql
echo ""

echo "=== WordPress Status ==="
ls -la /var/www/html/wp-config.php > /dev/null && echo "WordPress: Installed" || echo "WordPress: Not found"
echo ""

echo "Report completed."
```

**Make executable and run:**
```bash
chmod +x server-monitor.sh
./server-monitor.sh
```

### Administrative Information Script

**Create the PHP script:**
```bash
sudo nano /var/www/html/admin-info.php
```

**Script:**
```php
<?php
echo "<h1>ICT171 Server Information</h1>";
echo "<p><strong>Student ID:</strong> 34863009</p>";
echo "<p><strong>Server Time:</strong> " . date('Y-m-d H:i:s') . "</p>";
echo "<p><strong>Domain:</strong> bmbsta.xyz</p>";
echo "<p><strong>PHP Version:</strong> " . phpversion() . "</p>";
echo "<p><strong>Assignment:</strong> ICT171 Cloud Project</p>";
?>
```

## Testing Commands

```bash
sudo systemctl status apache2
sudo systemctl status mysql
curl http://bmbsta.xyz
```

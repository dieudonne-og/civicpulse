# Installation Guide

This document provides detailed instructions for setting up the Citizen Complaints and Engagement System in both development and production environments.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Local Development Setup](#local-development-setup)
- [Production Setup](#production-setup)
- [Database Configuration](#database-configuration)
- [Application Configuration](#application-configuration)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)

## Prerequisites

### Required Software

- Web Server (Apache recommended)
- PHP 8.0 or higher
- MySQL 8.0 or higher
- Git (for version control)

### PHP Extensions

The following PHP extensions must be enabled:

- mysqli
- pdo_mysql
- json
- fileinfo
- gd
- mbstring
- session
- xml

### Recommended Tools

- **XAMPP** (Windows/macOS/Linux) - For local development
- **phpMyAdmin** - For database management
- **Composer** - For dependency management (optional, for future extensions)

## Local Development Setup

### 1. Install XAMPP

1. Download XAMPP from [https://www.apachefriends.org/](https://www.apachefriends.org/)
2. Follow the installation instructions for your operating system
3. Start the Apache and MySQL services from the XAMPP Control Panel

### 2. Clone the Repository

```bash
# Navigate to the htdocs directory
cd /path/to/xampp/htdocs

# Clone the repository
git clone https://github.com/your-username/citizen-complaints-system.git
```

### 3. Create and Configure the Database

1. Open phpMyAdmin (usually at http://localhost/phpmyadmin)
2. Create a new database named `citizen_complaints_db`
3. Set the character set to `utf8mb4` with collation `utf8mb4_unicode_ci`
4. Import the database schema:
   - Find the SQL file in the `database` directory
   - Click on the "Import" tab in phpMyAdmin
   - Select the SQL file and click "Go"

### 4. Configure Database Connection

1. Open the project directory
2. Locate the `config` directory and copy `config.sample.php` to `config.php`
3. Edit `config.php` with your database credentials:

```php
<?php
// Database Configuration
define('DB_HOST', 'localhost');
define('DB_NAME', 'citizen_complaints_db');
define('DB_USER', 'root'); // Change in production
define('DB_PASS', ''); // Change in production

// Site Configuration
define('SITE_URL', 'http://localhost/citizen-complaints-system');
define('UPLOAD_DIR', __DIR__ . '/../uploads/');

// Security Configuration
define('SECURE_COOKIE', false); // Set to true in production
define('SESSION_LIFETIME', 1800); // 30 minutes
?>
```

### 5. Set File Permissions

Ensure that the web server has write access to the following directories:

```bash
# On Linux/macOS
chmod 755 -R /path/to/xampp/htdocs/citizen-complaints-system
chmod 777 -R /path/to/xampp/htdocs/citizen-complaints-system/uploads
chmod 777 -R /path/to/xampp/htdocs/citizen-complaints-system/logs
```

On Windows with XAMPP, the default permissions should work fine.

### 6. Access the Application

Open your web browser and navigate to:
```
http://localhost/citizen-complaints-system
```

You should see the home page of the application.

### 7. Create Admin Account

To create the initial admin account:

1. Navigate to http://localhost/citizen-complaints-system/setup.php
2. Follow the on-screen instructions to create an admin account
3. Delete or rename the setup.php file after creating the admin account for security

## Production Setup

### 1. Server Requirements

- A web server with PHP 8.0+ support (Apache/Nginx)
- MySQL 8.0+ or MariaDB 10.5+
- SSL certificate for HTTPS (recommended)
- SSH access to the server

### 2. Domain and Hosting Setup

1. Register a domain name if you don't have one
2. Set up web hosting with PHP and MySQL support
3. Configure your domain's DNS to point to your hosting

### 3. Deploy the Application

#### Option 1: Manual Deployment

1. Clone or upload the application files to your server:
   ```bash
   # Clone directly to server
   cd /var/www/html
   git clone https://github.com/your-username/citizen-complaints-system.git
   
   # OR upload via FTP/SFTP
   ```

2. Set proper file permissions:
   ```bash
   chown -R www-data:www-data /var/www/html/citizen-complaints-system
   chmod 755 -R /var/www/html/citizen-complaints-system
   chmod 777 -R /var/www/html/citizen-complaints-system/uploads
   chmod 777 -R /var/www/html/citizen-complaints-system/logs
   ```

#### Option 2: Automated Deployment (Advanced)

Consider setting up automated deployment using Git hooks or CI/CD pipelines.

### 4. Database Setup

1. Create a production database with a secure password:
   ```sql
   CREATE DATABASE citizen_complaints_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   CREATE USER 'complaints_user'@'localhost' IDENTIFIED BY 'your_secure_password';
   GRANT ALL PRIVILEGES ON citizen_complaints_db.* TO 'complaints_user'@'localhost';
   FLUSH PRIVILEGES;
   ```

2. Import the database schema:
   ```bash
   mysql -u complaints_user -p citizen_complaints_db < /path/to/database/schema.sql
   ```

### 5. Configure the Application

1. Create and edit the configuration file:
   ```bash
   cp /var/www/html/citizen-complaints-system/config/config.sample.php /var/www/html/citizen-complaints-system/config/config.php
   nano /var/www/html/citizen-complaints-system/config/config.php
   ```

2. Update with production values:
   ```php
   <?php
   // Database Configuration
   define('DB_HOST', 'localhost');
   define('DB_NAME', 'citizen_complaints_db');
   define('DB_USER', 'complaints_user');
   define('DB_PASS', 'your_secure_password');

   // Site Configuration
   define('SITE_URL', 'https://your-domain.com');
   define('UPLOAD_DIR', __DIR__ . '/../uploads/');

   // Security Configuration
   define('SECURE_COOKIE', true);
   define('SESSION_LIFETIME', 1800); // 30 minutes
   ?>
   ```

### 6. Web Server Configuration

#### Apache Configuration

Create or edit the virtual host configuration:

```apache
<VirtualHost *:80>
    ServerName your-domain.com
    ServerAlias www.your-domain.com
    DocumentRoot /var/www/html/citizen-complaints-system
    
    <Directory /var/www/html/citizen-complaints-system>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    
    # Redirect to HTTPS
    RewriteEngine on
    RewriteCond %{SERVER_NAME} =your-domain.com [OR]
    RewriteCond %{SERVER_NAME} =www.your-domain.com
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

#### Nginx Configuration

```nginx
server {
    listen 80;
    server_name your-domain.com www.your-domain.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name your-domain.com www.your-domain.com;
    
    ssl_certificate /path/to/certificate.crt;
    ssl_certificate_key /path/to/private.key;
    
    root /var/www/html/citizen-complaints-system;
    index index.php index.html;
    
    location / {
        try_files $uri $uri/ /index.php?$args;
    }
    
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }
    
    location ~ /\.ht {
        deny all;
    }
}
```

### 7. SSL Configuration

1. Install Let's Encrypt for free SSL certificates:
   ```bash
   apt install certbot python3-certbot-apache # For Apache
   # OR
   apt install certbot python3-certbot-nginx # For Nginx
   ```

2. Generate certificates:
   ```bash
   certbot --apache -d your-domain.com -d www.your-domain.com
   # OR
   certbot --nginx -d your-domain.com -d www.your-domain.com
   ```

### 8. Setup Admin Account

Follow the same procedure as in the local setup to create an admin account:

1. Navigate to https://your-domain.com/setup.php
2. Create the admin account
3. Delete setup.php

## Database Configuration

For detailed database schema information, please refer to [DATABASE.md](./DATABASE.md).

### Creating the Database Schema

If you need to manually create the database schema:

```sql
-- Create the database
CREATE DATABASE IF NOT EXISTS citizen_complaints_db
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;

USE citizen_complaints_db;

-- Create users table
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    address TEXT,
    user_type ENUM('citizen', 'admin') NOT NULL DEFAULT 'citizen',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP
);

-- Additional tables would be created here...
```

### Database Backup and Restore

#### Backup

```bash
# Backup the database
mysqldump -u [username] -p citizen_complaints_db > backup.sql
```

#### Restore

```bash
# Restore the database
mysql -u [username] -p citizen_complaints_db < backup.sql
```

## Application Configuration

### Core Configuration Options

All configuration options in `config.php`:

| Option | Description | Default | Production Value |
|--------|-------------|---------|------------------|
| DB_HOST | Database host | localhost | localhost |
| DB_NAME | Database name | citizen_complaints_db | citizen_complaints_db |
| DB_USER | Database username | root | [secure username] |
| DB_PASS | Database password | [empty] | [secure password] |
| SITE_URL | Site base URL | http://localhost/citizen-complaints-system | https://your-domain.com |
| UPLOAD_DIR | Path to uploads directory | [project_path]/uploads/ | [project_path]/uploads/ |
| SECURE_COOKIE | Whether to use secure cookies | false | true |
| SESSION_LIFETIME | Session lifetime in seconds | 1800 | 1800 |

### Email Configuration

To enable email functionality (for notifications, password resets, etc.), add these to your config.php:

```php
// Email Configuration
define('MAIL_HOST', 'smtp.example.com');
define('MAIL_PORT', 587);
define('MAIL_USERNAME', 'your-email@example.com');
define('MAIL_PASSWORD', 'your-email-password');
define('MAIL_ENCRYPTION', 'tls');
define('MAIL_FROM_ADDRESS', 'noreply@your-domain.com');
define('MAIL_FROM_NAME', 'Citizen Complaints System');
```

## Common Issues and Troubleshooting

### Database Connection Issues

**Problem**: Unable to connect to the database  
**Solution**:
1. Verify database credentials in config.php
2. Ensure MySQL service is running
3. Check if the database user has proper permissions

### File Permission Issues

**Problem**: Unable to upload files or write to log files  
**Solution**:
1. Check if the uploads and logs directories exist
2. Ensure proper write permissions:
   ```bash
   chmod 777 -R uploads/
   chmod 777 -R logs/
   ```

### .htaccess Not Working

**Problem**: URL rewriting not working  
**Solution**:
1. Ensure mod_rewrite is enabled:
   ```bash
   a2enmod rewrite
   service apache2 restart
   ```
2. Check that AllowOverride is set to All in your Apache configuration

### White Screen of Death

**Problem**: Blank page with no error  
**Solution**:
1. Enable error reporting in PHP:
   ```php
   ini_set('display_errors', 1);
   ini_set('display_startup_errors', 1);
   error_reporting(E_ALL);
   ```
2. Check PHP and web server error logs

### Session Issues

**Problem**: Users get logged out unexpectedly  
**Solution**:
1. Check session configuration in php.ini
2. Verify SESSION_LIFETIME in config.php
3. Ensure session directory is writable

---

For additional assistance, please create an issue in the project repository or contact the project maintainer.

Last updated: May 16, 2025

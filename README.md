# NextCloud
Nextcloud is an open-source cloud platform for storing, sharing, and managing files on your own server. It offers a secure, self-hosted alternative to services like Google Drive, giving you full control over your data. With customizable apps, it supports collaboration, making it ideal for both personal and business use.


<!--
SPDX-FileCopyrightText: 2021-2024 Nextcloud GmbH and Nextcloud contributors
SPDX-License-Identifier: MIT
-->

# Nextcloud
**A safe home for all your data.**

![](https://raw.githubusercontent.com/nextcloud/screenshots/master/nextcloud-hub-files-25-preview.png)

## Why is this so awesome? 🤩

* 📁 **Access your Data** You can store your files, contacts, calendars, conversations and more on a server of your choosing.
* 🔄 **Sync your Data** You keep your files, contacts, calendars, and more synchronized amongst your devices.
* 🙌 **Share your Data** …by giving others access to the stuff you want them to see or to collaborate with.
* 🚀 **Expandable with hundreds of Apps** ...like [Calendar](https://github.com/nextcloud/calendar), [Contacts](https://github.com/nextcloud/contacts), [Mail](https://github.com/nextcloud/mail), [Video Chat](https://github.com/nextcloud/spreed) and all those you can discover in our [App Store](https://apps.nextcloud.com)
* 🔒 **Security** with our encryption mechanisms, [HackerOne bounty program](https://hackerone.com/nextcloud) and two-factor authentication.

Would you like to learn more about using Nextcloud to access, share and protect your files, calendars, contacts, communication & more at home and in your organization? [**Learn about all our Features**](https://nextcloud.com).

## Get your Nextcloud 🚚

- ☑️ [**Simply sign up**](https://nextcloud.com/signup/) at one of our providers through our website or the apps directly.
- 🖥 [**Install** a server by yourself](https://nextcloud.com/install/#instructions-server) on your hardware or by using one of our ready-to-use **appliances**
- 📦 Buy one of the [awesome **devices** coming with a preinstalled Nextcloud](https://nextcloud.com/devices/)
- 🏢 Find a [service **provider**](https://nextcloud.com/partners/) who hosts Nextcloud for you or your company

Enterprise? Public Sector or Education user? You may want to have a look into [**Nextcloud Enterprise**](https://nextcloud.com/enterprise/) provided by Nextcloud GmbH.

##Insalltaion##
Here's a step-by-step guide to install Nextcloud using Nginx, MySQL, and an SSL certificate on Ubuntu 22.04:

### Step 1: Update and Upgrade Your System
Start by updating your package list and upgrading the installed packages:

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Required Packages
Nextcloud requires several packages, including Nginx, MySQL, PHP, and other dependencies:

```bash
sudo apt install nginx mysql-server php-fpm php-mysql php-curl php-gd php-intl php-json php-mbstring php-xml php-zip php-bz2 php-imagick php-gmp php-bcmath php-ldap unzip -y
```

### Step 3: Configure MySQL
Secure your MySQL installation and create a database for Nextcloud:

```bash
sudo mysql_secure_installation
```

Login to MySQL:

```bash
sudo mysql -u root -p
```

Create the database and user:

```sql
CREATE DATABASE nextcloud;
CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextclouduser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Step 4: Download Nextcloud
Download the latest version of Nextcloud from their official website:

```bash
cd /var/www/
sudo wget https://download.nextcloud.com/server/releases/latest.zip
sudo unzip latest.zip
sudo mv nextcloud /var/www/nextcloud
sudo chown -R www-data:www-data /var/www/nextcloud
sudo chmod -R 755 /var/www/nextcloud
```

### Step 5: Configure PHP
Edit the PHP configuration to optimize it for Nextcloud:

```bash
sudo nano /etc/php/8.1/fpm/php.ini
```

Make sure to set the following values:

```ini
memory_limit = 512M
upload_max_filesize = 100M
post_max_size = 100M
max_execution_time = 360
```

Restart PHP-FPM:

```bash
sudo systemctl restart php8.1-fpm
```

### Step 6: Configure Nginx
Create a new Nginx configuration file for Nextcloud:

```bash
sudo nano /etc/nginx/sites-available/nextcloud
```

Add the following content:

```nginx
server {
    listen 80;
    server_name your_domain_or_IP;

    root /var/www/nextcloud;
    index index.php index.html /index.php$request_uri;

    location / {
        try_files $uri $uri/ /index.php$request_uri;
    }

    location ~ ^/\.well-known/acme-challenge/ {
        root /var/www/nextcloud;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }

    client_max_body_size 100M;
}

```

Enable the configuration and restart Nginx:

```bash
sudo ln -s /etc/nginx/sites-available/nextcloud /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

### Step 7: Obtain and Install an SSL Certificate
Use Certbot to obtain and install an SSL certificate:

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d your_domain_or_IP
```

Follow the prompts to complete the SSL installation. Certbot will automatically update your Nginx configuration to redirect HTTP to HTTPS.

### Step 8: Complete the Nextcloud Installation
Open your web browser and navigate to `https://your_domain_or_IP`. You should see the Nextcloud setup page.

Enter the database details you created earlier and create an admin account. Complete the setup, and your Nextcloud installation will be ready to use.

### Optional: Set Up Cron Jobs for Nextcloud
It's recommended to set up a cron job to run background tasks:

```bash
sudo -u www-data crontab -e
```

Add the following line:

```bash
*/15  *  *  *  * php -f /var/www/nextcloud/cron.php
```

### Step 9: Secure MySQL (Optional)
Consider configuring your MySQL server for better security:

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Find the `bind-address` directive and set it to `127.0.0.1` to prevent remote access:

```ini
bind-address = 127.0.0.1
```

Restart MySQL:

```bash
sudo systemctl restart mysql
```

That's it! Your Nextcloud instance should now be up and running with Nginx, MySQL, and SSL on Ubuntu 22.04.

## Get in touch 💬

* [📋 Forum](https://help.nextcloud.com)
* [👥 Facebook](https://www.facebook.com/nextclouders)
* [🐣 Twitter](https://twitter.com/Nextclouders)
* [🐘 Mastodon](https://mastodon.xyz/@nextcloud)

You can also [get support for Nextcloud](https://nextcloud.com/support)!


## Join the team 👪

There are many ways to contribute, of which development is only one! Find out [how to get involved](https://nextcloud.com/contribute/), including as a [translator](https://help.nextcloud.com/t/translation-knowledge-valid-for-the-entire-nextcloud-project-wiki/51550), designer, tester, helping others, and much more! 😍

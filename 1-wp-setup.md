Here’s a step-by-step guide to setting up **WordPress** with **MySQL**, **phpMyAdmin**, **HTTPS/SSL**, and deploying it on an Azure **B1ms** instance for `wp.goprimetechnologies.com`. This includes optimal configurations for performance and security.

---

### **1. Set Up the Azure B1ms Instance**
1. **Log in to Azure Portal:**
   - Go to the [Azure Portal](https://portal.azure.com/).
   - Create a new **Virtual Machine** using the **B1ms size**.
   - Choose **Ubuntu Server 22.04 LTS** (recommended).

2. **Configure Networking:**
   - Open **HTTP (80)** and **HTTPS (443)** ports in the **Network Security Group (NSG)**.
   - Open port **22** for SSH access.

3. **Connect to the VM via SSH:**
   ```bash
   ssh azureuser@<public-ip-address>
   ```

---

### **2. Install Apache, MySQL, and PHP**
1. **Update the server:**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install Apache:**
   ```bash
   sudo apt install apache2 -y
   sudo systemctl enable apache2
   sudo systemctl start apache2
   ```

3. **Install MySQL:**
   ```bash
   sudo apt install mysql-server -y
   sudo mysql_secure_installation
   ```
   - Set a strong root password.
   - Remove anonymous users and test database.

4. **Install PHP and required extensions:**
   ```bash
   sudo apt install php libapache2-mod-php php-mysql php-curl php-xml php-mbstring -y
   ```

5. **Restart Apache:**
   ```bash
   sudo systemctl restart apache2
   ```

---

### **3. Set Up MySQL for WordPress**
1. **Log in to MySQL:**
   ```bash
   sudo mysql -u root -p
   ```

2. **Create a WordPress database and user:**
   ```sql
   CREATE DATABASE wordpress;
   CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'strongpassword';
   GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

---

### **4. Install phpMyAdmin (Optional)**
1. **Install phpMyAdmin:**
   ```bash
   sudo apt install phpmyadmin -y
   ```

2. **Configure phpMyAdmin:**
   - Select **Apache2** during installation.
   - Enable MySQL with the root password.

3. **Enable phpMyAdmin Access:**
   ```bash
   sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
   ```

4. **Secure phpMyAdmin:**
   ```bash
   sudo nano /etc/apache2/conf-available/phpmyadmin.conf
   ```
   Add:
   ```apache
   <Directory /usr/share/phpmyadmin>
       Options FollowSymLinks
       DirectoryIndex index.php
       AllowOverride All
       Require all granted
   </Directory>
   ```
   - Enable `.htaccess`:
     ```bash
     sudo a2enmod rewrite
     sudo systemctl restart apache2
     ```

---

### **5. Install WordPress**
1. **Download WordPress:**
   ```bash
   wget https://wordpress.org/latest.tar.gz
   tar -xvzf latest.tar.gz
   sudo mv wordpress /var/www/html/wp.goprimetechnologies.com
   ```

2. **Set Permissions:**
   ```bash
   sudo chown -R www-data:www-data /var/www/html/wp.goprimetechnologies.com
   sudo chmod -R 755 /var/www/html/wp.goprimetechnologies.com
   ```

3. **Configure Apache for WordPress:**
   ```bash
   sudo nano /etc/apache2/sites-available/wp.goprimetechnologies.com.conf
   ```
   Add:
   ```apache
   <VirtualHost *:80>
       ServerName wp.goprimetechnologies.com
       DocumentRoot /var/www/html/wp.goprimetechnologies.com
       <Directory /var/www/html/wp.goprimetechnologies.com>
           AllowOverride All
       </Directory>
   </VirtualHost>
   ```
   Enable the site:
   ```bash
   sudo a2ensite wp.goprimetechnologies.com.conf
   sudo a2enmod rewrite
   sudo systemctl restart apache2
   ```

4. **Access WordPress Setup:**
   - Visit `http://wp.goprimetechnologies.com` to complete the setup.

---

### **6. Secure with HTTPS/SSL**
1. **Install Certbot:**
   ```bash
   sudo apt install certbot python3-certbot-apache -y
   ```

2. **Obtain an SSL Certificate:**
   ```bash
   sudo certbot --apache -d wp.goprimetechnologies.com
   ```
   - Follow prompts to configure HTTPS.

3. **Auto-Renew SSL:**
   ```bash
   sudo crontab -e
   ```
   Add:
   ```bash
   0 0,12 * * * certbot renew --quiet
   ```

---

### **7. Optimize for B1ms**
1. **Increase PHP Memory Limit:**
   ```bash
   sudo nano /etc/php/8.1/apache2/php.ini
   ```
   Update:
   ```ini
   memory_limit = 256M
   ```

2. **Enable Caching:**
   - Install and configure a WordPress caching plugin like **W3 Total Cache**.

3. **Optimize Database:**
   - Use plugins like **WP-Optimize** to clean and optimize the database.

4. **Configure Swap Space:**
   ```bash
   sudo fallocate -l 1G /swapfile
   sudo chmod 600 /swapfile
   sudo mkswap /swapfile
   sudo swapon /swapfile
   echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
   ```

---

### **8. Final Steps**
1. **Update DNS Records:**
   - Point `wp.goprimetechnologies.com` to the public IP of your VM.

2. **Monitor Performance:**
   - Use **Azure Monitor** or install monitoring tools like **Netdata** on the server.

3. **Regular Backups:**
   - Use plugins like **UpdraftPlus** for regular backups of the WordPress site.

You should now have a fully functional, secure WordPress site running on Azure with HTTPS and phpMyAdmin access! Let me know if you need help with any specific step.
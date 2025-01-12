It looks like WordPress is unable to connect to the database because the `wp-config.php` file is either missing or misconfigured. Here's how to resolve this issue:

---

### **1. Verify the Presence of wp-config.php**
1. **Check for `wp-config.php` in the WordPress root directory:**
   ```bash
   ls -l /var/www/html/wp.goprimetechnologies.com
   ```
2. If `wp-config.php` is missing, WordPress may not have generated it yet. Proceed to manually create and configure it.

---

### **2. Create wp-config.php**
1. **Copy the sample configuration file:**
   ```bash
   sudo cp /var/www/html/wp.goprimetechnologies.com/wp-config-sample.php /var/www/html/wp.goprimetechnologies.com/wp-config.php
   ```

2. **Edit the wp-config.php file:**
   ```bash
   sudo nano /var/www/html/wp.goprimetechnologies.com/wp-config.php
   ```

3. **Update the database connection details:**
   Replace the placeholder values with your actual database information:
   ```php
   // ** MySQL settings ** //
   define( 'DB_NAME', 'wordpress' ); // Database name
   define( 'DB_USER', 'wpuser' );   // Database username
   define( 'DB_PASSWORD', 'yourpassword' ); // Database password
   define( 'DB_HOST', 'localhost' ); // Use 'localhost' since the database is on the same VM
   define( 'DB_CHARSET', 'utf8mb4' );
   define( 'DB_COLLATE', '' );
   ```

4. **Save and exit:**
   - Press `CTRL+O` to save and `CTRL+X` to exit.

---

### **3. Verify Database Connectivity**
1. **Test MySQL login:**
   ```bash
   mysql -u wpuser -p
   ```
   - Enter the password you used when creating the `wpuser` database user.
   - If you can log in, the credentials are correct.

2. **Check WordPress permissions:**
   Ensure the WordPress directory has proper permissions:
   ```bash
   sudo chown -R www-data:www-data /var/www/html/wp.goprimetechnologies.com
   sudo chmod -R 755 /var/www/html/wp.goprimetechnologies.com
   ```

---

### **4. Restart Services**
1. Restart Apache and MySQL:
   ```bash
   sudo systemctl restart apache2
   sudo systemctl restart mysql
   ```

---

### **5. Troubleshoot Further if Necessary**
If you still get the "Error establishing a database connection," check these points:
1. **MySQL Service Status:**
   ```bash
   sudo systemctl status mysql
   ```
   Ensure it’s active and running.

2. **PHP Error Logs:**
   Check if there are errors related to database connection:
   ```bash
   sudo tail -n 20 /var/log/apache2/error.log
   ```

3. **MySQL Bind Address:**
   Ensure MySQL is listening on `127.0.0.1`:
   ```bash
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```
   Look for:
   ```ini
   bind-address = 127.0.0.1
   ```
   If this line is commented or incorrect, fix it and restart MySQL:
   ```bash
   sudo systemctl restart mysql
   ```

---

### **6. Access WordPress Setup**
Visit `http://wp.goprimetechnologies.com` to verify the connection and complete the setup.

If you encounter further issues, let me know where it gets stuck!
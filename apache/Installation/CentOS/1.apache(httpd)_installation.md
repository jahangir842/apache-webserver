Here’s a detailed procedure for installing and configuring Apache2 (httpd) on CentOS without including virtual hosts:

### 1. **Update Package Index**
   First, update your package index to get the latest information about available packages:
   ```bash
   sudo yum update
   ```

### 2. **Install Apache (httpd)**
   Install the Apache package (`httpd`) using `yum`:
   ```bash
   sudo yum install httpd
   ```

   This command will download and install Apache along with its dependencies.

### 3. **Start and Enable Apache Service**
   After installation, start the Apache service and enable it to start on boot:
   ```bash
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```

### 4. **Verify Apache Installation**
   Check the status of the Apache service to ensure it’s running:
   ```bash
   sudo systemctl status httpd
   ```

   You should see output indicating that the service is active (running).

### 5. **Adjust Firewall Settings**
   If you are using `firewalld`, you need to allow HTTP and HTTPS traffic:
   ```bash
   sudo firewall-cmd --permanent --add-service=http
   sudo firewall-cmd --permanent --add-service=https
   sudo firewall-cmd --reload
   ```

   If you are using `iptables`, you can allow specific ports:
   ```bash
   sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
   sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
   sudo service iptables save
   ```

### 6. **Configure Apache**
   Apache’s main configuration file is located at `/etc/httpd/conf/httpd.conf`. You can make changes directly to this file or create additional configuration files in `/etc/httpd/conf.d/`.

   To modify the main configuration file:
   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   You might add custom configurations. For example:
   ```apache
   # Custom configuration example
   <Directory /var/www/html>
       AllowOverride All
   </Directory>

   # Example of setting a custom log level
   LogLevel info
   ```

   Save the file and restart Apache to apply the changes:
   ```bash
   sudo systemctl restart httpd
   ```

### 7. **Verify Apache is Working**
   Open a web browser and navigate to your server’s IP address or domain name. You should see the default Apache welcome page, indicating that Apache is working correctly.

   For example:
   ```
   http://your_server_ip
   ```

### 8. **Check Logs for Errors (Optional)**
   If you encounter issues, check Apache’s error log for troubleshooting:
   ```bash
   sudo tail -f /var/log/httpd/error_log
   ```

### Summary
You’ve installed Apache on CentOS, started and enabled the service, adjusted firewall settings, and configured Apache to meet your needs. If you encounter any issues, reviewing the logs will assist in troubleshooting.

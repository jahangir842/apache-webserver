Here’s a detailed procedure for installing and configuring Apache2 on Ubuntu without including virtual hosts:

### 1. **Update Package Index**
   Before installing new packages, update your package index to get the latest information:
   ```bash
   sudo apt update
   ```

### 2. **Install Apache2**
   Install the Apache2 package using `apt`:
   ```bash
   sudo apt install apache2
   ```

   This command will download and install Apache2 along with its dependencies.

### 3. **Start and Enable Apache2 Service**
   After the installation, start the Apache2 service and enable it to start on boot:
   ```bash
   sudo systemctl start apache2
   sudo systemctl enable apache2
   ```

### 4. **Verify Apache2 Installation**
   Check the status of the Apache2 service to ensure it's running:
   ```bash
   sudo systemctl status apache2
   ```

   You should see output indicating that the service is active (running).

### 5. **Adjust Firewall Settings**
   If you are using `ufw` (Uncomplicated Firewall), you need to allow HTTP and HTTPS traffic:
   ```bash
   sudo ufw allow 'Apache'
   sudo ufw enable
   ```

   Alternatively, you can allow specific ports for HTTP and HTTPS:
   ```bash
   sudo ufw allow 80/tcp
   sudo ufw allow 443/tcp
   ```

### 6. **Configure Apache2**
   Apache2 configuration files are located in `/etc/apache2/`. The main configuration file is `/etc/apache2/apache2.conf`. 

   To make changes, you might edit the configuration file or create a new configuration file in `/etc/apache2/conf-available/`.

   For example, to add a custom configuration:
   ```bash
   sudo nano /etc/apache2/conf-available/custom.conf
   ```

   Add your configuration settings. For instance:
   ```apache
   # Custom configuration example
   <Directory /var/www/html>
       AllowOverride All
   </Directory>

   # Example of setting a custom log level
   LogLevel info
   ```

   Save the file and enable it:
   ```bash
   sudo a2enconf custom.conf
   ```

   Reload Apache2 to apply the new configuration:
   ```bash
   sudo systemctl reload apache2
   ```

### 7. **Verify Apache2 is Working**
   Open a web browser and navigate to your server’s IP address or domain name. You should see the default Apache2 welcome page, which confirms that Apache2 is up and running.

   For example:
   ```
   http://your_server_ip
   ```

### 8. **Check Logs for Errors (Optional)**
   If you encounter any issues, check Apache2’s error log for troubleshooting:
   ```bash
   sudo tail -f /var/log/apache2/error.log
   ```

### Summary
You have installed Apache2, started and enabled the service, adjusted firewall settings, and configured Apache2 to suit your needs. If there are issues, reviewing the logs will help with troubleshooting.

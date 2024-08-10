To secure your website with HTTPS using a self-signed SSL certificate, follow these steps:

### **1. Create a Self-Signed SSL Certificate**

1. **Install OpenSSL (if not already installed)**

   OpenSSL is typically installed by default on CentOS. However, if it’s not, you can install it with:
   ```bash
   sudo yum install openssl
   ```

2. **Create a Directory for SSL Certificates**

   Store your SSL certificates in a secure directory:
   ```bash
   sudo mkdir /etc/httpd/ssl
   ```

3. **Generate a Private Key and Certificate**

   Use OpenSSL to create a private key and a self-signed certificate:
   ```bash
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/httpd/ssl/mywebsite.key -out /etc/httpd/ssl/mywebsite.crt
   ```

   - `req -x509`: Generate an X.509 certificate.
   - `-nodes`: Do not encrypt the private key.
   - `-days 365`: The certificate is valid for 365 days.
   - `-newkey rsa:2048`: Generate a new RSA key with a size of 2048 bits.
   - `-keyout`: Path to the private key file.
   - `-out`: Path to the certificate file.

   During this process, you’ll be prompted to enter information like country name, state, organization, and a common name (usually your domain name).

### **2. Configure Apache to Use the SSL Certificate**

1. **Edit the Apache Configuration**

   Create a new SSL configuration file for your website:
   ```bash
   sudo nano /etc/httpd/conf.d/mywebsite-ssl.conf
   ```

2. **Add SSL Configuration Settings**

   Add the following configuration to the file:
   ```apache
   <VirtualHost *:443>
       ServerAdmin webmaster@mywebsite.com
       DocumentRoot /var/www/html/mywebsite
       ServerName mywebsite.com
       ServerAlias www.mywebsite.com

       SSLEngine on
       SSLCertificateFile /etc/httpd/ssl/mywebsite.crt
       SSLCertificateKeyFile /etc/httpd/ssl/mywebsite.key

       <Directory /var/www/html/mywebsite>
           AllowOverride All
           Require all granted
       </Directory>

       ErrorLog /var/log/httpd/mywebsite_ssl_error.log
       CustomLog /var/log/httpd/mywebsite_ssl_access.log combined
   </VirtualHost>
   ```

   - `SSLEngine on`: Enables SSL for this virtual host.
   - `SSLCertificateFile`: Path to your SSL certificate.
   - `SSLCertificateKeyFile`: Path to your private key.

3. **Save and Exit**

   Save the file and exit the text editor.

### **3. Enable SSL Module and Restart Apache**

1. **Enable SSL Module**

   If the SSL module is not enabled, enable it by running:
   ```bash
   sudo yum install mod_ssl
   ```

2. **Restart Apache**

   Restart Apache to apply the SSL configuration:
   ```bash
   sudo systemctl restart httpd
   ```

### **4. Adjust Firewall Settings**

If your firewall is enabled, you need to allow HTTPS traffic:

```bash
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

### **5. Verify the SSL Setup**

1. **Open Your Web Browser**

   Go to your browser and navigate to your domain name using `https`:
   ```
   https://your_server_ip
   ```

2. **Check the SSL Certificate**

   Your browser should indicate that the connection is secure. However, since it’s a self-signed certificate, you may get a warning about the certificate not being trusted. You can safely ignore this warning for testing purposes.

### **6. Redirect HTTP to HTTPS (Optional)**

To ensure that all traffic is secured, you can redirect HTTP traffic to HTTPS:

1. **Edit the Apache Configuration**

   Open your main Apache configuration file or the default configuration file:
   ```bash
   sudo nano /etc/httpd/conf.d/mywebsite.conf
   ```

2. **Add Redirect Rule**

   Add the following lines:
   ```apache
   <VirtualHost *:80>
       ServerName mywebsite.com
       Redirect permanent / https://mywebsite.com/
   </VirtualHost>
   ```

3. **Save and Exit**

   Save the changes and exit the text editor.

4. **Restart Apache**

   Restart Apache to apply the changes:
   ```bash
   sudo systemctl restart httpd
   ```

### **7. Test the Redirection**

Navigate to `http://your_server_ip` in your web browser. You should be automatically redirected to the HTTPS version of your site.

### **Summary**
You have successfully created a self-signed SSL certificate, configured Apache to use it, and optionally set up HTTP to HTTPS redirection. Your website is now accessible over HTTPS, providing an encrypted connection to your visitors.

If you have any questions or need further assistance, feel free to ask!

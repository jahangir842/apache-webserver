### How to Install Nginx Web Server on Ubuntu Linux Server

Nginx is a powerful, high-performance web server used widely for serving static content, load balancing, reverse proxying, and more. This guide will walk you through the steps to install and configure Nginx on an Ubuntu Linux server.

#### Step 1: **Update the Package Index**
Before installing any software, it's a good practice to update the package index on your system to ensure you have the latest information on available packages.

1. Open a terminal and run the following command:

   ```bash
   sudo apt update
   ```

   This command updates the package index, ensuring that the package manager is aware of the latest versions of the packages and their dependencies.

#### Step 2: **Install Nginx**

2. After updating the package index, install Nginx by running:

   ```bash
   sudo apt install nginx
   ```

   This command installs Nginx along with any necessary dependencies.

3. During installation, the `systemctl` command will automatically start Nginx, and it will be configured to start automatically at boot.

#### Step 3: **Verify Nginx Installation**

4. Once the installation is complete, verify that Nginx is installed correctly by checking its version:

   ```bash
   nginx -v
   ```

   The output should display the version of Nginx installed, confirming a successful installation.

#### Step 4: **Manage Nginx Service**

5. **Start Nginx**: Although Nginx starts automatically after installation, you can manually start it if needed:

   ```bash
   sudo systemctl start nginx
   ```

6. **Stop Nginx**: To stop Nginx, use:

   ```bash
   sudo systemctl stop nginx
   ```

7. **Restart Nginx**: To restart Nginx after making changes to its configuration, use:

   ```bash
   sudo systemctl restart nginx
   ```

8. **Reload Nginx**: If you've made changes to the configuration and want to apply them without stopping the service, reload Nginx:

   ```bash
   sudo systemctl reload nginx
   ```

9. **Check Nginx Status**: To check the status of the Nginx service, use:

   ```bash
   sudo systemctl status nginx
   ```

   This command shows whether Nginx is running and provides additional details, such as process ID, active status, and the last log entries.

10. **Enable Nginx to Start at Boot**: To ensure Nginx starts automatically when the server boots, use:

    ```bash
    sudo systemctl enable nginx
    ```

#### Step 5: **Configure UFW Firewall to Allow Nginx Traffic**

11. If your server uses UFW (Uncomplicated Firewall), you need to allow traffic to Nginx.

    ```bash
    sudo ufw allow 'Nginx HTTP'
    ```

    This command opens port 80, allowing HTTP traffic to Nginx. If you're using HTTPS, also allow traffic on port 443:

    ```bash
    sudo ufw allow 'Nginx HTTPS'
    ```

12. **Check UFW Status**: To verify the firewall rules, run:

    ```bash
    sudo ufw status
    ```

    You should see rules for Nginx allowing traffic on the appropriate ports.

#### Step 6: **Test Nginx Installation**

13. Open a web browser and navigate to your server’s IP address (e.g., `http://your_server_ip`):

    ```bash
    curl http://your_server_ip
    ```

    You should see the Nginx default welcome page, confirming that Nginx is correctly installed and running.

#### Step 7: **Nginx Directory Structure**

14. The main Nginx configuration file is located at:

    ```bash
    /etc/nginx/nginx.conf
    ```

15. **Default Server Block**: Nginx serves content from the `/var/www/html` directory by default, and the default server block configuration file is located at:

    ```bash
    /etc/nginx/sites-available/default
    ```

    You can create additional server block files in the `/etc/nginx/sites-available/` directory and enable them by creating symbolic links in the `/etc/nginx/sites-enabled/` directory:

    ```bash
    sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
    ```

16. **Testing Configuration**: After making changes to any configuration files, test the configuration for syntax errors:

    ```bash
    sudo nginx -t
    ```

    If the test is successful, reload Nginx to apply the changes:

    ```bash
    sudo systemctl reload nginx
    ```

#### Step 8: **Basic Configuration**

17. **Setting Up a New Server Block**: To set up a new site, you can create a new server block configuration file:

    ```bash
    sudo nano /etc/nginx/sites-available/your_domain
    ```

    Add a basic configuration like this:

    ```nginx
    server {
        listen 80;
        server_name your_domain www.your_domain;

        root /var/www/your_domain/html;
        index index.html index.htm index.php;

        location / {
            try_files $uri $uri/ =404;
        }

        location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        }

        location ~ /\.ht {
            deny all;
        }
    }
    ```

18. **Enable the Server Block**: Link it to the `sites-enabled` directory:

    ```bash
    sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
    ```

19. **Reload Nginx**: After creating the server block, reload Nginx:

    ```bash
    sudo systemctl reload nginx
    ```

#### Step 9: **Set Up HTTPS with Let's Encrypt (Optional)**

20. If you need HTTPS for your site, use Certbot to obtain a free SSL certificate from Let’s Encrypt:

    ```bash
    sudo apt install certbot python3-certbot-nginx
    ```

21. Run Certbot to obtain the certificate and automatically configure Nginx:

    ```bash
    sudo certbot --nginx
    ```

    Follow the prompts to set up the certificate.

22. **Automatic Renewal**: Certbot will automatically renew the certificate. To ensure it works correctly, test the renewal process:

    ```bash
    sudo certbot renew --dry-run
    ```

#### Conclusion

By following these steps, you have successfully installed and configured the Nginx web server on your Ubuntu Linux server. Nginx is now ready to serve your web applications, and you can further customize its configuration to suit your needs.

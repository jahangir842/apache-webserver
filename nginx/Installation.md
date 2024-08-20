
### Identifying Web Server Type (Apache vs. Nginx) on a Linux Server

When managing a web server that hosts applications, it's often essential to determine which web server software—Apache or Nginx—is running. This is especially relevant when you have terminal access to the server. Here's a step-by-step guide to identify whether your server is running Apache or Nginx.

#### 1. **Understanding the Role of Web Servers**
   - **Apache**: Also known as Apache HTTP Server, it is one of the most widely used web server software. It is known for its flexibility, power, and extensive documentation. Apache is often used in combination with other software like PHP for dynamic web content.
   - **Nginx**: Nginx is another popular web server, often used as a reverse proxy or load balancer. It is known for its high performance, stability, and low resource consumption, especially for serving static content.

#### 2. **Checking Running Processes**

   To determine which web server is active, you can check the running processes on the server.

   - **For Apache**:
     Use the `ps` command to search for Apache processes:

     ```bash
     ps aux | grep apache2
     ```
     or
     ```bash
     ps aux | grep httpd
     ```

     - **Expected Output**: If Apache is running, you should see processes listed under the names `apache2` or `httpd`. The lack of any such output suggests that Apache is not running.

   - **For Nginx**:
     Similarly, use the `ps` command to search for Nginx processes:

     ```bash
     ps aux | grep nginx
     ```

     - **Expected Output**: If Nginx is running, you will see processes such as `nginx: master process` and multiple `nginx: worker process`. These indicate that Nginx is actively running and serving requests.

#### 3. **Interpreting the Output**
   - **No Apache Processes**: The output of `ps aux | grep apache2` returns only the `grep` command itself, indicating that Apache is not running.
   - **Active Nginx Processes**: The output of `ps aux | grep nginx` shows the `nginx: master process` and multiple `nginx: worker process` entries, indicating that Nginx is running on the server.

#### 4. **Verifying with Service Status Commands**

   Another method to check if Apache or Nginx is installed and running is by checking the service status:

   - **For Apache**:
     ```bash
     sudo systemctl status apache2
     ```
     or
     ```bash
     sudo systemctl status httpd
     ```

     - **Output**: If Apache is active, you will see a status message showing that the service is running.

   - **For Nginx**:
     ```bash
     sudo systemctl status nginx
     ```

     - **Output**: If Nginx is active, the status message will confirm that the Nginx service is running.

#### 5. **Checking Open Ports**

   Web servers typically listen on ports 80 (HTTP) and 443 (HTTPS). You can use `netstat` or `ss` to see which service is bound to these ports:

   ```bash
   sudo netstat -tuln | grep ':80\|:443'
   ```
   or
   ```bash
   sudo ss -tuln | grep ':80\|:443'
   ```

   - **Apache**: If Apache is running, the output will show `:80` or `:443` bound to `apache2` or `httpd`.
   - **Nginx**: If Nginx is running, you will see `:80` or `:443` bound to `nginx`.

#### 6. **Reviewing Configuration Files**

   You can also inspect the configuration files to determine which web server is configured on the server:

   - **Apache Configuration**:
     - Typically located at `/etc/apache2/apache2.conf` or `/etc/httpd/conf/httpd.conf`.
     - Use the following command to inspect the Apache configuration file:
       
       ```bash
       sudo cat /etc/apache2/apache2.conf
       ```
       or
       ```bash
       sudo cat /etc/httpd/conf/httpd.conf
       ```

   - **Nginx Configuration**:
     - Typically located at `/etc/nginx/nginx.conf`.
     - Use the following command to inspect the Nginx configuration file:

       ```bash
       sudo cat /etc/nginx/nginx.conf
       ```

   Inspecting these files will provide insights into the configurations of the respective web servers.

#### 7. **Testing with `curl`**

   You can use `curl` to send a request to your server and inspect the response headers. This might give you additional clues about the web server:

   ```bash
   curl -I http://your-server-ip
   ```

   - Look for the `Server` header in the response:
     - **Apache**: The header might say `Server: Apache/2.4.41 (Ubuntu)`.
     - **Nginx**: The header might say `Server: nginx/1.18.0 (Ubuntu)`.

#### 8. **Conclusion**
   
   By following the steps above, you can determine whether your web application is hosted with Apache or Nginx. In this specific case, since the `ps` command output showed active Nginx processes and no Apache processes, it confirms that the server is running **Nginx**.

This process is critical for managing and troubleshooting web applications since different web servers have distinct configuration methods, performance characteristics, and module support. Understanding which server is running allows you to tailor your configuration and optimization efforts accordingly.

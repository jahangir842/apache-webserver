To serve a directory's contents without requiring an `index.html` file using Nginx, you can enable directory listing by using the `autoindex` directive in your Nginx configuration. Here’s how to set that up:

**Open the Nginx Configuration File**:
   Edit the default configuration file:
   ```sh
   vi /etc/nginx/conf.d/default.conf
   ```

3. **Modify the Configuration**:
   Ensure that the configuration for the root location includes the `autoindex on;` directive. Your configuration should look something like this:

   ```nginx
   server {
       listen       80;
       listen  [::]:80;
       server_name  localhost;

       location / {
           root   /usr/share/nginx/html;
           autoindex on;  # Enable directory listing
       }

       error_page   404 /404.html;

       location = /404.html {
           root   /usr/share/nginx/html;
       }
   }
   ```

### 2. **Reload Nginx**
After editing the configuration, reload Nginx to apply the changes:
```sh
nginx -s reload
```

### 3. **Access the Directory**
Now, when you access your server at `http://localhost:8000/`, you should see a list of all the files and directories within `/usr/share/nginx/html`, even if there’s no `index.html` file present.

### Summary
- Enable directory listing in your Nginx configuration using `autoindex on;`.
- Reload Nginx to apply the changes.

This setup will allow users to browse the contents of the directory directly. Let me know if you need further assistance!

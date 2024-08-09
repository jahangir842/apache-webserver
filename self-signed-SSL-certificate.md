## Configure self-signed SSL certificate


**Christian Lempa Tutorial**   https://www.youtube.com/watch?v=VH4gXcvkmOY&t=251s

**Christian Lempa Cheatsheet** https://github.com/christianlempa/cheat-sheets


Configuring a self-signed SSL certificate for an Apache server involves generating a certificate, configuring Apache to use it, and ensuring that your site is accessible over HTTPS. Here's a step-by-step guide to help you through the process:

### 1. **Install OpenSSL**
First, ensure that OpenSSL is installed on your system. This is usually installed by default on most Linux distributions. If not, you can install it using:

```bash
sudo apt-get install openssl
```

### 2. **Create a Directory for the SSL Certificate**
Create a directory to store your SSL certificate and key files.

```bash
sudo mkdir /etc/ssl/private
```

### 3. **Generate the Private Key and Certificate Signing Request (CSR)**
Run the following command to generate a private key and a self-signed SSL certificate:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/private/apache-selfsigned.crt
```

- `-x509`: This option specifies that we want a self-signed certificate.
- `-nodes`: This option tells OpenSSL to skip the option to secure the certificate with a passphrase, making it easier to work with Apache.
- `-days 365`: The certificate will be valid for 365 days.
- `-newkey rsa:2048`: This specifies that a new RSA key with a length of 2048 bits will be generated.
- `-keyout`: This option specifies where to save the private key file.
- `-out`: This option specifies where to save the self-signed certificate.

You will be prompted to enter information that will be incorporated into your certificate, such as country name, organization name, and Common Name (your domain or server name).

### 4. **Configure Apache to Use the SSL Certificate**

Next, configure Apache to use the newly generated SSL certificate and key.

#### a. **Enable SSL and Headers Modules**

Enable the necessary Apache modules:

```bash
sudo a2enmod ssl
sudo a2enmod headers
```

#### b. **Create a Configuration File for SSL**

Create a new Apache configuration file for the SSL-enabled virtual host:

```bash
sudo nano /etc/apache2/sites-available/default-ssl.conf
```

Add the following configuration:

```apache
<IfModule mod_ssl.c>
    <VirtualHost _default_:443>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        SSLEngine on
        SSLCertificateFile /etc/ssl/private/apache-selfsigned.crt
        SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key

        <FilesMatch "\.(cgi|shtml|phtml|php)$">
            SSLOptions +StdEnvVars
        </FilesMatch>
        <Directory /usr/lib/cgi-bin>
            SSLOptions +StdEnvVars
        </Directory>

        BrowserMatch "MSIE [2-6]" \
                nokeepalive ssl-unclean-shutdown \
                downgrade-1.0 force-response-1.0

        CustomLog ${APACHE_LOG_DIR}/ssl_request_log \
                "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"

        # Enable HSTS (HTTP Strict Transport Security)
        Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains"

    </VirtualHost>
</IfModule>
```

#### c. **Enable the SSL Virtual Host**

Enable the SSL virtual host with the following command:

```bash
sudo a2ensite default-ssl.conf
```

#### d. **Restart Apache**

Restart Apache to apply the changes:

```bash
sudo systemctl restart apache2
```

### 5. **Test the SSL Configuration**

To test your SSL configuration, open a web browser and navigate to `https://your-domain-or-IP`. You should see your site with a self-signed certificate warning (since the certificate is not from a trusted Certificate Authority).

### 6. **Optionally Force HTTPS**

To force HTTPS, you can add a redirect to the default HTTP virtual host configuration:

Edit the default site configuration:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Add the following line inside the `<VirtualHost *:80>` block:

```apache
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    Redirect permanent "/" "https://your-domain-or-IP/"
</VirtualHost>
```

Restart Apache again:

```bash
sudo systemctl restart apache2
```

### Notes:
- **Self-signed certificates** will trigger warnings in browsers because they aren't issued by a trusted CA. These are suitable for internal testing or personal projects but not for production use on the public internet.
- **Alternative**: For public-facing sites, consider using **Let's Encrypt** to generate a free SSL certificate that is trusted by most browsers.

This guide should cover the basics of setting up a self-signed SSL certificate with Apache.

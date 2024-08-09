# Guide: Setting Up Apache Virtual Hosts on Ubuntu 20.04

## Introduction

The Apache HTTP server is one of the most widely used open-source web servers, known for its versatility, power, and robust support in the development community. Unlike many other servers that require configuration in a single, large file, Apache’s configuration is modular. This allows for adding or modifying configuration files as needed. Within this framework, Apache can manage multiple websites or domains on a single server using a feature called *Virtual Hosts*.

*Virtual Hosts* enable a single Apache instance to serve multiple websites, each with its unique domain. These sites can be hosted on the same server without the user being aware that the same server hosts other sites. This setup is scalable, limited only by your server's capacity to handle the load.

In this guide, we'll walk through setting up Apache Virtual Hosts on an Ubuntu 20.04 server. By the end, you'll be able to host different websites under different domains using the same server.

## Prerequisites

Before proceeding, ensure you have the following:

- **An Ubuntu 20.04 server** with a non-root user with sudo privileges. If you haven’t set this up yet, follow the [Initial Server Setup with Ubuntu 20.04](https://example.com/initial-server-setup) guide.
- **Apache installed** on the server. You can install Apache by following the first three steps in [How To Install the Apache Web Server on Ubuntu 20.04](https://example.com/install-apache-ubuntu).

If you're using DigitalOcean, refer to the [How to Add Domains](https://example.com/add-domains) documentation to set up domains.

### Domain Configuration

To successfully complete this guide, you'll need two domains configured with the following:

- **An A record** pointing `your_domain` to your server's public IP address.
- **An A record** pointing `www.your_domain` to your server's public IP address.

If you're using another provider, refer to their documentation for setting up domain records.

**Note:** If you don't have actual domains, you can use test values locally on your computer. Step 6 will show you how to configure these values to validate your setup.

---

## Step 1: Creating the Directory Structure

First, create a directory structure to hold the website data you'll be serving.

Apache's *DocumentRoot*—the top-level directory where Apache looks for the content to serve—will be set to specific directories under the `/var/www` directory. Create a directory for each of your virtual hosts:

```bash
sudo mkdir -p /var/www/your_domain_1/public_html
sudo mkdir -p /var/www/your_domain_2/public_html
```

Replace `your_domain_1` and `your_domain_2` with your actual domain names. For example, if one of your domains is `example.com`, your directory structure should look like `/var/www/example.com/public_html`.

---

## Step 2: Granting Permissions

The directories you just created are owned by the root user. To allow your regular user to modify files in these directories, change the ownership:

```bash
sudo chown -R $USER:$USER /var/www/your_domain_1/public_html
sudo chown -R $USER:$USER /var/www/your_domain_2/public_html
```

The `$USER` variable automatically fills in with the username you’re currently logged in as.

Next, modify permissions to ensure read access is granted to the general web directory:

```bash
sudo chmod -R 755 /var/www
```

Your web server now has the necessary permissions to serve content, and your user can create content within these folders.

---

## Step 3: Creating Default Pages for Each Virtual Host

Now that your directory structure is set up, create a basic HTML page for each virtual host site.

For the first site:

```bash
nano /var/www/your_domain_1/public_html/index.html
```

Add the following content:

```html
<html>
  <head>
    <title>Welcome to your_domain_1!</title>
  </head>
  <body>
    <h1>Success! The your_domain_1 virtual host is working!</h1>
  </body>
</html>
```

Save and close the file.

Copy this file to create the page for the second site:

```bash
cp /var/www/your_domain_1/public_html/index.html /var/www/your_domain_2/public_html/index.html
```

Edit the copied file:

```bash
nano /var/www/your_domain_2/public_html/index.html
```

Modify the content as needed:

```html
<html>
  <head>
    <title>Welcome to your_domain_2!</title>
  </head>
  <body>
    <h1>Success! The your_domain_2 virtual host is working!</h1>
  </body>
</html>
```

Save and close the file.

---

## Step 4: Creating New Virtual Host Files

Virtual host files dictate how Apache responds to different domain requests. Apache includes a default virtual host file, `000-default.conf`. You’ll copy this file to create configurations for each of your domains.

Copy the default configuration file for the first domain:

```bash
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/your_domain_1.conf
```

Open the new file for editing:

```bash
sudo nano /etc/apache2/sites-available/your_domain_1.conf
```

Modify the file to reflect your domain:

```apache
<VirtualHost *:80>
    ServerAdmin admin@your_domain_1
    ServerName your_domain_1
    ServerAlias www.your_domain_1
    DocumentRoot /var/www/your_domain_1/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Save and close the file.

Repeat the process for the second domain:

```bash
sudo cp /etc/apache2/sites-available/your_domain_1.conf /etc/apache2/sites-available/your_domain_2.conf
sudo nano /etc/apache2/sites-available/your_domain_2.conf
```

Edit the file for the second domain:

```apache
<VirtualHost *:80>
    ServerAdmin admin@your_domain_2
    ServerName your_domain_2
    ServerAlias www.your_domain_2
    DocumentRoot /var/www/your_domain_2/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Save and close the file.

---

## Step 5: Enabling the New Virtual Host Files

With your virtual host files in place, it's time to enable them. Apache provides the `a2ensite` tool for this purpose.

Enable your virtual host sites:

```bash
sudo a2ensite your_domain_1.conf
sudo a2ensite your_domain_2.conf
```

You’ll receive a reminder to reload Apache:

```bash
sudo systemctl reload apache2
```

Before reloading, disable the default site:

```bash
sudo a2dissite 000-default.conf
```

Test your configuration for errors:

```bash
sudo apache2ctl configtest
```

If everything is configured correctly, you should see:

```bash
Syntax OK
```

Finally, restart Apache to apply the changes:

```bash
sudo systemctl restart apache2
```

---

## Step 6: (Optional) Setting Up Local Hosts File

If you don’t have real domains and are using example domains instead, you can still test your virtual host configuration locally by modifying the *hosts* file on your computer.

### On macOS or Linux

Edit your local *hosts* file:

```bash
sudo nano /etc/hosts
```

### On Windows

Open the Command Prompt and type:

```bash
notepad %windir%\system32\drivers\etc\hosts
```

Add your server's IP address and the corresponding domain names:

```bash
your_server_IP your_domain_1
your_server_IP your_domain_2
```

Save and close the file.

---

## Step 7: Testing Your Results

With everything set up, test your configuration by visiting the domains in your web browser:

- [http://your_domain_1](http://your_domain_1)
- [http://your_domain_2](http://your_domain_2)

If both sites display correctly, you've successfully configured multiple virtual hosts on your Apache server.

**Note:** If you modified your local *hosts* file for testing, consider removing the entries now that your setup is verified.

---

## Conclusion

You’ve now configured Apache to handle multiple domains on a single server using Virtual Hosts. You can expand this setup by following the same steps to add additional virtual hosts, allowing your server to manage more domains as needed. Apache has no software limit on the number of domains it can serve—just be mindful of your server’s hardware limitations.

--------------------------------
versions detail
--------------------------------

- litespeed version now   	1.7.16 
- litespeed version available 	1.7.19 (stable)

- litespeed version available  	1.8.2 (current branch)

---------------------------------

- lsphp now	  73
- lsphp latest	  83 (ubuntu 22 and 24)
- available: 	  74
- PHP versions 5.6*, 7.0*, 7.1*, 7.2*, 7.3*, 7.4*, 8.0, 8.1, 8.2 and 8.3.
- Ubuntu versions 18.04*, 20.04, 22.04 and 24.04.

---------------------------------

mysql  Ver 14.14 Distrib 5.7.40, for Linux (x86_64) using  EditLine wrapper

Install MySQL 8.0:

------------------------------------

---

## How to Change Your Website's PHP Version in OpenLiteSpeed

https://www.youtube.com/watch?v=jhSx-vcx710

apt install lsphp80 lsphp80-common lsphp80-mysql

- verify that is availabe now in /usr/local/lsws/lsphp80
- Server Configuration > External App

## update litespeed

https://docs.litespeedtech.com/lsws/updates/

## Get started with LSPHP

https://docs.litespeedtech.com/lsws/extapp/php/getting_started/


-----

# Live Updating Procedute

To update **OpenLiteSpeed** from version **1.7.19** to the latest version on **Ubuntu 18.04**, follow these steps. I will also cover possible issues and how to resolve them.

### Step 1: Backup Your Configuration and Data
Before updating, create backups of important files and databases to avoid any accidental loss during the update.

1. **Backup OpenLiteSpeed configuration**:
   ```bash
   sudo cp -r /usr/local/lsws/conf /usr/local/lsws/conf.bak
   ```

2. **Backup WordPress files**:
   ```bash
   sudo tar -czf wordpress_backup.tar.gz /usr/local/lsws/altebby.com
   ```

3. **Backup your MySQL databases**:
   ```bash
   sudo mysqldump -u root -p --all-databases > all_databases_backup.sql
   ```

### Step 2: Update the OpenLiteSpeed Repository
OpenLiteSpeed provides an official repository. To ensure you're getting the latest version, you need to update this repository.

1. **Add OpenLiteSpeed Repository**:
   If you haven't already added the repository, do so with these commands:
   ```bash
   sudo wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh | sudo bash
   ```

2. **Update the package index**:
   ```bash
   sudo apt update
   ```

### Step 3: Install the Latest Version of OpenLiteSpeed
Now, you can update the OpenLiteSpeed server to the latest version using the following command:

```bash
sudo apt install --only-upgrade openlitespeed
```

### Step 4: Restart OpenLiteSpeed
After updating, restart OpenLiteSpeed for the changes to take effect.

```bash
sudo systemctl restart lsws
```

### Step 5: Verify the Update
To verify that the update was successful, check the version of OpenLiteSpeed:

```bash
/usr/local/lsws/bin/openlitespeed -v
```

### Issues That May Arise and How to Fix Them

#### 1. **Configuration Compatibility Issues**
- **Problem**: OpenLiteSpeed configuration files may become incompatible with newer versions, especially custom settings.
- **Solution**: Compare your backup configuration (`/usr/local/lsws/conf.bak`) with the new configuration and apply changes carefully. Look for deprecated directives or settings in the release notes of the version you're upgrading to.

#### 2. **WordPress Compatibility Issues**
- **Problem**: Newer OpenLiteSpeed versions might not work with your current PHP, MySQL, or WordPress setup.
- **Solution**: Ensure that your PHP version is compatible with WordPress. WordPress often works best with PHP 7.4 or higher, so if PHP 7.3 becomes problematic, consider upgrading PHP (details below). Also, ensure that MySQL 5.7 is compatible with the updated WordPress plugins and themes you use.

#### 3. **PHP 7.3 Compatibility**
- **Problem**: PHP 7.3 might no longer be supported in newer OpenLiteSpeed releases.
- **Solution**: Consider upgrading PHP. You can install newer PHP versions (e.g., PHP 7.4 or PHP 8.0) using OpenLiteSpeed’s `lsphp` packages. For example:
  
  ```bash
  sudo apt install lsphp74 lsphp74-mysql
  ```

  Then, configure OpenLiteSpeed to use the newer PHP version by editing the external application settings in the WebAdmin console.

#### 4. **MySQL Compatibility**
- **Problem**: If your MySQL 5.7 version is incompatible with newer OpenLiteSpeed and WordPress setups, you may experience issues like database errors or poor performance.
- **Solution**: Upgrade MySQL to version 8.0 if compatibility issues arise. Follow the official MySQL guide to upgrade your database, but remember to backup your data.

#### 5. **SSL and Security**
- **Problem**: SSL settings and modules such as `mod_security` may require updates to work with the new OpenLiteSpeed version.
- **Solution**: Check your SSL certificate and security settings after the upgrade. Verify that SSL certificates are still functional by running:

  ```bash
  sudo /usr/local/lsws/admin/misc/admcfgcert.sh
  ```

#### 6. **Caching Issues (LiteSpeed Cache Plugin)**
- **Problem**: LiteSpeed Cache for WordPress (LSCache) might need updating or reconfiguration post-upgrade.
- **Solution**: Ensure the LSCache plugin is updated to the latest version and clear any caches after upgrading OpenLiteSpeed.

### Step 6: Update PHP (if necessary)
If PHP 7.3 is not supported, or if you want to upgrade, install the latest PHP version supported by OpenLiteSpeed:

1. **Install PHP 7.4 or 8.0**:
   ```bash
   sudo apt install lsphp74 lsphp74-mysql
   ```

2. **Set OpenLiteSpeed to use the new PHP version**:
   - Access OpenLiteSpeed’s WebAdmin at `https://your_server_ip:7080`
   - Go to **Server Configuration > External App > lsphp** and modify the path to the new PHP version:
     - For PHP 7.4: `/usr/local/lsws/lsphp74/bin/lsphp`

3. **Restart OpenLiteSpeed**:
   ```bash
   sudo systemctl restart lsws
   ```

### Additional Resources
- **LiteSpeed Official Docs**: [OpenLiteSpeed Upgrade Guide](https://openlitespeed.org/kb/upgrading/)
- **PHP Compatibility for WordPress**: [PHP Versions Supported by WordPress](https://make.wordpress.org/hosting/handbook/handbook/server-environment/#php)
- **MySQL Official Upgrade Docs**: [MySQL 5.7 to MySQL 8.0 Upgrade](https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html)

By following these steps, you'll ensure a smooth upgrade of OpenLiteSpeed, minimizing downtime and maintaining compatibility with your existing WordPress, PHP, and MySQL setup.

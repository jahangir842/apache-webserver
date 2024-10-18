## Change Your Website's (WordPress) PHP Version in OpenLiteSpeed

To update your PHP version in OpenLiteSpeed for a WordPress site, follow these steps:

### 1. Verify the PHP Version in WordPress

To check the current PHP version being used in WordPress:
- Go to **Tools** > **Site Health** > **Info**.
- Under the **Server** section, locate the current PHP version.

### 2. Verify the PHP Version on the System

To see which PHP version is used by LiteSpeed on your system, you can run:
```bash
ls /usr/local/lsws
```
This will show entries like `lsphp73` or `lsphp74` that represent the installed PHP versions.

### 3. Install the Latest PHP Version

To install a newer version of PHP, such as PHP 8.0, run the following command:

```bash
sudo apt install lsphp80 lsphp80-common lsphp80-mysql
```

Once installed, confirm the new version is available by listing the contents again:
```bash
ls /usr/local/lsws
```

### 4. Configure PHP in LiteSpeed Server

- Log into your LiteSpeed Web Admin Panel.
- Navigate to **Server Configuration** > **External App**.
- Here, you'll see the current PHP version entry. To add a new version, copy the settings of the existing entry:
    1. Click the **+** button.
    2. Select **LiteSpeed SAPI App** and click **Next**.
    3. Fill in the details by copying the old entry for the new PHP version (e.g., PHP 8.0) and saving it.

### 5. Associate the New PHP Version with Your Website

- In the LiteSpeed Web Admin Panel, go to **Virtual Hosts**.
- Click on view button in your website.
- Click on **Script Handler**.
    - If an entry for PHP exists, edit it. Otherwise, click the **+** button to add a new script handler.
    - Fill in the details as follows:
        - **Suffixes**: `php`
        - **Handler Type**: `LiteSpeed SAPI`
        - **Handler Name**: `[Server Level]: lsphp80`
- Save the changes and restart the server gracefully by clicking the green button.

### 6. Verify the PHP Version Again

To ensure the PHP version has been updated successfully:
- Go back to **Tools** > **Site Health** > **Info** in WordPress and check the PHP version under the **Server** section.

### 6. Verify Website

Verify that the website is running smoothly after the update.

### Change Your Website's (Wordpress) PHP Version in OpenLiteSpeed

**Reference:** 
- https://www.youtube.com/watch?v=jhSx-vcx710
- https://docs.litespeedtech.com/lsws/extapp/php/getting_started/

### Verify the php version in Wordpress:

go to tools, sitehealth, info and in server check the current php version.

### Verify the php version in System:

In system the version of php used by litespeed can be verified from: 

```
ls /usr/local/lsws
```

it will show here like lsphp73 of lsphp74 etc.

### Install latest php version:
The new version can be installed like:

```
apt install lsphp80 lsphp80-common lsphp80-mysql
```
- verify that is available

```
ls /usr/local/lsws
```

### Configure php in LiteSpeed Server
login into litespeed webpanel,
Go to Server Configuration > External App
there will be entry of current version of PHP, now we will add a new entry here by copying the settings of current one with details of latest version of php which we have install now. first click on plus (+) button, select "LitespeedSAPI App" and click on next button, here fill the detail and save it.

### Associate php with Website Itself

login into litespeed webpanel,
brows virtual hosts, click on script handler, 
Edit the available script, if not available, then click plus (+) button to add new script, 
fill like this:
suffices: PHP
Handler Type: LiteSpeed SAPI
Handler Name: [Server Level]: lsphp80

Then Save it.
and click on Green Button to restart server Gracefully.

### Verify the php Version Again:

Verify the php version again to ensure it has been update successfully.



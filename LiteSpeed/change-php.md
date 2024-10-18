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





- verify that is availabe now in /usr/local/lsws/lsphp80
- Server Configuration > External App



# fix-htaccess-notread
Fixing htaccess file not read on apache2

You try to setup a project with relative urls only to discover that the browser displays an unexplainable error (“page not found” or the famous “localhost not working” error). You ensure that the rules in the htaccess are correct. Still the same thing. Suddenly, it hits you – probably the htaccess is not being read correctly or maybe not being read at all! Well you couldn’t be more right! <br>
![Screenshots 1](https://i0.wp.com/digitalfortress.tech/wp-content/uploads/2019/09/kQMdz.png) <br>

# How to fix htaccess that is not read?
Turns out, htaccess file has to be enabled (which is not enabled by default). This can be done in 2 easy steps – <br> <br>
**1. Enabling htaccess file –** <br>
To enable htaccess, you need to update the apache configuration. <br>
To find your apache config file, you can use the following commands –
```bash
// Get absolute path of Apache
$ ps -ef | grep apache
apache   12846 14590  0 Oct20 ?        00:00:00 /usr/sbin/apache2
 
// Get the root and the name of the config file
$ /usr/sbin/apache2 -V | grep HTTPD_ROOT           // /etc/apache2/
$ /usr/sbin/apache2 -V | grep SERVER_CONFIG_FILE   // apache2.conf
```
As seen, we found out that our config file is located at /etc/apache2/apache2.conf <br>
(For some shared hosts, the config file can be found using the httpd -V or apache2ctl -V commands instead)
Open your apache configuration file and make changes to it –

Change `AllowOverride None`

```bash
<Directory /var/www/html/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```
to `AllowOverride All`.
```bash
<Directory /var/www/html/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>
```

This basically allows the rules written in the htaccess file to override routing rules for this directory `(/var/www/html/)`. Note that `/var/www/html` is the default web root for Ubuntu 14.x onwards. This could be different/customized as per one’s project. <br> <br>


**2. Enable module rewrite –** <br>
You can enable module rewrite by executing the following in your terminal
```bash
sudo a2enmod rewrite
```
Once you do this, restart apache so that the new changes should take effect.
```bash
 sudo service apache2 restart 
 sudo systemctl restart apache2
```
That should be it. Refreshing your page should start functioning correctly based on the rules defined in your htaccess file.<br> <br> <br>


Source: https://digitalfortress.tech/bugfix/fix-htaccess-not-being-read/

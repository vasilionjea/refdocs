# Apache

http://httpd.apache.org/docs/2.4/invoking.html
http://httpd.apache.org/docs/2.4/programs/httpd.html
http://httpd.apache.org/docs/2.4/programs/apachectl.html

httpd is the Apache HTTP server program. On Unix, the httpd program is run as a daemon that executes continuously in the background to handle requests.  It is designed to be run as a standalone daemon process. When used like this it will create a pool of child processes or threads to handle requests.

Once the server has started and performed a few preliminary activities such as opening its log files, it will launch several child processes which do the work of listening for and answering requests from clients.

The recommended method of invoking the httpd executable is to use the apachectl control script. apachectl will pass through any command line arguments, so any httpd options may also be used with apachectl. The first thing that httpd does when it is invoked is to locate and read the configuration file httpd.conf.


## Run ember /dist with an Apache VirtualHost (Mac OSX)

1. Open file `/etc/apache2/httpd.conf` and uncomment the following line:
```
  # Virtual hosts
  # Include /private/etc/apache2/extra/httpd-vhosts.conf
```
2. Open file `/etc/apache2/extra/httpd-vhosts.conf` and add the following:
```
  <VirtualHost 127.0.0.1:80>
    ServerAdmin webmaster@ember-prod.com
    DocumentRoot "/Users/--YOU--/Sites/ember-prod/"
    ServerName ember-prod.com # THIS IS THE DOMAIN NAME YOU CHOOSE
    ErrorLog "/private/var/log/apache2/ember-prod.com-error_log"
    CustomLog "/private/var/log/apache2/ember-prod.com-access_log" common

    # Fallback to HTML5 pushstate support -> ember app
    FallbackResource index.html
  </VirtualHost>
```
3. Open file `/etc/hosts` and add the following:
```
  127.0.0.1 ember-prod.com
```
4. Move your ember `/dist/` folder to `/Users/--YOU--/Sites/ember-prod/`
5. Finally turn on apache `sudo apachectl start` and go to your app http://ember-prod.com

### Resources
- http://httpd.apache.org/docs/2.2/mod/core.html#virtualhost
- https://httpd.apache.org/docs/trunk/mod/mod_dir.html#fallbackresource
- https://httpd.apache.org/docs/current/programs/apachectl.html (all you need: `sudo apachectl start|stop|restart`)
- http://ember-cli-deploy.com/ (for actual prod)
- https://coolestguidesontheplanet.com/set-virtual-hosts-apache-mac-osx-10-9-mavericks-osx-10-8-mountain-lion

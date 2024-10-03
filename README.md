## LARAVEL NOTES
### Composer Install
Update the ubuntu package
```vim
$sudo apt update
```
Install required package
```vim
$sudo apt install php-cli unzip
```
Download and install composer
```vim
$cd ~
$curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
```
Verify downloaded install and verify the installer
```vim
$HASH=`curl -sS https://composer.github.io/installer.sig`
$echo $HASH
```
```
dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6
```
Check if downloaded script is safe to run
```vim
$php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```
```
Installer verified
```
To install composer globally
```vim
$sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
```
Check composer if installed
```vim
$composer
```
### Laravel Installer

The first requirement is the PHP, with open terminal type the following command.

```vim
$sudo apt-get install php
```
Check the version of PHP. 
```vim
$php -v 
```
Also install the PHP Mbstring, XML, Zip, Curl, Extension.
```vim
$sudo apt-get install php7.0-mbstring php7.0-xml php7.0-zip php7.0-curl
```
Alternatively, you may also init a Laravel project by issuing the Composer create-project command in your terminal:
```vim
$composer create-project laravel/laravel project-name
```
Or install Laravel installer to use the following command “laravel new project”:
```vim
$composer global require "laravel/installer"
```
For the laravel command works we need append a line in bashrc file, if you only uses terminal.
```vim
$echo 'export PATH="$HOME/.composer/vendor/bin:$PATH"' >> ~/.bashrc
```
Check the .bashrc source
```vim
$source ~/.bashrc
```
Restart the terminal.

And initialize a Laravel project.
```vim
$laravel new blog
```
Run the Laravel server. Enjoy!

### Folder Permission
```vim
$chown -R www-data:www-data project-folder
```
### Setup Multiple Project Folder using Apache2
First change the .htaccess public folder add 
```vim
RewriteBase /custom-url
```
Second in your site-available sitename.conf config add the following: <br>
```vim
<Directory /var/www/html/appfolder/public>
  Options Indexes FollowSymLinks
  AllowOverride All
</Directory>
```
<i> Note: Replace *** with proper tagging </i>
```vim
DocumentRoot /var/www/html
ServerName localhost 
Alias /custom-url     /var/www/html/appfolder/public
```
Sample complete code:

```vim
<VirtualHost *:80>
  ServerName 192.486.45.1
  ServerAlias 127.0.0.1
  Alias /assetapp /var/www/html/appfolder/public
  DocumentRoot /var/www/html
  <Directory /var/www/html/appfolder/public>
    AllowOverride All
    Options -Indexes +MultiViews
    Order deny,allow
    allow from 192.486.45.1
  </Directory>
</VirtualHost>
```

### References

[Sample Eagerloading](https://vegibit.com/laravel-hasmany-and-belongsto-tutorial/)

[File upload](https://appdividend.com/2022/02/28/laravel-file-upload/)

[Laravel tutorials](https://laravel-news.com/category/tutorials)

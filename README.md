### Laravel-Notes
### Setup Laravel Installer In Ubuntu

The first requirement is the PHP, with open terminal type the following command.

```
$sudo apt-get install php
```

Check the version of PHP. 

```
$php -v 
```

Also install the PHP Mbstring, XML, Zip, Curl, Extension.

```
$sudo apt-get install php7.0-mbstring php7.0-xml php7.0-zip php7.0-curl
```

The Laravel uses the composer to manage your dependencies, so the next step is install Composer.

```
$curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

Change the permissions to run the composer command without sudo:

```
$sudo chown -R $USER ~/.composer/
```

Alternatively, you may also init a Laravel project by issuing the Composer create-project command in your terminal:

```
$composer create-project --prefer-dist laravel/laravel project-name
```

Or install Laravel installer to use the following command “laravel new project”:

```
$composer global require "laravel/installer"
```

For the laravel command works we need append a line in bashrc file, if you only uses terminal.

```
$echo 'export PATH="$HOME/.composer/vendor/bin:$PATH"' >> ~/.bashrc
```

Check the .bashrc source

```
$source ~/.bashrc
```

Restart the terminal.

And initialize a Laravel project.

```
$laravel new blog
```

Run the Laravel server. Enjoy!

### Folder Permission
```
$chown -R www-data:www-data FolderName
```

### Setup Multiple Project Folder

First change the .htaccess public folder add 

```
RewriteBase /custom-url
```

Second in your site-available sitename.conf config add the following: <br>

```
***Directory /var/www/html/appfolder/public***
Options Indexes FollowSymLinks
AllowOverride All
***Directory***
```

<i> Note: Replace *** with proper tagging </i>

```
DocumentRoot /var/www/html
ServerName localhost 
Alias /custom-url     /var/www/html/appfolder/public<b>
```

Sample complete code:

```
<VirtualHost *:80>

  ServerName 192.486.45.1
  ServerAlias 127.0.0.1
  Alias /ilcasset /var/www/html/appfolder/public
  DocumentRoot /var/www/html

  <Directory /var/www/html/appfolder/public>
     AllowOverride All
         Options -Indexes +MultiViews
         Order deny,allow
         allow from 192.486.45.1
  </Directory>

</VirtualHost>
```

### Lumen
Generate project of Lumen
```
Generate project for lumen
$composer create-project --prefer-dist laravel/lumen blog-app
```
Generate app key for .env using PHP interactive mode
```
$php -a
echo substr(md5(rand()),0,32); //32 chars
```

### Eager Loading

```
//User
public post function(){
    return $this->hasMany(PostClass::class);
}
//Post
public userpost function(){
    return $this->belongsto(UserClass::class);
}

//Schema User
id
name
//Schema Post
id
user_id
post
```
### References

[Sample Eagerloading](https://vegibit.com/laravel-hasmany-and-belongsto-tutorial/)

[File upload](https://appdividend.com/2022/02/28/laravel-file-upload/)

[Laravel tutorials](https://laravel-news.com/category/tutorials)

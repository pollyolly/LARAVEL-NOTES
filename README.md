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
### Setup Project
/var/www/project-folder/.env
```
APP_NAME="Inventory App"
APP_ENV=development
APP_KEY=generated_unique_key_application
APP_DEBUG=true
APP_URL=http://domain_or_ip

 DB_CONNECTION=mysql
 DB_HOST=127.0.0.1
 DB_PORT=3306
 DB_DATABASE=inventory_app
 DB_USERNAME=root
 DB_PASSWORD=samplePassword
```
### Folder Permission
```vim
$sudo chown -R www-data.www-data /var/www/project-folder
$sudo chown -R www-data.www-data /var/www/project-folder/storage
$sudo chown -R www-data.www-data /var/www/project-folder/bootstrap/cache
```
### Setup NginX
```vim
$sudo nano /etc/nginx/sites-available/project-folder
```
```vim
server {
    listen 80;
    server_name server_domain_or_ip.com;
    root /var/www/project-folder/public;
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";
    index index.html index.htm index.php;
    charset utf-8;
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
    error_page 404 /index.php;
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
```vim
$sudo ln -s /etc/nginx/sites-available/project-config /etc/nginx/sites-enabled/
```
```vim
$nginx -t
```
```vim
$service nginx reload
```
```vim
http://server_domain_or_ip.com
```
### Setup Multiple Project Folder using Apache2
First change the .htaccess public folder add 
```vim
RewriteBase /myapp
```
Second in your site-available sitename.conf config add the following: <br>
```vim
<Directory /var/www/html/project-folder/public>
  Options Indexes FollowSymLinks
  AllowOverride All
</Directory>
```
```vim
DocumentRoot /var/www/html
ServerName localhost 
Alias /myapp-url     /var/www/html/project-folder/public
```
```vim
<VirtualHost *:80>
  ServerName 192.486.45.1 myapp.com
  ServerAlias 127.0.0.1 localhost
  Alias /myapp-url /var/www/html/project-folder/public
  DocumentRoot /var/www/html
  <Directory /var/www/html/project-folder/public>
    AllowOverride All
    Options -Indexes +MultiViews
    Order deny,allow
    allow from 192.486.45.1
  </Directory>
</VirtualHost>
```
### The Routes, Views, Controller, Models, Migration, Factories, Seeders
routes
```vim
project-folder/routes/web.php 
```
views 
```vim
project-folder/resources/views/welcome.blade.php
commands:
$php artisan make:view welcome
```
Controller
```vim
project-folder/app/Http/Controllers/UserController.php
commands:
$php artisan make:controller UserController
```
Models
```vim
project-folder/app/Models/UserModel.php
commands:
$php artisan make:model UserModel
```
Migrations (Handle database and table changes)
```vim
project-folder/database/migrations
commands:
//Create migration
$php artisan make:migration create_product_table
//Run any pending and new migrations
$php artisan migrate
```
factories
```
https://fakerphp.org/formatters/numbers-and-strings/)
https://learninglaravel.net/cheatsheet/#schema
```
```vim
project-folder/database/factories/
commands:
$php artisan make:factory UserFactory
```
seeders
```vim
project-folder/database/seeders/
commands:
//Create Seeder
$php artisan make:seeder UserSeeder
//Run seeder all or individually
$php artisan db:seed
$php artisan db:seed --class=UserSeeder
```
## Warning
This Will Empty Your Table Rows and Drop All Tables
```
$php artisan migrate:fresh
$php artisan migrate:refresh
```
### Blade Templating
Form Request
```vim
<form action="/custom-url" action="POST">
@csrf
@method('PUT') <!-- Update Route (web.php) Route:put() -->
<button>Update</button>
</form>
```
Adding blade file to blade file
```vim
@include('header') <!-- header.blade.php -->
@include('footer') <!-- footer.blade.php -->
```
Adding script to blade
```vim
<html>
 <body>
  @stack('scripts') <!-- Add here the Script -->
 </body>
<html>

@push('scripts') <!-- Script to add -->
 <script>alert('hello');</script>
@endpush
```
Adding section to extended blade file
```vim
//layout.blade.php
<html>
 @yield('body') <!-- Add here the Section -->
</html>

//home.blade.php
@extends('layout') <!-- Extended layout.blade.php -->
@section('body') <!-- Section to add in layout.blade.php -->
 <body>
  <h1>Body</h1>
 </body>
@endsection
```
Display errors
```vim
<!-- Display all error -->
@if($errors->any())
 {!! implode('', $errors->all('<div>:message</div>')) !!}
@endif
<!-- Display per input -->
<input name="product_name" />
@error('product_name') <!-- Input name -->
 <p>Error Product Name Required</p>
@enderror
```
### References
[Laravel tutorials](https://laravel-news.com/category/tutorials)

[Install Composer on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-20-04)

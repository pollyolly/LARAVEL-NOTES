### Installation
Install composer
```
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-20-04
```
Install laravel 5.5
```
composer create-project --prefer-dist laravel/laravel qr_ticket_management "5.5.*"
```
Configure Database
```
cd qr_ticket_management
vi qr_ticket_management/.env

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=mydatabase
DB_USERNAME=myusername
DB_PASSWORD=mypassword
```
Install laravel admin
```
composer require encore/laravel-admin
```
Publish assets and config：
```
php artisan vendor:publish --provider="Encore\Admin\AdminServiceProvider"
```
Finish install
```
php artisan admin:install
```
Reconfigure Laravel Admin
```
cd qr_ticket_management
$php artisan key:generate
$php artisan cache:clear
$php artisan migrate
$chmod -R 775 storage
$composer dump-autoload
$chmod -R 775 bootstrap/cache
```


Setup Nginx
```
server {
        listen 80;
        listen [::]:80;
        root /var/www/html/qr_ticket_management/public;
        index index.php index.html;
        server_name _;
        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }
        location ~ \.php$ {
                #NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
                include fastcgi_params;
                fastcgi_intercept_errors on;
                fastcgi_pass unix:/run/php/php7.4-fpm.sock;
                fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
        }
}
```

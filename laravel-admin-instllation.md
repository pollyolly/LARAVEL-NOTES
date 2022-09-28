### LARAVEL ADMIN INSTALLATION
```
https://laravel-admin.org/docs/en/#Installation
```
Install composer
```
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-20-04
```
PHP Extensions
```
BCMath PHP Extension
Ctype PHP Extension
Fileinfo PHP extension
JSON PHP Extension
Mbstring PHP Extension
OpenSSL PHP Extension
PDO PHP Extension
Tokenizer PHP Extension
XML PHP Extension
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
$php artisan storage:link
$php artisan key:generate
$php artisan cache:clear
$php artisan migrate
$chmod -R 775 storage
$composer dump-autoload
$chmod -R 775 bootstrap/cache
```
Disk [admin] not configured, please add a disk config in `config/filesystems.php`.
```
https://laravel-admin.org/docs/en/model-form-upload
Open 'config/filesystems.php'  add this to the disk array :

        'admin' => [
                'driver' =>'local',
                'root' => public_path('uploads'),
                'visibility' =>'public',
                'url' => env('APP_URL').'/uploads',
        ],
```

Setup Nginx
```
server {
        listen 80;
        listen [::]:80;
        root /var/www/html/qr_ticket_management/public;
        index index.php index.html;
        server_name _;
        
        add_header X-Frame-Options "SAMEORIGIN";
        add_header X-XSS-Protection "1; mode=block";
        add_header X-Content-Type-Options "nosniff";
        charset utf-8;
        
        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }
        
        location = /favicon.ico { access_log off; log_not_found off; }
        location = /robots.txt  { access_log off; log_not_found off; }
        
        location ~ \.php$ {
                #NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
                include fastcgi_params;
                fastcgi_intercept_errors on;
                fastcgi_pass unix:/run/php/php7.4-fpm.sock;
                fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
                fastcgi_buffers 4 16k;
                fastcgi_buffer_size 16k;
        }
        
        location ~ /\.(?!well-known).* {
                deny all;
        }
}
```
Services
```
service mysql start
service nginx start
service php7.4-fpm start
```

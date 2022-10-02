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
Class 'Doctrine\DBAL\Driver\PDOMySql\Driver' not found
```
Check for Laravel 5.5
$composer require doctrine/dbal
or
$composer require doctrine/dbal:2.*
```
Creating Controller and Model
```
Model
$php artisan make:model Models\QRTickets -m

Controller
$php artisan admin:make QRTicketsController --model='App\Models\QRTickets'
```
Updating Columns Database (Go to Latest migration /database/migration/)
```
    public function up()
    {
        Schema::create('q_r_tickets', function (Blueprint $table) {
            $table->increments('id');
            $table->string('ticket_number');
            $table->string('ticket_subject');
            $table->text('ticket_content');
            $table->integer('user_id')->unsigned();
            $table->foreign('user_id')->references('id')->on('users');
            $table->timestamps();
        });
    }
    
$php artisan migrate
```
Important Default Locations
```
appfolder/app/database/migration/ - Laravel Database Schema
appfolder/app/                    - Admin Models
appfolder/app/Admin/Controllers/  - Admin Controllers
appfolder/app/Admin/routes.php    - Admin Routings
appfolder/config/admin.php        - Extensions Configs
appfolder/config/filesystems.php  - Laravel File Upload Settings
appfolder/.env                    - Laravel DB, SMTP, etc settings
```
Move Model to Models Folder
```
1. Update app/Model
From: namespace App; 
to: namespace App\Models;

2. Update app/Admin/Controller (app/Http/Controllers/Auth/ and app/Admin/Controllers/)
From: use App\QRTickets; 
to: use App\Models\QRTickets;

3. Update app/config/auth.php
From: 'model' => App\User::class, 
to: 'model' => App\Models\User::class,

4. Update app/database/factories
From: $factory->define(App\User::class 
to: $factory->define(App\Models\User::class

5. Update app/config/services.php
From: 'model' => App\User::class, 
to: 'model' => App\Models\User::class,

6. Run
$composer dump-autoload
$chown -R www-data:www-data appfolder

To Test:
$php artisan make:model Models\Test -m
```
Delete Model
```
1. Delete the Model
2. Delete the Model migration in /database/migration
3. This will Remove All the Data in Database
   $php artisan migrate:refresh 
```
Unable to reset Laravel Admin
```
1. Backup Current Laravel Customizations
2. Uninstall laravel-admin
$php artisan admin:uninstall
3. Reinstall Laravel Admin
$composer require encore/laravel-admin
$php artisan vendor:publish --provider="Encore\Admin\AdminServiceProvider"
$php artisan admin:install
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

Tutorials

[Laravel Admin Walkthrough](https://www.youtube.com/watch?v=F0ujUOAgWqg)

[Install Laravel Admin Panel | Admin Dashboard in Laravel](https://www.youtube.com/watch?v=1-6vBAPvU4k)

[Laravel](https://laravel.com/docs/9.x/eloquent-relationships)


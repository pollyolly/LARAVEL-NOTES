### Installation
Install composer
```
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-20-04
```
Install laravel 5.5
```
composer create-project --prefer-dist laravel/laravel blog "5.5.*"
```
Install laravel admin
```
composer require encore/laravel-admin
```
publish assets and config：
```
php artisan vendor:publish --provider="Encore\Admin\AdminServiceProvider"
```
finish install
```
php artisan admin:install
```

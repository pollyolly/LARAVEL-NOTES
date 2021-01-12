### Laravel-Notes
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

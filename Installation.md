Setup Laravel Installer In Ubuntu

The first requirement is the PHP, with open terminal type the following command.

<pre>
<b>sudo apt-get install php</b>
</pre>

Check the version of PHP. 

<pre>
<b>php -v</b> 
</pre>

Also install the Mbstring PHP Extension.

<pre>
<b>sudo apt-get install php7.0-mbstring</b> 
</pre>

The XML PHP Extension.

<pre>
<b>sudo apt-get install php-xml</b>
</pre>

And the ZIP PHP Extension.

<pre>
<b>sudo apt-get install php7.0-zip</b>
</pre>

If you don’t have already installed curl in your machine, type:

<pre>
<b>sudo apt-get install curl</b>
</pre>

The Laravel uses the composer to manage your dependencies, so the next step is install Composer.

<pre>
<b>curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer</b>
</pre>

Change the permissions to run the composer command without sudo:

<pre>
<b>sudo chown -R $USER ~/.composer/ </b>
</pre>

Alternatively, you may also init a Laravel project by issuing the Composer create-project command in your terminal:

<pre>
<b>composer create-project --prefer-dist laravel/laravel project-name </b>
</pre>

Or install Laravel installer to use the following command “laravel new project”:

<pre>
<b>composer global require "laravel/installer" </b>
</pre>

For the laravel command works we need append a line in bashrc file, if you only uses terminal.

<pre>
<b>echo 'export PATH="$HOME/.composer/vendor/bin:$PATH"' >> ~/.bashrc </b>
</pre>

Check the .bashrc source

<pre>
<b>source ~/.bashrc </b>
</pre>

Restart the terminal.

And initialize a Laravel project.

<pre>
<b>laravel new blog</b>
</pre>

Run the Laravel server. Enjoy!

<br> ============================== <br>

Folder Permission:

<pre>
<b>chown -R www-data:www-data FolderName</b>
</pre>

<br> ============================= <br>

Setup multiple project folder:

First change the .htaccess public folder add 

<pre>
<b>RewriteBase /custom-url</b>
</pre>

Second in your site-available sitename.conf config add the following: <br>

<b>
<pre>
***Directory /var/www/html/appfolder/public***
Options Indexes FollowSymLinks
AllowOverride All
***Directory***
</pre>
</b>

<i> Note: Replace *** with proper tagging </i>

<b>
<pre>
<b>
DocumentRoot /var/www/html
ServerName localhost 
Alias /custom-url     /var/www/html/appfolder/public<b>
</pre>

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

## disable wp cron
```sh
ADD CRON JOB wget -q -O - https://example.com/wp-cron.php?doing_wp_cron
```
```php
/* production.php */

use Roots\WPConfig\Config;

Config::define('DISALLOW_INDEXING', false);
Config::define('DISALLOW_FILE_MODS', false);
Config::define('DISABLE_WP_CRON', true);
```
```php
define('DISABLE_WP_CRON', true);
```
## Enable redis for bedrock Object Cache Pro:
```php
if (!empty(env('WP_REDIS_TOKEN',''))) {
	Config::define('WP_REDIS_CONFIG', [
		'token' => env('WP_REDIS_TOKEN'),
        'host' =>  env('WP_REDIS_HOST'), // '127.0.0.1' by default
		'port' => env('WP_REDIS_PORT'),
		'database' => env('WP_REDIS_DATABASE'),
		'timeout' => 2.5,
		'read_timeout' => 2.5,
		'split_alloptions' => true,
		'async_flush' => true,
		'client' => 'phpredis',
		'compression' => 'zstd',
		'serializer' => 'igbinary',
		'prefetch' => true,
		'debug' => false,
		'save_commands' => false,
		'prefix' => Config::get('DB_NAME'),
	]);
	Config::define( 'WP_REDIS_DISABLED', false );
}
```

## .env new variables
```sh
WP_REDIS_TOKEN=""
WP_REDIS_PORT=""
WP_REDIS_DATABASE=""
```

## ERRORS and WARNINGS ARRAY
```php
$errorsActive = [
    E_ERROR             => TRUE,
    E_WARNING           => FALSE,
    E_PARSE             => FALSE,
    E_NOTICE            => FALSE,
    E_CORE_ERROR        => FALSE,
    E_CORE_WARNING      => FALSE,
    E_COMPILE_ERROR     => FALSE,
    E_COMPILE_WARNING   => FALSE,
    E_USER_ERROR        => TRUE,
    E_USER_WARNING      => FALSE,
    E_USER_NOTICE       => FALSE,
    E_STRICT            => FALSE,
    E_RECOVERABLE_ERROR => TRUE,
    E_DEPRECATED        => FALSE,
    E_USER_DEPRECATED   => FALSE,
    E_ALL               => FALSE,
];
error_reporting(
    array_sum(
        array_keys($errorsActive, $search = true)
    )
);
```
## Import file into database
mysql -u <username> -p <databasename> < <filename.sql>

## remove_all_transients.sql file removing all transients in the database

## .htaccess file for bedrock application in subfolder
```sh
RewriteEngine on
RewriteCond %{HTTP_HOST} ^example.com$ [NC,OR]
RewriteCond %{HTTP_HOST} ^example.com$
RewriteCond %{REQUEST_URI} !web/
RewriteRule (.*) /web/$1 [L]
RewriteEngine On
```
## .htaccess redirect from www
```sh
RewriteCond %{HTTP_HOST} ^www.example.com [NC]
RewriteRule ^(.*)$ https://example.com/$1 [L,R=301]
```

## .htaccess file for bedrock application if website root folder can not be changed
```sh
# BEGIN WordPress Bedrock
# The directives (lines) between "BEGIN WordPress Bedrock" and "END WordPress Bedrock" are
# dynamically generated, and should only be modified via WordPress filters.
# Any changes to the directives between these markers will be overwritten.
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule \/composer.json$ - [R=404]
RewriteRule \/composer.local$ - [R=404]
RewriteRule \/phpcs.xml$ - [R=404]
RewriteRule ^wp/(.*) /web/wp/$1 [L]
RewriteRule ^app/(.*) /web/app/$1 [L]
RewriteRule . /index.php [L]
</IfModule>
<FilesMatch "\.(htaccess|htpasswd|ini|log|sh|inc|bak|env|example|md|yml|gitignore)$">
Order Allow,Deny
Deny from all
</FilesMatch>
<files "/config">
  order allow,deny
  deny from all
</files>
<files "/vendor">
  order allow,deny
  deny from all
</files>
<files "/.git">
  order allow,deny
  deny from all
</files>
# END WordPress Bedrock
```

## define website URL
```php

define( 'WP_HOME', 'example_url' );
define( 'WP_SITEURL', 'example_url' );

```
### git init helper for define existed repo

```sh
git init
git remote add origin git@gitlab.com:exampleprofile/example-repo.git
git add .
git commit -m "first commit"
git branch -M main
git push --set-upstream origin main
```

# Search and reaplce db:
```sh
git clone https://github.com/interconnectit/Search-Replace-DB.git sar && cd sar && rm -fr ./.git
php srdb.cli.php -h dbhost -n dbname -u root -p "" -s "search" -r "replace"
```

# Search and delete .DS_Store files:
```sh
find . -name '.DS_Store' -type f -delete
```
# Search for any file that have php format
```sh
find . -type f -name "*.php"
```

# Reset chmod
```sh
find -type d -exec chmod 775 {} ';'
find -type f -exec chmod 664 {} ';'
```

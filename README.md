# Wordpress-Auto-Detect-Site-Urls

I was getting really frustrated on how i had to constantly change the URLs in the database every time I was switching environments and migrating dbs for wordpress sites.

So I decided to write the below code to the wp-config.php file, which adds the ability to dynamicly switch the urls at runtime.

It also contains code showing how you can switch dbs based on different host names.

```php

$port = ':'.$_SERVER["SERVER_PORT"];

if (!empty($_SERVER['HTTPS']) && (strtolower($_SERVER['HTTPS']) == 'on' || $_SERVER['HTTPS'] == '1')) {
	$protocol = 'https';
} else {
	$protocol = 'http';
}

if($port == ':80' || $protocol == 'https')
	$port = '';

$url = $protocol.'://'.$serverName.$port;

// Tells wordpress to use the auto detected urls instead of the ones in the db
define( 'WP_HOME', $url );
define( 'WP_SITEURL', $url );

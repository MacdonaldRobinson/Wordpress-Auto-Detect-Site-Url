# Wordpress-Auto-Detect-Site-Urls

I was getting really frustrated on how i had to constantly change the URLs in the database every time I was switching environments and migrating dbs for wordpress sites.

So I decided to write the below code which adds the ability to dynamicly switch the urls at runtime.

It also contains code showing how you can switch dbs based on different host names.

```php

$port = ':'.$_SERVER["SERVER_PORT"];

if (isset($_SERVER) && array_key_exists('SERVER_NAME', $_SERVER)) {
  $serverName = $_SERVER['SERVER_NAME'];
} else {
  global $_FILE_TO_URL_MAPPING;
  $basePath = dirname(SOURCE_PATH);
  $urlInfo = parse_url($_FILE_TO_URL_MAPPING[$basePath]);
  $serverName = $urlInfo['host'];
}

global $databaseConfig;

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

switch ($serverName) {
	// LIVE SITE SETTINGS
	case 'site.com':
	case 'www.site.com':	
	//case 'localhost':
		define('DB_NAME', '[DB_NAME]');
		define('DB_USER', '[DB_USER]');
		define('DB_PASSWORD', '[DB_PASSWORD]');
		define('DB_HOST', '[DB_HOST]');		
		define('DB_CHARSET', 'utf8');
		define('DB_COLLATE', '');
		define('WP_DEBUG', false);
	break;
	// LOCAL/DEV SITE SETTINGS
	default:
		define('DB_NAME', '[DB_NAME]');
		define('DB_USER', '[DB_USER]');
		define('DB_PASSWORD', '[DB_PASSWORD]');
		define('DB_HOST', '[DB_HOST]');		
		define('DB_CHARSET', 'utf8');
		define('DB_COLLATE', '');
		define('WP_DEBUG', true);
	break;
}

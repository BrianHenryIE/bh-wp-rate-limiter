# BH WP Rate Limiter

A rate limiter for WordPress, built on [nikolaposa/rate-limit](https://github.com/nikolaposa/rate-limit). It stores counts in WordPress transients, so an object cache plugin takes over the storage when one is installed. The transients are accessed through [wp-oop/transient-cache](https://github.com/wp-oop/transient-cache).

## Install

```
composer require wp-oop/transient-cache:@alpha brianhenryie/bh-wp-rate-limiter
```

You need `wp-oop/transient-cache:@alpha` because that package has only alpha releases. Composer ignores stability flags set inside dependencies, so your project has to allow alpha itself.

## Usage

```php
use BrianHenryIE\WP_Rate_Limiter\WordPress_Rate_Limiter;
use RateLimit\Exception\LimitExceeded;
use RateLimit\Rate;

$rate_limiter = new WordPress_Rate_Limiter( Rate::perMinute( 5 ), 'my-plugin' );

try {
	$rate_limiter->limit( $ip_address );
} catch ( LimitExceeded $e ) {
	// Too many requests.
}
```

If you'd rather check the status than catch an exception, use `limitSilently()`. It returns a `RateLimit\Status`.

Any of the characters `{}()/\@:` in a key (IPv6 addresses, for example) are replaced with `-`, because wp-oop/transient-cache reserves them.

For more, see the [nikolaposa/rate-limit documentation](https://github.com/nikolaposa/rate-limit).

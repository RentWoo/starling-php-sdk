# Starling PHP SDK

[![CircleCI](https://circleci.com/gh/MoneyMeg/starling-php-sdk.svg?style=svg&circle-token=e03ccecf3832ac3bc62bd569646d59ed610c2fb9)](https://circleci.com/gh/MoneyMeg/starling-php-sdk) [![StyleCI](https://styleci.io/repos/111804992/shield?branch=master)](https://styleci.io/repos/111804992) <a href="https://codeclimate.com/github/MoneyMeg/starling-php-sdk/maintainability"><img src="https://api.codeclimate.com/v1/badges/01623f9ecbd68a54a600/maintainability" /></a>

This is an SDK for the starling SDK, you'll have to take care of getting tokens from users but once you have one this SDK will wrap the whole API to allow for ease of use in PHP.

Right now its only read functions, I'll some write functions when I get around to using them.

See the tests folder for all the requests, you can pass any request to the request function on `Starling\Api\Client` if you want to write your own they'll need to implements `RequestInterface`

I didn't want to be too opinionated so responses are not formatted.

## Install
```
composer require rentwoo/starling-php-sdk
```

## Test
```
$ ./vendor/bin/phpunit
```

## Get a users balance
```php
$identity = new Starling\Identity($client_token);
$client = new Starling\Api\Client($identity, ['env' => 'prod']);
$request = new Starling\Api\Request\Accounts\Balance();

try {
    $result = $client->request($request);
    $body = json_decode((string) $result->getBody(), true);
    print "Your balance is " . $body['effectiveBalance'] . " as for right now.";
} catch (Exception $e) {
    print $e->getMessage();
}
```

## Get a users account details
```php
$identity = new Starling\Identity($client_token);
$client = new Starling\Api\Client($identity, ['env' => 'prod']);
$request = new Starling\Api\Request\Accounts();

try {
    $result = $client->request($request);
    $body = json_decode((string) $result->getBody(), true);
    print $body;
} catch (Exception $e) {
    print $e->getMessage();
}
```

## Gets a customers Direct Debits
```php
$identity = new Starling\Identity($client_token);
$client = new Starling\Api\Client($identity, ['env' => 'prod']);
$request = new Starling\Api\Request\DirectDebits();

try {
    $result = $client->request($request);
    $body = json_decode((string) $result->getBody(), true);
    print $body;
} catch (Exception $e) {
    print $e->getMessage();
}
```

## Get transactions for the last 7 days
```php
$identity = new Starling\Identity($client_token);
$client = new Starling\Api\Client($identity);

$request = new Starling\Api\Request\Transactions([
    'from' => new DateTime("-7 Days"),
    'to'   => new DateTime(),
]);

try {
    $result = $client->request($request);
    $body = json_decode((string) $result->getBody(), true);
    print $body;
} catch (Exception $e) {
    print $e->getMessage();
}
```

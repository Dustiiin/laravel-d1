# Connects Cloudflares D1 Databases with Laravel

[![Latest Stable Version](http://poser.pugx.org/dustiiin/laravel-d1/v)](https://packagist.org/packages/dustiiin/laravel-d1) [![Total Downloads](http://poser.pugx.org/dustiiin/laravel-d1/downloads)](https://packagist.org/packages/dustiiin/laravel-d1) [![Latest Unstable Version](http://poser.pugx.org/dustiiin/laravel-d1/v/unstable)](https://packagist.org/packages/dustiiin/laravel-d1) [![License](http://poser.pugx.org/dustiiin/laravel-d1/license)](https://packagist.org/packages/dustiiin/laravel-d1) [![PHP Version Require](http://poser.pugx.org/dustiiin/laravel-d1/require/php)](https://packagist.org/packages/dustiiin/laravel-d1)

Extend your PHP/Laravel application with Cloudflare bindings.

This package offers support for:

- [x] [Cloudflare D1](https://developers.cloudflare.com/d1)
- [ ] [Cloudflare KV](https://developers.cloudflare.com/kv/)
- [ ] [Cloudflare Queues](https://developers.cloudflare.com/queues)

## 🚀 Installation

You can install the package via Composer:

```bash
composer require dustiiin/laravel-d1
```

## 🙌 Usage

### D1 with raw PDO

Though D1 is not connectable via SQL protocols, it can be used as a PDO driver via the package connector. This proxies the query and bindings to the D1's `/query` endpoint in the Cloudflare API.

```php
use dustiiin\db1\D1\D1Pdo;
use dustiiin\db1\D1\D1PdoStatement;
use dustiiin\db1\CloudflareD1Connector;

$pdo = new D1Pdo(
    dsn: 'sqlite::memory:', // irrelevant
    connector: new CloudflareD1Connector(
        database: 'your_database_id',
        token: 'your_api_token',
        accountId: 'your_cf_account_id',
    ),
);
```

### D1 with Laravel

In your `config/database.php` file, add a new connection:

```php
'connections' => [
    'd1' => [
        'driver' => 'd1',
        'prefix' => '',
        'database' => env('CLOUDFLARE_D1_DATABASE_ID', ''),
        'api' => 'https://api.cloudflare.com/client/v4',
        'auth' => [
            'token' => env('CLOUDFLARE_TOKEN', ''),
            'account_id' => env('CLOUDFLARE_ACCOUNT_ID', ''),
        ],
    ],
]
```

Then in your `.env` file, set up your Cloudflare credentials:

```
CLOUDFLARE_TOKEN=
CLOUDFLARE_ACCOUNT_ID=
CLOUDFLARE_D1_DATABASE_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

The `d1` driver will proxy the PDO queries to the Cloudflare D1 API to run queries.

## 🐛 Testing

Start the built-in Worker that simulates the Cloudflare API:

```bash
cd tests/worker
npm ci
npm run start
```

In a separate terminal, run the tests:

``` bash
vendor/bin/phpunit
```

## 🤝 Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## 🔒  Security

If you discover any security related issues, please email <d.mueller@dustiiin.de> instead of using the issue tracker.

## 🎉 Credits

- [Dustin Müller](https://github.com/dustiiin)
- [Inter epcm Team](https://github.com/interepcm)
- [All Contributors](../../contributors)

# HubSpot PHP API Client Wrapper for Laravel

[![Latest Stable Version](https://poser.pugx.org/rossjcooper/laravel-hubspot/v/stable)](https://packagist.org/packages/rossjcooper/laravel-hubspot) [![Total Downloads](https://poser.pugx.org/rossjcooper/laravel-hubspot/downloads)](https://packagist.org/packages/rossjcooper/laravel-hubspot)

This is a wrapper for the [Hubspot/hubspot-api-php](https://github.com/HubSpot/hubspot-api-php) package and gives the user a Service Container binding and facade of the `HubSpot\Discovery\Discovery` class.

## Installation
1. `composer require rossjcooper/laravel-hubspot`
2. Get a HubSpot API Key from the Intergrations page of your HubSpot account.
3. Laravel 5.4 or earlier, in your `config/app.php` file:
    - Add `Rossjcooper\LaravelHubSpot\HubSpotServiceProvider::class` to your providers array.
    - Add `'HubSpot' => Rossjcooper\LaravelHubSpot\Facades\HubSpot::class` to your aliases array.
4. `php artisan vendor:publish --provider="Rossjcooper\LaravelHubSpot\HubSpotServiceProvider" --tag="config"` will create a `config/hubspot.php` file.
5. Add your HubSpot API key and private app access token into the `.env` file: `HUBSPOT_ACCESS_TOKEN=yourApiKey`
6. If you use the private app access token, you should alo add `HUBSPOT_USE_OAUTH2=true` to your `.env` file

## Usage
You can use either the facade or inject the HubSpot class as a dependency:
### Facade
```php
// Echo all contacts first and last names
$response = HubSpot::crm()->contacts()->basicApi()->getPage();
    foreach ($response->getResults() as $contact) {
        echo sprintf(
            "Contact name is %s %s." . PHP_EOL,
            $contact->getProperties()['firstname'],
            $contact->getProperties()['lastname']
        );
    }
```
```php
Route::get('/', function (HubSpot\Discovery\Discovery $hubspot) {
    $response = $hubspot->crm()->contacts()->basicApi()->getPage();
    foreach ($response->getResults() as $contact) {
        echo sprintf(
            "Contact name is %s %s." . PHP_EOL,
            $contact->getProperties()['firstname'],
            $contact->getProperties()['lastname']
        );
    }
});
```

```php
// Create a new contact
$contactInput = new \HubSpot\Client\Crm\Contacts\Model\SimplePublicObjectInputForCreate();
$contactInput->setProperties([
    'email' => 'example@example.com'
]);

$contact = $hubspot->crm()->contacts()->basicApi()->create($contactInput);
```


For more info on using the actual API see the main repo [Hubspot/hubspot-api-php](https://github.com/HubSpot/hubspot-api-php)

## Testing

We're using the brilliant [Orchestra Testbench](https://github.com/orchestral/testbench) to run unit tests in a Laravel based environment. If you wish to run tests be sure to have a HubSpot API key inside your `.env` file and run `composer run test`

Current unit test access the HubSpot API and expect to see the demo contacts/leads that HubSpot provides to its developer accounts.

## Issues
Please only report issues relating to the Laravel side of things here, main API issues should be reported [here](https://github.com/HubSpot/hubspot-api-php/issues)

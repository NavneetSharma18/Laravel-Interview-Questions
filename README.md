# Laravel-Interview-Questions

## Request Life Cycle
The entry point for all requests to a Laravel application is the public/index.php file. All requests are directed to this file by your web server (Apache / Nginx) configuration. 
The index.php file doesn't contain much code. Rather, it is a starting point for loading the rest of the framework. 
Next, the incoming request is sent to either the **HTTP kernel** or the console kernel, using the handleRequest or handleCommand methods of the application instance

## Service Container
Think of the Service Container as a big box (or toolbox) in Laravel where you can store things (services, classes, helpers) and later ask Laravel to give them back to you whenever you need them.
It’s mainly used for Dependency Injection → instead of you creating objects manually (new Something()), you let Laravel give them to you from the box

```php
namespace App\Http\Controllers;

use App\Services\PaymentGateway;

class OrderController extends Controller
{
    protected $payment;

    public function __construct(PaymentGateway $payment)
    {
        $this->payment = $payment; // Laravel auto-fills it from the container
    }

    public function checkout()
    {
        return $this->payment->charge(1000);
    }
}

```
##### 👉 Here, Laravel sees that OrderController needs a PaymentGateway, so it looks inside the Service Container and provides it. You don’t write new PaymentGateway().

## Service Providers (Service provider ko bind krna padta hai service container me and resgister krna padta hai config.ppp inside array )
Think of a service provider as a “starter/initializer” for different features in Laravel.

Database ✔️
Queue ✔️
Validation ✔️
Routing ✔️

All these features don’t magically appear — they are bootstrapped (started up) by service providers
Service providers are like the “startup scripts” of Laravel. They prepare and launch all the features your app needs. First register() prepares things, then boot() starts them.

**register() method →** here the provider registers things in the service container (bindings, configs, etc.).

**boot() method →** here things are finalized, because now all services are available.

##### php artisan make:provider CustomServiceProvider

## Facades


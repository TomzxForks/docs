---
title: Identification
icon: fal fa-street-view
---

At runtime, the `Environment` class is bound into Laravel's container. The
tenancy environment holds one single responsibility, which is telling the code
what hostname was identified and resolving the related website and customer.

# Early identification

By default tenancy will identify the tenant after all middlewares have been processed.
Previously, we could simply resolve the `Environment` class through the container
to force identification of the tenant early on. If you want the package to handle
this on your behalf you can simply enable the `early-identification` flag in
the `tenancy.php` configuration file under `hostname`.

# Custom identification

The environment can be used to manually set any active tenant. This is one of the
reasons why the tenancy configuration file has the `hostname > auto-identification`
setting. Turn it off to implement your own tenancy identification.

Custom identification is pretty straightforward. Take for example the following
snippet for a custom (app) service provider:

```php
public function boot()
{
    $environment = $this->app->make(\Hyn\Tenancy\Environment::class);

    // Retrieve your hostname

    // Now switch the environment to a new tenant.
    $environment->hostname($hostname);
}
```

> Be wary about switching tenants. It is recommended to switch only once during code
execution. Use a queued job to run mass changes on tenant databases.

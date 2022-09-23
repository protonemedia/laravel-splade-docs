# Upgrading

Splade consists of two packages, a PHP one and a JavaScript one. When you upgrade Splade to the latest version, make sure you update both packages. They should be on the same version. For example, when using version `0.5.0` of the PHP package, you should use the same JavaScript package version.

```bash
composer update protonemedia/laravel-splade

npm upgrade @protonemedia/laravel-splade
```

Make sure to clear Laravel's [view cache](https://laravel.com/docs/9.x/views#optimizing-views) after upgrading:

```
php artisan view:clear
```

Lastly, if you've published the config file or the Blade templates, make sure your customizations are up-to-date with the defaults.
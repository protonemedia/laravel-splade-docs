# Customization

You can find every bit of styling in Blade templates, so you never have to customize Vue components. You may publish the Blade templates using the `vendor:publish` Artisan command:

```bash
php artisan vendor:publish --provider="ProtoneMedia\Splade\ServiceProvider" --tag="views"
```

Now the templates are available in the `/resources/views/vendor/splade` folder. Only the styled templates are published. The strictly functional components (those without any markup or styling) will not be published, as there's nothing to customize.

## Laravel Configuration File (config/splade.php)

You may also publish the configuration file. This allows you to configure settings like the Blade Component prefix, SEO defaults, SSR settings, and more.

```bash
php artisan vendor:publish --provider="ProtoneMedia\Splade\ServiceProvider" --tag="config"
```
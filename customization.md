# Customization

Every piece of styling is located in Blade templates, so you never have to customize Vue components. You may publish the Blade templates using the `vendor:publish` Artisan command:

```bash
php artisan vendor:publish --provider="ProtoneMedia\Splade\ServiceProvider"
```

Now the templates are available in the `/resources/views/vendor/splade` folder.
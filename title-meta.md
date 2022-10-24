# Title and meta tags

You may use the `SEO` facade to set your page's title, description, and keywords.

```php
use ProtoneMedia\Splade\Facades\SEO;

class CourseController
{
    public function show()
    {
        SEO::title('Laravel Splade Course')
            ->description('Become the Splade expert!')
            ->keywords('laravel, splade, course');

        return view('course.show');
    }
}
```

The `keywords` method accepts an array as well:

```php
SEO::keywords(['laravel', 'splade', 'course']);
```

You may publish and customize the `splade-seo.php` configuration file to set default values. In previous versions of Splade, the SEO configuration was part of the `splade.php` configuration file. As of v0.7, there is a separate file for the SEO configuration.

```bash
php artisan vendor:publish --provider="ProtoneMedia\Splade\ServiceProvider" --tag="seo"
```

```php
return [
    'defaults' => [
        'title' => 'My application',

        'description' => 'This is the description of my application.',

        'keywords' => ['application', 'keywords'],
    ],
];
```

You may also set a default prefix, suffix and separator for the title. With the example below, if you call `SEO::setTitle('Home')`, the final title will become `Home | My app`.

```php
return [
    'title_separator' => '|',

    'title_suffix' => 'My app',
];
```

## Canonical URL

By default, the canonical URL is set to the current URL, but you may override it with the `canonical` method:

```php
SEO::canonical('https://splade.dev');
```

If you don't want to automatically set the canonical URL to the current URL, you may disable this in the `splade-seo.php` config file by setting the `auto_canonical_link` key to `false`.

## Open Graph tags

There are five helper methods to set Open Graph tags:

```php
SEO::openGraphType('WebPage');
SEO::openGraphSiteName('My Application');
SEO::openGraphTitle('Home Page');
SEO::openGraphUrl('https://my.app/home');
SEO::openGraphImage(public_path('home.png'));
```

In the `splade-seo.php` configuration file you may set default values, and there's also an `auto_fill` option. When you set this option to `true`, the value passed to the `SEO::title()` method will also be applied to the Open Graph Title tag.

## Twitter tags

There are five helper methods to set Twitter tags:

```php
SEO::twitterCard('summary_large_image');
SEO::twitterSite('@pascalbaljet');
SEO::twitterTitle('Home Page | My App');
SEO::twitterDescription('This is the home page of my application');
SEO::twitterImage(public_path('home.png'));
```

In the `splade-seo.php` configuration file you may set default values, and there's also an `auto_fill` option. When you set this option to `true`, values passed to `SEO::title()` and `SEO::description()` method will also be applied to the corresponding Twitter tags.

## Custom Meta Tags

There are three additional methods to set other meta tags. You can set meta tags by name, property, or by giving a custom set of attributes.

```php
SEO::metaByName('theme-color', '#ffffff');

SEO::metaByProperty('article:section', 'news');

SEO::meta([
    'custom-attribute' => 'example'
]);
```

## SEO Macro

The underlying class of the `SEO` facade uses Laravel's `Macroable` trait, so it's easy to add a method:

```php
SEO::macro('openGraphLocale', function (string $value) {
    return $this->metaByProperty('og:locale', $value);
});
```

Make sure to return from the callback so you may use the facade fluently:

```php
SEO::title('Laravel Splade Cursus')
    ->openGraphLocale('nl')
    ->description('Wil je een Splade expert worden?');
```
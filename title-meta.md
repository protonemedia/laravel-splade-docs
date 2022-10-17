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

In the `splade.php` [configuration file](/customization.md), you may set the default content:

```php
return [
    ...

    'seo' => [
        'defaults' => [
            'title' => 'My application',

            'description' => 'This is the description of my application.',

            'keywords' => ['application', 'keywords'],
        ],
    ],

    ...
];
```

You may also set a default prefix/suffix for the title. With the example below, if you call `SEO::setTitle('Home')`, the final title will become `Page: Home | My app`.

```php
return [
    ...

    'seo' => [
        'title_prefix' => 'Page:',

        'title_suffix' => '| My app',
    ]

    ```
];
```

There are three additional methods to set other meta tags. You can set meta tags by name, property, or by giving a custom set of attributes. Future releases of Splade will include additional helper methods to set OpenGraph and other SEO tags.

```php
SEO::metaByName('twitter:site', '@pascalbaljet');

SEO::metaByProperty('article:section', 'news');

SEO::meta([
    'custom-attribute' => 'example'
]);
```
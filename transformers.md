# Transformers

In components such as [Data](/x-data.md) and [Form](/x-form.md), you may want to pass PHP objects to the frontend. The most common example is an Eloquent Model. Though you may [hide sensitive attributes](https://laravel.com/docs/10.x/eloquent-serialization#hiding-attributes-from-json) from being passed to the frontend, and though the Form Components have [built-in support](/form-model-binding-attributes.md) to guard Model attributes, you may also choose to use a Transformer.

Splade makes it very easy to define a Transformer once, and use it throughout the application in the Data, Defer, Form, and Bridge Components. It supports [Eloquent API Resources](https://laravel.com/docs/10.x/eloquent-resources), [Fractal Transformers](https://fractal.thephpleague.com), and a simple closure.


## Transform using an Eloquent Resource Class

Configuring a Transformer is as easy as calling the `transformUsing` method on the `Splade` facade, for example, in the `AppServiceProvider` class:

```php
Splade::transformUsing(User::class, UserResource::class);
```

### Pass multiple transformers

Instead of calling the `transformUsing` methods multiple times, you may also pass an array:

```php
Splade::transformUsing([
    Team::class => TeamResource::class,
    User::class => UserResource::class,
]);
```

### Fallback and Enforcement

When no Transformer has been configured, Splade won't transform the given data and passes it untouched. If you want to enforce using Transformers, and protect against missing Transformers, you may call the `requireTransformer` method on the `Splade` facade:

```php
Splade::requireTransformer();
```

## Transform using a Fractal Transformer

To use Fractal Transformers, you need to install the [`spatie/fractalistic`](https://github.com/spatie/fractalistic) package.

```php
Splade::transformUsing(User::class, UserTransformer::class);
```

## Transform using a Closure

For simple transformations, you may pass a closure:

```php
Splade::transformUsing(User::class, function ($user) {
    return [
        'name'  => $user->name,
        'email' => $user->email,
    ];
});
```
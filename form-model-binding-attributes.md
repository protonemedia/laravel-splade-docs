# Model Binding Attributes

When using Form Components, only the attributes used in the form are passed to the frontend. This applies to `Fluent` and `Model` instances that you pass to the form. You may disable this behavior by using the `unguarded` attribute on the form.

```blade
<x-splade-form :default="$user" unguarded>
    <x-splade-input name="email" placeholder="Email address" />
</x-splade-form>
```

Instead of fully unguarding the form, you may specify which attributes to unguard. You can do this with an array or string.

```blade
<x-splade-form ... unguarded="name" />

<x-splade-form ... unguarded="name, email" />

<x-splade-form ... :unguarded="['name', 'email']" />
```

If you want forms to be unguarded by default, you may use the static `defaultUnguarded` on the Form class, for example, in the `AppServiceProvider` class:

```php
use ProtoneMedia\Splade\Components\Form;

Form::defaultUnguarded();
```

Alternatively, you may specify when a form should be guarded based on the bound resource.

```php
use Illuminate\Database\Eloquent\Model;
use ProtoneMedia\Splade\Components\Form;

Form::guardWhen(function ($resource) {
    return $resource instanceof Model;
});
```

Also, check out the [Transformers](/transformers.md) section.
# Form Components

Splade comes with a set of **Form Components** to rapidly build forms. It supports model binding and validation, includes default styling, and is still fully customizable! It integrates with [Autosize](https://www.jacklmoore.com/autosize/) to automatically adjust textarea height, [Choices.js](https://github.com/Choices-js/Choices) to make selects searchable and taggable, [Flatpickr](https://flatpickr.js.org) to provide a powerful datetime picker, and [FilePond](https://pqina.nl/filepond/) for smooth file uploads.

Available components:

* [Checkbox](/form-checkbox.md)
* [File](/form-file.md)
* [Group](/form-group.md)
* [Input](/form-input.md)
* [Radio](/form-radio.md)
* [Select](/form-select.md)
* [Submit](/form-submit.md)
* [Textarea](/form-textarea.md)

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/-utQsyibvZM?controls=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Labels

All elements allow passing in a label, which will be rendered above the input element. Just like other attributes, the label can also be computed.

```blade
<x-splade-form>
    <x-splade-input name="email" label="Email address" />

    <x-splade-input name="username" :label="__('Username')" />

    <x-splade-submit />
</x-splade-form>
```

If you want to eliminate the `splade` prefix, you may update the `blade.component_prefix` key in the `splade.php` [configuration file](/customization.md):

```php
return [
    ...

    'blade' => [

        'component_prefix' => '',

    ],

    ...
];
```

This will result in more readable `<x-form>`, `<x-input>`, and `<x-submit>` components.

## Model Binding

To bind a resource to a form, for example, an [Eloquent Model](https://laravel.com/docs/10.x/eloquent), you may use the `default` attribute. Instead of using the `v-model` attribute on the input element, you may now use the `name` attribute. Splade will take care of the two-way binding with the User model.

```blade
<x-splade-form :default="$user">
    <x-splade-input name="email" placeholder="Email address" />
</x-splade-form>
```

Note that the user data is passed to the frontend, so be careful with sensitive attributes. By default, it only passes the attributes to the frontend that you use. So in this example, only the `email` attribute will be passed to the frontend. To customize this behavior, check out the [Model Binding Attributes](/form-model-binding-attributes.md) section.

## Validation Errors

The validation errors are populated automatically based on the `name` attribute. This behavior is enabled by default for all elements except `radio` elements, as you don't want to repeat the error for each element. In that case, you may use a `<x-splade-group>` component, which you can read more about on [its page](/form-group.md).

To disable validation errors on an element, you may use the `show-errors` attribute:

```blade
<x-splade-input name="email" :show-errors="false" />
```

If you want to override how Splade will resolve the validation, you may use the `validation-key` attribute:

```blade
<x-splade-input name="email" validation-key="email_address" />
```

By default, Splade scrolls to the first element with a validation error. You may disable this with the `scroll-on-error` attribute:

```blade
<x-splade-form :scroll-on-error="false">
    ...
</x-splade-form>
```

Splade escapes Validation errors by default, so if you use raw HTML in one of your messages, Vue won't insert those as plain HTML. If you want to disable this behavior, set the `blade.escape_validation_messages` key in the `splade.php` [configuration file](/customization.md) to `false`. Only disable this on trusted content and never on user-provided content, as it may lead to XSS attacks.

## Form Builder

In addition to building forms in Blade templates, you may also build forms in PHP with the [Form Builder](./form-builder-overview.md).
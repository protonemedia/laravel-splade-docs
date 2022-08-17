# Form Components

Splade comes with a set of **Form Components** to rapidly build forms. It supports model binding and validation, includes default styling, and is still fully customizable! It integrates with [Autosize](https://www.jacklmoore.com/autosize/) to automatically adjust textarea height, [Choices.js](https://github.com/Choices-js/Choices) to make selects searchable and taggable, and [Flatpickr](https://flatpickr.js.org) to provide a powerful datetime picker.

Available components:

* [Input](/form-input.md)
* [Textarea](/form-textarea.md)
* [Select](/form-select.md)
* [Checkbox](/form-checkbox.md)
* [Radio](/form-radio.md)
* [File](/form-file.md)
* [Group](/form-group.md)
* [Submit](/form-submit.md)

## Model Binding

To bind a resource to a form, for example, an [Eloquent Model](https://laravel.com/docs/9.x/eloquent), you may use the `default` attribute. Instead of using the `v-model` attribute on the input element, you may now use the `name` attribute. Splade will take care of the two-way binding with the User model.

```blade
<x-splade-form :default="$user">
    <x-splade-input name="email" placeholder="Email address" />
</x-splade-form>
```

Note that the user data is passed to the frontend, so please be sure sensitive attributes [hidden](https://laravel.com/docs/9.x/eloquent-serialization#hiding-attributes-from-json).

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
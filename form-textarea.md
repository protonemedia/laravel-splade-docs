# X-Splade-Textarea Component

The **Textarea Component** is based mainly on the [Input component](/form-input.md) and provides an integration with the [autosize](https://www.jacklmoore.com/autosize/) library. This is as simple as adding the `autosize` attribute to the element:

```blade
<x-splade-textarea name="biography" autosize />
```

If you want to enable autosize on every textarea element, you may use the static `defaultAutosize` on the `Textarea` class, for example, in the `AppServiceProvider` class:

```php
use ProtoneMedia\Splade\Components\Form\Textarea;

Textarea::defaultAutosize();
```
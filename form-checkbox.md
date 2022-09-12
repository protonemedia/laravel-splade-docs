# X-Splade-Checkbox Component

The **Checkbox Component** has a default value of `1`, but you may customize it with the `value` attribute:

```blade
<x-splade-checkbox name="newsletter" value="mailchimp" label="Subscribe to newsletter" />
```

If you have a fieldset of multiple checkboxes, you can group them with the `x-splade-group` component. A group is a great way to handle the validation of arrays. If you disable the errors on the individual checkboxes, it will show the validation errors once. The [group component](/form-group.md) has a `show-errors` attribute that defaults to `true`.

```blade
<x-splade-group name="tags" label="Pick one or more interests">
    <x-splade-checkbox name="tags[]" :show-errors="false" value="laravel" label="Laravel" />
    <x-splade-checkbox name="tags[]" :show-errors="false" value="tailwindcss" label="Tailwind" />
</x-splade-group>
```

## X-Splade-Checkboxes Component

You can shorten and rewrite the example above with the `x-splade-checkboxes` component that automatically renders the group and checkboxes based on a *key-value* array. This component behaves similarly to the [select component](/form-select.md).

```blade
<x-splade-checkboxes name="tags" label="Pick one or more interests" :options="$tags" />
```

## Eloquent Relationships

Just like the [select component](/form-select.md), there's built-in support for `BelongsToMany` and `MorphToMany` relationships. To utilize this feature, you must add the `relation` attributes to the `checkbox` or `checkboxes` element.

In the example below, you can attach one or more tags to the video. Using the `relation` attribute will correctly retrieve the selected options (attached tags) from the database.

```blade
<x-splade-form :default="$video">
    <x-splade-checkboxes name="tags" label="Tags" :options="$tags" relation />
</x-splade-form>
```
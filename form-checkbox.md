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
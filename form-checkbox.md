# X-Splade-Checkbox Component

The **Checkbox Component** has a default value of `1`, but you may customize it with the `value` attribute:

```blade
<x-splade-checkbox name="newsletter" value="mailchimp" label="Subscribe to newsletter" />
```

If you have a fieldset of multiple checkboxes, you can group them together with the `x-splade-group` component. This component has an optional `label` attribute and you can set the `name` as well. This is a great way to handle the validation of arrays. If you disable the errors on the individual checkboxes, it will one show the validation errors once. The  component has a `show-errors` attribute that defaults to `true`.

```blade
<x-splade-group name="interests" label="Pick one or more interests">
    <x-splade-checkbox name="interests[]" :show-errors="false" value="laravel" label="Laravel" />
    <x-splade-checkbox name="interests[]" :show-errors="false" value="tailwindcss" label="Tailwind CSS" />
</x-splade-group>
```
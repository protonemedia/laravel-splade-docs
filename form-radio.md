# X-Splade-Radio Component

The **Radio Component** behaves the same as [checkboxes](/form-checkbox.md), except the `show-errors` attribute defaults to `false` as you almost always want to wrap multiple radio elements in a `x-splade-group`.

You can [group](/form-group.md) checkbox and radio elements on the same horizontal row by adding an `inline` attribute.

```blade
<x-splade-group name="notification_channel" label="Preferred notification channel" inline>
    <x-splade-radio name="notification_channel" value="mail" label="Mail" />
    <x-splade-radio name="notification_channel" value="slack" label="Slack" />
</x-splade-group>
```

If you still want to show validation errors on the Radio component, you can use the `show-errors` attribute:

```blade
<x-splade-radio name="theme" value="dark" label="Dark theme" :show-errors="true" />
<x-splade-radio name="theme" value="light" label="Light theme" :show-errors="true" />
```

## X-Splade-Radio*s* Component

You can shorten and rewrite the example above with the `x-splade-radios` component that automatically renders the group and radios based on a *key-value* array. This component behaves similarly to the [select component](/form-select.md).

```blade
<x-splade-radios name="notification_channel" label="Preferred notification channel" :options="$channels" />
```
# X-Splade-Radio Component

The **Radio Component** behaves exactly the same as [checkboxes](/form-checkbox.md), except the `show-errors` attribute defaults to `false` as you almost always want to wrap multiple radio elements in a `x-splade-group`.

You can group checkbox and radio elements on the same horizontal row by adding an inline attribute to the form-group element.

```blade
<x-form-group name="notification_channel" label="How do you want to receive your notifications?" inline>
    <x-form-radio name="notification_channel" value="mail" label="Mail" />
    <x-form-radio name="notification_channel" value="slack" label="Slack" />
</x-form-group>
```
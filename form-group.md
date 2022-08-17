# X-Splade-Group Component

With the **Group Component**, you can group [checkboxes](/form-checkbox.md) and [radio](/form-radio.md) elements. You can use the `inline` attribute to arrange the children horizontally. Like other components, the component also supports the `label` and `name` attributes.

You often don't want to show validation errors on each radio element, so the validation errors are hidden by default on the `<x-splade-radio>` component. Instead, you can use the Group component to show the error just once.

```blade
<x-splade-form :default="$settings">
    <x-splade-group label="Choose a theme" name="theme" inline>
        <x-splade-radio name="theme" value="dark" label="Dark theme" />
        <x-splade-radio name="theme" value="light" label="Light theme" />
    </x-splade-group>

    <x-splade-group>
        <x-splade-checkbox name="newsletter" label="Do you want to receive my newsletter?" />
        <x-splade-checkbox name="terms" label="Do you agree with the terms?" />
    </x-splade-group>
</x-splade-form>
```
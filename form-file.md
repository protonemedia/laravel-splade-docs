# X-Splade-File Component

The **File Component** is a dedicated component you can use to select files. This component is based mainly on the [Input component](/form-input.md) but provides additional logic to display the filename of the selected file. You don't have to manually use the input event to bind the selected files to the form. Splade does this for you.

```blade
<x-splade-form :default="$user">
    <x-splade-file name="avatar" />
</x-splade-form>
```

The component supports selecting multiple files as well by adding the `multiple` attribute.

```blade
<x-splade-form :default="['documents' => []]">
    <x-splade-file name="documents" multiple />
</x-splade-form>
```
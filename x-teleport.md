# X-Splade-Teleport Component

The **Teleport Component** allows you to *teleport* a section of a template into another DOM node. It works even outside the scope of a component.

You may pass a CSS selector to the `to` attribute. This will tell Splade to teleport the component's contents, the 'You are searching' part, to the bottom `div` element with the `footer` ID.

```blade
<x-splade-form>
    <x-splade-input name="search" />

    <x-splade-teleport to="#footer">
        You are searching for: <span v-text="form.search" />
    </x-splade-teleport>
</x-splade-form>

<div id="footer" class="p-4 bg-green-500 text-white" />
```
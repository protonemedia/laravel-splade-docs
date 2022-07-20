# X-Splade-Toggle Component

The **Toggle Component** is a simplified variant of the Data Component, primarily meant to toggle boolean values. It exposes `toggle`, `setToggle` and `toggled` properties:

```blade
<x-splade-toggle>
    <button @click.prevent="toggle">Show text</button>

    <div v-show="toggled">
        <p>...</p>
        <button @click.prevent="setToggle(false)">Hide text</button>
    </div>
</x-splade-toggle>
```

You may specify a default state with the `data` attribute:

```blade
<x-splade-toggle :data="true">
```

## Multiple toggles

Sometimes, you may want to toggle multiple values. Instead of using multiple components, you can specify all keys in the `data` attribute. This will slightly alter the exposed props. You can use the key directly, and the `toggle` and `setToggle` props now require a key:

```blade
<x-splade-toggle data="isCompany, hasVatNumber">
    <button @click.prevent="toggle('isCompany')">Switch account type</button>
    <input name="company_name" v-if="isCompany" />

    <button @click.prevent="setToggle('hasVatNumber', true)">Enable Vat Invoice</button>

    <div v-show="hasVatNumber">
        ...
    </div>
</x-splade-toggle>
```
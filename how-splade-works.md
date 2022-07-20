# How Splade works

Splade is super easy to use, but rather complicated under the hood. Let's break down the `<x-data>` component by using the example below:

```blade
<x-data :default="['name' => 'Laravel Splade']">
    <input v-model="data.name" />
</x-data>
```

First in chain, the Blade compiler will render the `x-data` component. It understand that the given data (the default array) is *jsonable*, so it passes the data as a JSON-encoded string to the `SpladeData` Vue component.

```vue
<SpladeData :default="@js($data)">
    <template #default="data">
        <input v-model="data.name" />
    </template>
</SpladeData>
```

The Vue component itself is renderless, it doesn't have a template or styling. It simply renders what's inside the componet (the slot).
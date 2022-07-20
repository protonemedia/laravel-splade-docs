# How Splade works

## Components

Splade is super easy to use, but rather complicated under the hood. Let's break down the `<x-data>` component by using the example below:

```blade
<x-data :default="['name' => 'Laravel Splade']">
    <input v-model="data.name" />
</x-data>
```

First in chain, the Blade compiler will render the `x-data` component. This Blade components is essentially a wrapper around a corresponding Vue component. It understand that the given data (the default array) is *jsonable*, so it passes the data as a JSON-encoded string to the `SpladeData` Vue component as a property.

```vue
<SpladeData :default="@js($data)">
    <template #default="data">
        <input v-model="data.name" />
    </template>
</SpladeData>
```

The Vue component itself is renderless, it doesn't have a template or styling. It simply renders what's inside the component (the slot). Everything you pass in the Blade component, we straightaway pass through to the Vue component. Although the Vue component doesn't have a template itself, the `template` tag allows you to interact with the component. For example, it exposes the `data` prop which allows for two-way data binding.

Here's another example, the `<x-defer>` component:

```blade
<x-defer url="https://api.com/quote/random">
    <p v-show="processing">Loading random quote...</p>
    <p v-if="response" v-text="response.data.quote" />
    <button @click.prevent="reload">New quote</button>
</x-defer>
```

Again, the `x-defer` Blade component is a wrapper around its corresponding Vue component:

```vue
<SpladeDefer url="https://api.com/quote/random">
    <template #default="{ processing, response, reload }">
        <p v-show="processing">Loading random quote...</p>
        <p v-if="response" v-text="response.data.quote" />
        <button @click.prevent="reload">New quote</button>
    </template>
</SpladeDefer>
```

So keep in mind: Blade is rendered first, Vue is rendered second. What you'll pass to the Blade component, will end up in the Vue component.
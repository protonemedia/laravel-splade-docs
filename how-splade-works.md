# How Splade works

## Components

Splade is super easy to use but rather complicated under the hood. Let's break down the `<x-data>` component by using the example below:

```blade
<x-data :default="['name' => 'Laravel Splade']">
    <input v-model="data.name" />
</x-data>
```

First, the Blade compiler will render the `x-data` component. This Blade component is essentially a wrapper around a corresponding Vue component. It understands that the given data (the default array) is *jsonable*, so it passes the data as a JSON-encoded string to the `SpladeData` Vue component as a property.

```blade
<SpladeData :default="@js($data)">
    <template #default="data">
        <input v-model="data.name" />
    </template>
</SpladeData>
```

The Vue component itself is renderless. It doesn't have a template or styling. It simply renders what's inside the component (the slot). Everything you pass in the Blade component, we straightaway pass through to the Vue component. Although the Vue component doesn't have a template itself, the `template` tag allows you to interact with the component. For example, it exposes the `data` prop, which provides two-way data binding.

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

Keep in mind that Laravel will render the Blade component first, and then there'll be a second round of rendering by Vue. So what you'll pass to the Blade component will end up in the Vue component.

## Routing and Rendering

Splade comes with a magical `<Render>` Vue component that you'll never have to interact with manually. Do you know Vue's `v-html` attribute? It can render HTML, but it doesn't interpret Vue components:

```vue
<script setup>
    const html = "<div class='w-full'>Content</div>";

    const template = "<VueComponent>Content</VueComponent>";
</script>

<template>
    <!-- this works -->
    <div v-html="html" />

    <!-- this doesn't -->
    <div v-html="template" />
</template>
```

The `Render` component allows you to pass in *other* Vue components, which Vue will render on-the-fly. This is why we can pass a fully rendered Blade view (having Vue components) to the `Render` component.

When requesting a route that returns a Blade view, Splade will grab the rendered Blade view and pass it to the `Render` component. When navigating a subsequent route, it will pass the freshly returned Blade view to the `Render` component, replacing the previous page.
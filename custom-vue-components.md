# Custom Vue components

Using custom Vue components in a Splade app works the same as any other Vue application. Imagine this Counter component, stored in the `resources/js/Components` folder as `Counter.vue`:

```vue
<template>
    <button @click="count++">{{ count }}</button>
</template>

<script setup>
import { ref } from "vue";

const count = ref(1);
</script>
```

In the main `app.js` file, you must import and register the component by passing both a name string and a component definition. If you're using [SSR](/ssr.md), make sure to import the component in `ssr.js` as well.

```js
import Counter from "./Components/Counter.vue";  // [tl! add]

createApp({
    render: renderSpladeApp({ el })
})
    .use(SpladePlugin, {
        "max_keep_alive": 10,
        "transform_anchors": false,
        "progress_bar": true
    })
    .component('Counter', Counter)   // [tl! add]
    .mount(el);
```

Instead of calling the `component` method for each component, you may also use the `components` key and pass an object:

```js
import Counter from "./Components/Counter.vue";  // [tl! add]

createApp({
    render: renderSpladeApp({ el })
})
    .use(SpladePlugin, {
        "max_keep_alive": 10,
        "transform_anchors": false,
        "progress_bar": true,
        "components": { // [tl! add]
            Counter, // [tl! add]
        },  // [tl! add]
    })
    .mount(el);
```

Now you may use the component in a Blade template:

```blade
<x-layout>
    <div class="text-2xl">
        <Counter />
    </div>
</x-layout>
```

## Passing data

If you need to pass data to a Vue component (as a property), you may use the `@js` directive:

```blade
<Cart :products="@js($products)" />
```

## Using Splade inside a Vue component

You may use the the global `$splade` variable to interact with the Splade core, for example, to visit another page:

```vue
<script>
export default {
    methods: {
        visitCheckout() {
            this.$splade.visit("/checkout");
        }
    },
};
</script>
```

If you're using the `setup` attribute on a `<script>` block, you may use [Vue's `inject` function](https://vuejs.org/guide/components/provide-inject.html#inject).

```vue
<script setup>
import { inject } from "vue";

const Splade = inject("$splade");

function visitCheckout() {
    Splade.visit("/checkout");
}
</script>
```
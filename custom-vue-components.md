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

## Renderless Vue component

Using renderless Vue components allows you to separate the template from the script. The built-in Splade components are built this way. This allows you to put all the logic in the Vue component and keep the template and styling in Blade.

Let's take the Counter example, extract the `increase` method and add a `render` method that exposes the `count` value and the `increase` method:

```vue
<script>
export default {
    data() {
        return {
            count: 1
        };
    },

    methods: {  // [tl! add]
        increase() {  // [tl! add]
            this.count++;  // [tl! add]
        }  // [tl! add]
    },  // [tl! add]

    render() {  // [tl! add]
        return this.$slots.default({  // [tl! add]
            count: this.count,  // [tl! add]
            increase: this.increase,  // [tl! add]
        });  // [tl! add]
    },  // [tl! add]
};
</script>
```

Now you can use the renderless Vue component in a Blade template, and use the `increase` method and `count` value.

```blade
<x-layout>
    <Counter v-slot="counter">
        <button @click="counter.increase" v-text="counter.count" />
    </Counter>
<x-layout>
```

Note that you can't use the `{{ counter.count }}` Vue syntax to echo out the count value, as the template is rendered first by the Blade engine, and Blade [uses the same syntax](https://laravel.com/docs/9.x/blade#blade-and-javascript-frameworks). You may use the `@` symbol to inform Blade the expression should remain untouched. This would result in `@{{ counter.count }}`. Though this works fine, using the `v-text` attribute seems neater.

You may also use the *destructuring assignment syntax* in the slot:

```blade
<x-layout>
    <Counter v-slot="{ count, increase }">
        <button @click="increase" v-text="count" />
    </Counter>
<x-layout>
```
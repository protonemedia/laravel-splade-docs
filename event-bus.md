# Event Bus

Splade has a built-in **Event Bus** that allows for communication between components. Events can be emitted by one component, and listened for by another.

## Emit from Vue component

To emit an event, you may call the `emit` method, for example, in a [custom Vue component](/custom-vue-components.md), on the global `$splade` instance:

```vue
<script>
export default {
    methods: {
        emitCheckoutEvent() {
            this.$splade.emit('checkout');
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

function emitCheckoutEvent() {
    Splade.emit('checkout');
}
</script>
```

Additionally, you may pass data along with the event:

```js
Splade.emit('checkout', { id: 1 });
```

## Emit on event

It's also possible to call the `emit` method from another event. For example, the [Form Component](/x-form.md) emits `error` and `success` events that you can hook into:

```blade
<x-splade-form @error="$splade.emit('checkout-failed')">
```

## Listen for events

You may register a listener by using the `on` method:

```js
this.$splade.on('checkout-failed', function() {
    // do something
});
```

When the callback is rather simple, you may use a [Script Component](/x-script.md) to avoid writing a custom Vue component:

```blade
<x-splade-script>
    $splade.on('checkout-failed', function() {
        // do something
    });
</x-splade-script>
```
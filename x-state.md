# X-Splade-State Component

The **State Component** is a convenient wrapper around validation errors, flash data, and shared data.

## Errors

The `state` prop has a `hasError` method, and you may access the first error of a key with the `state.errors` object. If you want full access to the server-side error bag, you may use the `rawErrors` object. To determine if there are any errors, you may use the `hasErrors` boolean.

```blade
<x-splade-state>
    <p v-if="state.hasError('name')" v-text="state.errors.name" />

    <div v-for="(errorArray, key) in state.rawErrors">
        ...
    </div>

    <div v-if="state.hasErrors">
        Whoops!
    </div>
</x-splade-state>
```

## Flash Data

Similar to validation errors, there is a `hasFlash` method and a `flash` object to access flashed data.

```blade
<x-splade-state>
    <p v-if="state.hasFlash('message')" v-text="state.flash.message" />
</x-splade-state>
```

## Shared Data

The same applies to shared data:

```blade
<x-splade-state>
    <p v-if="state.hasShared('publicKey')" v-text="state.shared.publicKey" />
</x-splade-state>
```
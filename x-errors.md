# X-Splade-Errors Component

You may use the **Errors Component** to show validation errors. As the value for a key is always an array, there's a helper method to retrieve the first error of the key:

```blade
<x-splade-errors>
    <p v-if="errors.has('name')" v-text="errors.first('name')" />
</x-splade-errors>
```

You may also access the array by using the key directly on the `errors` prop:

```blade
<x-splade-errors>
    <p>Name errors: <span v-text="errors.name.length" /></p>
</x-splade-errors>
```

Alternatively, you can loop through all errors:

```blade
<x-splade-errors>
    <div v-for="(errors, key) in errors.all">
        ...
    </div>
</x-splade-errors>
```
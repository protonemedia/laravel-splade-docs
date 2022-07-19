# X-Splade-Data Component

You may use the **Data Component** to interact with a set of reactive data *inside* the component. To get started, you don't even need to define a structure of default set of data.

```vue
<x-splade-data>
    <input v-model="data.name" />
    <p>Your name is <span v-text="data.name /></p>
</x-splade-data>
```

You may use the `default` attribute to pass a default set of data.

Note how this is a *JavaScript* object. The value passed to the `default` attribute will be parsed by Vue, not by PHP.

```vue
<x-splade-data default="{ name: 'Laravel Splade' }>
    <input v-model="data.name" />
</x-splade-data>
```

If you want to parse the value by PHP, you may use the `:default` attribute (note the colon).

```vue
<x-splade-data :default="['name' => 'Laravel Splade']">
    <input v-model="data.name" />
</x-splade-data>
```

As you can see, this allows you to pass in arrays, but you can use `Arrayable`, `Jsonable` or `JsonSerializable` classes as well. For example, you may pass an Eloquent Model:

```vue
<x-splade-data :default="\App\Models\User::first()">
    <input v-model="data.name" />
</x-splade-data>
```


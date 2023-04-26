# X-Splade-Data Component

You may use the **Data Component** to interact with a set of reactive data *inside* the component. To get started, you don't even need to define a structure or a default set of data.

```blade
<x-splade-data>
    <input v-model="data.name" />
    <p>Your name is <span v-text="data.name" /></p>
</x-splade-data>
```

## Default Data

You may use the `default` attribute to pass a default set of data.

Note how this is a *JavaScript* object. Therefore, the value passed to the `default` attribute will be parsed by Vue, not by PHP.

```blade
<x-splade-data default="{ name: 'Laravel Splade' }">
    <input v-model="data.name" />
</x-splade-data>
```

If you want to parse the value by PHP, you may use the `:default` attribute (note the colon).

```blade
<x-splade-data :default="['name' => 'Laravel Splade']">
    <input v-model="data.name" />
</x-splade-data>
```

As you can see, this allows you to pass in arrays, but you can also use `Arrayable`, `Jsonable`, or `JsonSerializable` classes. So, for example, you may pass an Eloquent Model:

```blade
<x-splade-data :default="\App\Models\User::first()">
    <input v-model="data.name" />
</x-splade-data>
```

Note that the user data is passed to the frontend, so please be sure sensitive attributes are [hidden](https://laravel.com/docs/10.x/eloquent-serialization#hiding-attributes-from-json), or use the built-in [Transformer feature](/transformers.md).

## Remember

You may use the `remember` attribute to keep the component's state while users navigate your app. For example, you could keep track of opened tabs in a menu. The `remember` attribute requires a key that's unique throughout your app.

```blade
<x-splade-data remember="menu" default="{ tab1: false, tab2: false, tab3: false }">
    <aside v-show="data.tab1">
        ...
    </aside>

    <aside v-show="data.tab2">
        ...
    </aside>

    <aside v-show="data.tab3">
        ...
    </aside>
</x-splade-data>
```

## Local Storage

The remembered state will be lost whenever the user fully refreshes the app. You may use the `local-storage` attribute to keep the component's state in the browser's local storage.

```blade
<x-splade-data remember="cookie-popup" local-storage default="{ accepted: false }">
    <div v-if="!data.accepted">
        <h1>Cookie warning</h1>
    </div>

    <button @click.prevent="data.accepted = true">Accept</button>
</x-splade-data>
```

## Global Store

Sometimes you want to use a set of reactive data through the template or layout. Instead of wrapping everything inside the data component, you may also define a store and use it at other places in your template.

```blade
<x-splade-data store="navigation" default="{ opened: false }" />

<!-- other elements... -->

<button @click.prevent="navigation.opened = true">
    Open Navigation
</button>

<nav v-show="navigation.opened">
    ...
</nav>
```

You must pass a key to the `store` attribute and avoid generic naming. For example, don't use keys like *data*, *form*, and *toggle* as they might interfere with other scoped Vue components.
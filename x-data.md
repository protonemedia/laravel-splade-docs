# X-Splade-Data Component

You may use the **Data Component** to interact with a set of reactive data *inside* the component. To get started, you don't even need to define a structure of a default set of data.

```blade
<x-splade-data>
    <input v-model="data.name" />
    <p>Your name is <span v-text="data.name" /></p>
</x-splade-data>
```

## Default Data

You may use the `default` attribute to pass a default set of data.

Note how this is a *JavaScript* object. The value passed to the `default` attribute will be parsed by Vue, not by PHP.

```blade
<x-splade-data default="{ name: 'Laravel Splade' }>
    <input v-model="data.name" />
</x-splade-data>
```

If you want to parse the value by PHP, you may use the `:default` attribute (note the colon).

```blade
<x-splade-data :default="['name' => 'Laravel Splade']">
    <input v-model="data.name" />
</x-splade-data>
```

As you can see, this allows you to pass in arrays, but you can use `Arrayable`, `Jsonable` or `JsonSerializable` classes as well. For example, you may pass an Eloquent Model:

```blade
<x-splade-data :default="\App\Models\User::first()">
    <input v-model="data.name" />
</x-splade-data>
```

## Remember

You may use the `remember` attribute to keep the state of the component while users navigate through your app. For example, you could keep track of opened tabs in a menu. The `remember` attribute requires key that's unique throughout your app.

```blade
<x-splade-data remember="menu" default="{ tab1: false, tab2: false, tab3: false }>
    <aside v-show="tab1">
        ...
    </aside>

    <aside v-show="tab2">
        ...
    </aside>

    <aside v-show="tab3">
        ...
    </aside>
</x-splade-data>
```

## Local Storage

The remembered state will be lost whenever the user performs a full refresh of the app. You may use the `local-storage` attribute to keep the state of the component in the browser's local storage.

```blade
<x-splade-data remember="cookie-popup" local-storage default="{ accepted: false }">
    <div v-if="!data.accepted">
        <h1>Cookie warning</h1>
    </div>

    <button @click.prevent="data.accepted = true">Accept</button>
</x-splade-data>
```
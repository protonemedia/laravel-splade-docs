# X-Splade-Form Component

The **Form Component** allows you to send forms asynchronously. This way, you'll prevent full page reloads and can present validation errors without reevaluating the form with old submitted data. In addition, you may use the `form` prop to do two-way binding. The default method is `POST`.

```blade
<x-splade-form action="/users/store">
    <input v-model="form.name" type="text" />
    <input v-model="form.email" type="email" />
    <button type="submit">Send</button>
</x-splade-form>
```

## Method and Validation errors

Just like traditional forms, there's a `method` attribute. In addition, you may use the `form.errors` prop to evaluate validation errors:

```blade
<x-splade-form method="put">
    ...
    <p v-text="form.errors.name" />
    ...
</x-splade-form>
```

## Default data

You may use the `default` attribute to pass a default set of data.

Note how this is a *JavaScript* object. Therefore, the value passed to the `default` attribute will be parsed by Vue, not PHP.

```blade
<x-splade-form default="{ name: 'Laravel Splade' }">
    <input v-model="form.name" />
</x-splade-form>
```

If you want to parse the value by PHP, you may use the `:default` attribute (note the colon). Note that, just like the previous example, the data is passed to the frontend, so be careful with sensitive data.

```blade
<x-splade-form :default="['name' => 'Laravel Splade']">
    <input v-model="form.name" />
</x-splade-form>
```

As you can see, this allows you to pass in arrays, but you can also use `Arrayable`, `Jsonable` or `JsonSerializable` classes. So, for example, you may pass an Eloquent Model:

```blade
<x-splade-form :default="\App\Models\User::first()" unguarded>
    <input v-model="form.name" />
</x-splade-form>
```

You might have noticed the `unguarded` attribute. Eloquent Models often contain sensitive data, so by default, `Fluent` and `Model` instances will be fully guarded. This means their attributes won't be passed to the client-side unless the form is unguarded.

Instead of fully unguarding the form, you may specify which attributes to unguard. You can do this with an array or string.

```blade
<x-splade-form ... unguarded="name" />

<x-splade-form ... unguarded="name, email" />

<x-splade-form ... :unguarded="['name', 'email']" />
```

You can read more about this behaviour on the [Form Model Binding](/form-model-binding-attributes.md) page. Even better, the dedicated [Form Components](/form-overview.md) handle this automatically for you.

## Confirmation

You may use the `confirm` attribute to show a confirmation dialog before the form is submitted:

```blade
<x-splade-form confirm>
```

In addition, you may customize the confirmation dialog:

```blade
<x-splade-form
    confirm="Delete profile"
    confirm-text="Are you sure you want to delete your profile?"
    confirm-button="Yes, delete everything!"
    cancel-button="No, I want to stay!"
>
```

## File uploads

Splade will send the form using a `FormData` object instead of a JSON-encoded string when the submitted data contains a file. This is done automatically for you. In addition, you may use the input event to bind the selected file to your form data:

```blade
<x-splade-form>
    <input type="file" dusk="avatar" @input="form.avatar = $event.target.files[0]" />
    <p v-text="form.errors.avatar" />
    <button type="submit">Submit</button>
</x-splade-form>
```

If for some reason, you want to force using a `FormData` object, you may use the `force-form-data` attribute:

```blade
<x-splade-form force-form-data>
```

## Reset and restore form

You may use the `form.reset` method to clear all form data.

```blade
<x-splade-form>
    <button @click.prevent="form.reset">
        Clear all values
    </button>
</x-splade-form>
```

Similarly, there's a `form.restore` method to restore the default values.

```blade
<x-splade-form>
    <button @click.prevent="form.restore">
        Restore default values
    </button>
</x-splade-form>
```

### Reset and restore on success

If you redirect back to the same page after a successful request, you may choose to reset or restore the form data automatically. You can do this with the `reset-on-success` and `restore-on-success` attributes:

```blade
<x-splade-form reset-on-success>
<x-splade-form restore-on-success>
```

## State

There are several props that you can use to show the state of the form:

```blade
<x-splade-form>
    <p v-if="form.processing">Submitting the data...</p>

    <p v-if="form.wasSuccessful">Successfully submitted!</p>

    <p v-if="form.recentlySuccessful">Flash message to show success!</p>
</x-splade-form>
```

## Form components

While writing traditional input elements is fine, Splade comes with various components to build forms even faster. Make sure to check out the [documentation page](/form-overview.md)!
# X-Splade-Form Component

The **Form Component** allows you to send forms asynchronously. This way, you'll prevent full page reloads and can present validation errors without reevaluating the form with old submitted data. In addition, you may use the `form` prop to do two-way binding. The default method is `POST`.

```blade
<x-splade-form action="/users/store">
    <input v-model="form.name" type="text" />
    <input v-model="form.email" type="email" />
    <button type="submit">Send</button>
</x-splade-form>
```

The example above uses regular HTML form inputs, but Splade also comes with a collection of feature-rich [Form Components](/form-overview.md). These components all support labels, validation, data-binding, are more.

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

## Using Data

Besides binding form data to an input element, you may the form data the build interactive elements just like the [Data component](/x-data.md). For example, you could use the `form` object to toggle classes or show an element.

```blade
<x-splade-form>
    {{-- Interact with the value of the data --}}
    <input type="text" v-model="form.name" />
    <p>Your name is: <span v-text="form.name" /></p>

    {{-- Toggle classes based on the value of the data --}}
    <input type="checkbox" v-model="form.newsletter" />
    <svg :class="{ 'text-green-500': form.newsletter, 'text-red-500': !form.newsletter }" />

    {{-- Show elements based on the value of the data --}}
    <input type="checkbox" v-model="form.agree_with_terms" />
    <button type="submit" v-show="form.agree_with_terms">Submit</button>
</x-splade-form>
```

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

You may use the input event to bind the selected file to your form data:

```blade
<x-splade-form>
    <input type="file" dusk="avatar" @input="form.avatar = $event.target.files[0]" />
    <p v-text="form.errors.avatar" />
    <button type="submit">Submit</button>
</x-splade-form>
```

The dedicated [File Component](/form-file.md) provides a cleaner solution, and has support for selecting multiple files as well as displaying the filename of the selected file.

## Submit on change

Sometimes you want to submit the form whenever a value changes, for example, on a settings page that you want to save immediately. For this, you may use the `submit-on-change` attribute.

```blade
<x-splade-form submit-on-change>
```

In addition, you may optionally specify one or more values (with an `array` or `string`) that Splade should watch instead of all values.

```blade
<x-splade-form submit-on-change="name">

<x-splade-form submit-on-change="name, email">

<x-splade-form :submit-on-change="['name', 'email']">
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

### Prevent navigation on submit

If you want to stay on the same page after a successful request and want to preserve the page's current state, add the `stay` attribute.

```blade
<x-splade-form stay>
```

### Reset and restore on success

You may choose to reset or restore the form data automatically. You can do this with the `reset-on-success` and `restore-on-success` attributes:

```blade
<x-splade-form stay reset-on-success>

<x-splade-form stay restore-on-success>
```

## State

There are several props that you can use to show the state of the form:

```blade
<x-splade-form>
    <p v-if="form.processing">Submitting the data...</p>
</x-splade-form>

<x-splade-form stay>
    <p v-if="form.wasSuccessful">Successfully submitted!</p>

    <p v-if="form.recentlySuccessful">Flash message to show success!</p>
</x-splade-form>
```

## Form API

The `form` object has several additional methods and properties that you could use. With the `$put` method, you can set a value:

```blade
<button @click.prevent="form.$put('plan', 'pro')">Set Plan to Pro</button>
```

The `$all` property could help you debug the Form by printing all values:

```blade
<pre v-text="form.$all" />
```

If you'd somehow need to submit the Form with a custom trigger, you can use the `submit` method:

```blade
<div @click.prevent="form.submit">Start</div>
```

As shown above, there's an `errors` object to evaluate validation errors, but there's also a `hasError` method to determine whether there is an error:

```blade
<svg v-show="form.hasError('avatar')">...</svg>
```

If you want full access to the server-side error bag, you may use the `rawErrors` object.

```blade
<div v-for="(errorArray, key) in form.rawErrors">
    ...
</div>
```

## Form components

While writing traditional input elements is fine, Splade comes with various components to build forms even faster. Make sure to check out the [documentation page](/form-overview.md)!
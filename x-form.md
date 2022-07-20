# X-Splade-Defer Component

The **Form Component** allows you to send forms asynchronously. This prevents full page reloads, and it can present validation errors without having to reevaluate the form with old submitted data. You may use the `form` prop to do two-way binding. The default method is `POST`.

```blade
<x-splade-form action="/users/store">
    <input v-model="form.name" type="text" />
    <input v-model="form.email" type="email" />
    <button type="submit">Send</button>
</x-splade-form>
```

## Method and Validation errors

Just like regular forms, there's a `method` attribute. You may use the `form.errors` prop to evaluate validation errors:

```blade
<x-splade-form method="put">
    ...
    <p v-text="form.errors.name" />
    ...
</x-splade-form>
```

## Confirmation

You may use the `confirm` attribute to show a confirmation dialog before the form is submitted:

```blade
<x-splade-form confirm>
```

You may customize the confirmation dialog:

```blade
<x-splade-form
    confirm="Delete profile"
    confirm-text="Are you sure you want to delete your profile?"
    confirm-button="Yes, delete everything!"
    cancel-button="No, I want to stay!"
>
```

## File uploads

When the submitted data contains a file, the form will be sent using a `FormData` object instead of a JSON-encoded string. This is done automatically for you. You may use the input event to bind the selected file to your form data:

```blade
<x-splade-form>
    <input type="file" dusk="avatar" @input="form.avatar = $event.target.files[0]" />
    <p v-text="form.errors.avatar" />
    <button type="submit">Submit</button>
</x-splade-form>
```

If for some reason you want to force using a `FormData` object, you may use the `force-form-data` attribute:

```blade
<x-splade-form force-form-data>
```


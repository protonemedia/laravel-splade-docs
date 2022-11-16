# X-Splade-File Component

The **File Component** is a dedicated component you can use to select files. This component is based mainly on the [Input component](/form-input.md) but provides additional logic to display the filename of the selected file. You don't have to manually use the input event to bind the selected files to the form. Splade does this for you.

```blade
<x-splade-form :default="$user">
    <x-splade-file name="avatar" />
</x-splade-form>
```

The component supports selecting multiple files as well by adding the `multiple` attribute.

```blade
<x-splade-form :default="['documents' => []]">
    <x-splade-file name="documents" multiple />
</x-splade-form>
```

## FilePond

The [FilePond](https://pqina.nl/filepond/) integration comes with a default stylesheet which you should import into the main JavaScript file. If you've used the automatic installer, it has already done this for you.

```js
import "@protonemedia/laravel-splade/dist/style.css";
import { renderSpladeApp, SpladePlugin } from "@protonemedia/laravel-splade";
```

Now you may add the `filepond` attribute to the component:

```blade
<x-splade-file name="avatar" filepond />
```

This works for uploading multiple files as well:

```blade
<x-splade-file name="avatar" multiple filepond />
```

You can instantiate FilePond with a [custom set of options](https://pqina.nl/filepond/docs/api/instance/properties/) by passing a *JavaScript* object to the `filepond` attribute. To pass a PHP array, you may use the `:filepond` attribute (note the colon).

```blade
<x-splade-file name="avatar" filepond="{ allowDrop: false }" />

<x-splade-file name="avatar" :filepond="['allowDrop' => false ]" />
```

### Image Preview

FilePond can render a downscaled preview of the selected image by adding the `preview` attribute to the component:

```blade
<x-splade-file name="avatar" filepond preview />
```

### File Type and Size Validation

FilePond supports validating the selected file based on the type and size. To validate the type, you may use the `accept` attribute:

```blade
<x-splade-file name="avatar" filepond accept="image/png" />
```

To validate the file size of the file, you may use the `min-size` or `max-size` attribute. You may once them both at once as well:

```blade
<x-splade-file name="avatar" filepond min-size="100KB" max-size="5MB" />
```

Please be aware that both features are *client-side* validation. For security reasons, make sure you use [server-side validation](https://laravel.com/docs/9.x/validation#validating-files) as well.

### Customize FilePond styling

FilePond uses a *SCSS* stylesheet to style the library. Our stylesheet extends the vendor stylesheet (of FilePond) and adds some Tailwind-specific tweaks. Make sure your bundler handles SCSS stylesheets correctly, for example, by installing `sass`. The `splade:publish-form-stylesheets` Artisan command copies the stylesheet to the `resources` directory of your app.

```bash
npm install sass -D

php artisan splade:publish-form-stylesheets
```

Then import the stylesheet in your main JavaScript file (instead of the default `@protonemedia/laravel-splade/dist/style.css` stylesheet):

```js
import "../css/filepond.scss"
```
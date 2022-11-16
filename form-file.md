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

<x-splade-file name="avatar" :filepond="['allowDrop' => false]" />
```

### Image Preview

FilePond can render a downscaled preview of the selected image by adding the `preview` attribute to the component:

```blade
<x-splade-file name="avatar" filepond preview />
```

### Validate type and size

FilePond supports validating the selected file based on the type and size. To validate the type, you may use the `accept` attribute:

```blade
<x-splade-file name="avatar" filepond accept="image/png" />
```

You may use the `min-size` or `max-size` attribute to validate the file size. You may use both attributes at once as well:

```blade
<x-splade-file name="avatar" filepond min-size="100KB" max-size="5MB" />
```

Please be aware that both features are *client-side* validation. For security reasons, make sure you use [server-side validation](https://laravel.com/docs/9.x/validation#validating-files) as well.

### Validate images

FilePond supports validating the dimensions of a selected image. For example, you may use the `min-width` and `min-height` attributes to validate the minimum width and height. Similarly, to validate the maximum width and height, you may use the `max-width` and `max-height` attributes:

```blade
<x-splade-file name="avatar" filepond min-width="200" min-height="200" />

<x-splade-file name="header" filepond max-width="1500" max-height="1500" />
```

If you want to validate against an exact size, you may use the `width` and `height` attributes:

```blade
<x-splade-file name="poster" filepond width="720" height="576" />
```

Alternatively, you may specify a minimum or maximum resolution:

```blade
<x-splade-file name="avatar" filepond :min-resolution="300 * 300" />

<x-splade-file name="header" filepond :max-resolution="1500 * 1200" />
```

### Asynchronous uploads

FilePond supports uploading the file to the server before the form is submitted. First, you must register a supporting route using the `spladeUploads()` method on the `Route` facade. As of version 0.7.6, the automatic installer does this for you. If you need to register the route manually, make sure it uses the `web` Middleware, for example, in `web.php`:

```php
Route::spladeUploads();
```

Next, in the template, add the `server` attribute to the component. From now on, when a user drops a file into the FilePond instance, it will immediately start uploading to the server.

```blade
<x-splade-file name="avatar" filepond server />
```

Splade will store the file in a temporary directory and report the path to the file back to the File component. So when the user submits the form, it will send this path instead of uploading the file.

There are three ways of handling the temporary upload. First, you may use the `HandleSpladeFileUploads` class, for example, in your controller:

```php
use Illuminate\Http\Request;
use ProtoneMedia\Splade\FileUploads\HandleSpladeFileUploads;    // [tl! add]

public function store(Request $request)
{
    HandleSpladeFileUploads::forRequest($request);  // [tl! add]

    $request->validate([
        'photo' => ['required', 'file', 'image'],
    ]);

    $path = $request->file('photo')->store('images');
}
```

The `HandleSpladeFileUploads` class will loop through the request data and transform paths to temporary uploads back into `UploadedFile` instances. Make sure you call the `forRequest()` method *before* validating the request. Instead of looping through all request data, you may also pass the key (or an array of keys):

```php
HandleSpladeFileUploads::forRequest($request, 'photo');
```

The second option is to use a Route Middleware. You may use the same `HandleSpladeFileUploads` class as the example above. Using the *fully qualified class name* will loop through all request data, but you may also specify one or more keys using the static `for` method.

```php
Route::post('podcast', StorePodcastController::class)
    ->middleware(HandleSpladeFileUploads::class);   // [tl! add]

Route::post('podcast', StorePodcastController::class)
    ->middleware(HandleSpladeFileUploads::for('photo'));    // [tl! add]
```

The last option is to use a [Form Request](https://laravel.com/docs/9.x/validation#form-request-validation). Then, you only have to implement the `HasSpladeFileUploads` interface and use the `file` validation rule. Splade will automatically extract the keys from the rules.

```php
use Illuminate\Foundation\Http\FormRequest;
use ProtoneMedia\Splade\FileUploads\HasSpladeFileUploads;   // [tl! add]

class StorePodcastRequest extends FormRequest implements HasSpladeFileUploads   // [tl! add]
{
    public function rules()
    {
        return [
            'photo' => 'required|file|image',
        ];
    }
}
```

### Custom temporary directory

By default, Splade uses the `/storage/splade-temporary-file-uploads` directory for temporary uploads. If you want to use a custom [Filesystem disk](https://laravel.com/docs/9.x/filesystem#configuration), you may update the `file_uploads.disk` key in the `splade.php` [configuration file](/customization.md). For now, it only supports local disks.

### Cleanup temporary uploads

It may happen that temporarily uploaded files are not being used and will fill the temporary directory. Splade comes with a built-in Artisan command to delete all files that are older than one hour:

```bash
php artisan splade:cleanup-uploads
```

You may change the lifetime of temporary files with the `file_uploads.temporary_file_lifetime` key in the `splade.php` [configuration file](/customization.md).

### Customize FilePond styling

FilePond uses an *SCSS* stylesheet to style the library. Our stylesheet extends the vendor stylesheet (of FilePond) and adds some Tailwind-specific tweaks. Make sure your bundler handles SCSS stylesheets correctly, for example, by installing `sass`. The `splade:publish-form-stylesheets` Artisan command copies the stylesheet to the `resources` directory of your app.

```bash
npm install sass -D

php artisan splade:publish-form-stylesheets
```

Then import the stylesheet in your main JavaScript file (instead of the default `@protonemedia/laravel-splade/dist/style.css` stylesheet):

```js
import "../css/filepond.scss"
```
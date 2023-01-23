---
description: The Splade File component can display the selected file and integrates with FilePond and Spatie's Media Library. It supports handling existing files, reordering files, async uploads, and image validation.

keywords: laravel file upload, laravel filepond, filepond spatie media library, filepond file manager, laravel file manager, laravel image library, laravel async upload, laravel image preview, laravel multiple file upload
---

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
    <x-splade-file name="documents[]" multiple />
</x-splade-form>
```

## Image preview

You might want to show a preview of the selected image. You may use the `form.$fileAsUrl` method to generate a *base64-encoded* representation of the image, and use it as the source of an `img` element. In addition, with the `show-filename` attribute, you can disable displaying the filename.

```blade
<x-splade-form>
    <x-splade-file name="avatar" :show-filename="false" />

    <img v-if="form.avatar" :src="form.$fileAsUrl('avatar')" />
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
<x-splade-file name="avatars[]" multiple filepond />
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

### Adding remote files

FilePond can upload a file using a Remote URL. The `form` object has an `$addFile` method that allows you to pass a URL to a FilePond instance. You must pass the *field* as the first argument and the *URL* as the second argument. In the example below, we'll use a regular input element to set the Remote URL:

```blade
<x-splade-form>
    <x-splade-file filepond server name="avatar" />

    <x-splade-input name="remote_url" label="Remote URL" />

    <button @click.prevent="form.$addFile('avatar', form.remote_url)">
        Add from Remote URL
    </button>
</x-splade-form>
```

If you need to add multiple files at once, you may use the `$addFiles` method, which accepts an array as a second argument.

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

There are three ways of handling the temporary upload. You may choose the option that best fits your needs. First, you may use the `HandleSpladeFileUploads` class, for example, in your controller:

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

### Cleanup temporary uploads

It may happen that temporarily uploaded files are not being used and will fill the temporary directory. Splade comes with a built-in Artisan command to delete all unused files that are older than one hour:

```bash
php artisan splade:cleanup-uploads
```

You may change the lifetime of temporary files with the `file_uploads.temporary_file_lifetime` key in the `splade.php` [configuration file](/customization.md).

### Custom temporary directory

By default, Splade uses the `/storage/splade-temporary-file-uploads` directory for temporary uploads. If you want to use a custom [Filesystem disk](https://laravel.com/docs/9.x/filesystem#configuration), you may update the `file_uploads.disk` key in the `splade.php` [configuration file](/customization.md). For now, it only supports local disks.

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

## Working with existing files

FilePond allows you to show existing files in the UI. Splade comes with a set of tools to help you present and preserve existing files. Imagine you use the file input to replace a user's avatar while still showing the current one. You may use the `ExistingFile` class to load the current avatar:

```php
use ProtoneMedia\Splade\FileUploads\ExistingFile;

$avatar = ExistingFile::fromDisk('public')->get('avatars/user.jpeg');
```

Then in the Blade template, you may use the `ExistingFile` instance as default form data:

```blade
<x-splade-form :default="['avatar' => $avatar]">
    <x-splade-file filepond preview name="avatar" />
    <x-splade-submit />
</x-splade-form>
```

If you submit the form *without* changing the avatar, the `avatar` field will be empty, but Splade will submit an `avatar_existing` field to inform you the current avatar hasn't changed. When you access that key from the `Request` instance, it will give you an instance of `ExistingFile` again:

```php
public function update(Request $request)
{
    HandleSpladeFileUploads::forRequest($request);

    // This is an instance of ExistingFile:
    $existingAvatar = $request->avatar_existing;
}
```

### Multiple existing files

You may also use multiple existing files with a file input that allows uploading multiple files. For example, instead of passing one instance of `ExistingFile`, you may now use an array:

```php
$photos = [
    ExistingFile::fromDisk('public')->get('photos/1.jpeg'),
    ExistingFile::fromDisk('public')->get('photos/2.jpeg'),
];
```

Luckily, the `get` method also accepts an array:

```php
$photos = ExistingFile::fromDisk('public')->get([
    'photos/1.jpeg',
    'photos/2.jpeg',
]);
```

Just like using a single existing file as default form data, you may do the same for multiple files:

```blade
<x-splade-form :default="['photos' => $photos]">
    <x-splade-file filepond multiple preview name="photos[]" />
    <x-splade-submit />
</x-splade-form>
```

The user may submit the form with existing and new files when dealing with multiple files. In addition, FilePond allows the user to reorder the files as well. So how would you handle such forms in the controller? Like the avatar example, Splade will submit a `photos` array with new files and a `photos_existing` array with the existing files. Additionally, the request data will have a `photos_order` key representing the order, as a user can mix existing and new files.

You can combine existing and new files with their order manually. However, Splade comes with a `orderedSpladeFileUploads()` method that you may call on a `Request` instance. This method returns a `Collection` with `SpladeFile` instances. The files are already in the correct order, and have helper methods like `exists()` and `doesntExist()` to determine the file's nature.

```php
use ProtoneMedia\Splade\FileUploads\SpladeFile;

public function update(Request $request)
{
    $request->orderedSpladeFileUploads('photos')->each(function (SpladeFile $file) {
        if ($file->exists()) {
            // This is an instance of ExistingFile:
            $file->existing;
        }

        if ($file->doesntExist()) {
            // This is an instance of UploadedFile:
            $file->upload;
        }
    })
}
```

### Spatie Media Library

The FilePond integration has built-in support for [Spatie's Media Library package](https://spatie.be/docs/laravel-medialibrary/v10/introduction). The `ExampleFile` class has a helper method to load media collections. Instead of loading a collection, it also works with the `getFirstMedia()` method.

```php
ExistingFile::fromMediaLibrary($model->getMedia());
```

Additionally, you may specify the [conversion name](https://spatie.be/docs/laravel-medialibrary/v10/converting-images/defining-conversions) that FilePond should use for a preview image. There's also support for customizing the expiration and options of the Temporary URL (S3 disk only).

```php
ExistingFile::fromMediaLibrary(
    media: $model->getMedia(),
    previewConversionName: 'thumb',
    previewUrlExpiration: now()->addMinutes(15),
    previewUrlOptions: ['ResponseContentType' => 'application/octet-stream']
);
```

Instead of using the `orderedSpladeFileUploads()` method and syncing the media collection, you may use the `syncMediaLibrary` method. This method will add and delete files and set the order. You must pass the `Request` instance, the Eloquent model that interacts with media (the subject), and the request key containing the files.

```php
public function update(Request $request)
{
    $user = $request->user();

    HandleSpladeFileUploads::syncMediaLibrary($request, $user, 'photos');
}
```

Optionally, there's a fourth and fifth argument to customize the name of the media collection and the name of the disk:

```php
HandleSpladeFileUploads::syncMediaLibrary(
    request: $request,
    subject: $user,
    key: 'photos',
    collectionName: 'photos',
    diskName: 's3'
);
```

### Additional helper methods

To help you identify existing files, you may set an array with metadata:

```php
$existingFile->metadata(['id' => $template->id]);
```

To retrieve the metadata, you may call the `getMetadata()` method to get the array, or pass a key to retrieve a value:

```php
$allMetadata = $existingFile->getMetadata();

$id = $existingFile->getMetadata('id');
```

Splade will automatically serialize Eloquent models and collections, and the metadata will be encrypted before it's passed to the Vue frontend.
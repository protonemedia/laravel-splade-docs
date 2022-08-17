# X-Splade-Input Component

The **Input Component** can be used for all kinds of input, like texts, emails, passwords and dates. The `type` attribute defaults to `text`.

```blade
<x-splade-form :default="$user">
    <x-splade-input name="username" />
    <x-splade-input name="job_title" label="Your Job Title" />
    <x-splade-input name="email" type="email" placeholder="Your Email Address" />
</x-splade-form>
```

## Flatpickr

To enable the date picker, add the `date` attribute to the Input component. You don't have to set the `type` attribute to `date`.

```blade
<x-splade-input name="published_at" date />
```

To enable to time picker, add the `time` attribute to the Input component.

```blade
<x-splade-input name="scheduled_at" time />
```

If you want a date + time picker, add both attributes.

```blade
<x-splade-input name="scheduled_at" date time />
```

If you want to select a date range, add the `range` attribute.

```blade
<x-splade-input name="scheduled_period" date range />
```

You can instantiate Flatpickr with a [custom set of options](https://flatpickr.js.org/options/) by passing a *JavaScript* object to either the `date` or `time` attribute. To pass a PHP array, you may use the `:date` or `:time` attribute (note the colon).

```blade
<x-splade-input name="published_at" date="{ showMonths: 2 }" />

<x-splade-input name="published_at" :date="['showMonths' => 2]" />

<x-splade-input name="scheduled_at" time="{ time_24hr: false }" />

<x-splade-input name="scheduled_at" :time="['time_24hr' => false]" />
```

### Default settings

You may set the default formatting by using static methods on the `Input` class:

```php
use ProtoneMedia\Splade\Components\Form\Input;

Input::defaultDateFormat('d-m-Y H:i');
Input::defaultTimeFormat('H:i');
Input::defaultDatetimeFormat('d-m-Y H:i');
```

If you want to use Flatpickr for every input element by default, you may use the static `defaultFlatpickr` method:

```php
Input::defaultFlatpickr();
```

You may even pass an array with default options to the method:

```php
Input::defaultFlatpickr([
    'showMonths' => 2
]);
```

### Customize Flatpickr styling

Flatpickr uses a *Stylus* stylesheet to style the library. Our stylesheet extends the vendor stylesheet (of Flatpickr) and adds some Tailwind-specific tweaks. Make sure your bundler handles Stylus stylesheets correctly, for example, by installing `stylus`. The `splade:publish-form-stylesheets` Artisan command copies the stylesheet to the `resources` directory of your app.

```bash
npm install stylus -D

php artisan splade:publish-form-stylesheets
```

Then import the stylesheet in your main JavaScript file (instead of the default `@protonemedia/laravel-splade/dist/style.css` stylesheet):

```js
import "../css/flatpickr.styl"
```
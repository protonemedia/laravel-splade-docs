# X-Splade-Select Component

The **Select Component** can render the options based on a *key-value* array or `Collection`, with support for groups.

```php
$countries = [
    'be' => 'Belgium',
    'nl' => 'The Netherlands',
];
```

```blade
<x-splade-select name="country_code" :options="$countries" />
```

You may also pass a set of objects, like an Eloquent Collection, and specify the *value* and *label* keys for the option elements:

```blade
<x-splade-select name="company" :options="$companies" option-label="name" option-value="id" />
```

You can provide a slot to the select element as well:

```blade
<x-splade-select name="country_code">
   <option value="be">Belgium</option>
   <option value="nl">The Netherlands</option>
</x-splade-select>
```

If you want a select element where multiple options can be selected, add the `multiple` attribute to the element.

```blade
<x-splade-select name="countries[]" :options="$countries" multiple />
```

## Placeholder

You may add a `placeholder` attribute to the select element. This will prepend a disabled option.

```
<x-splade-select name="country_code" placeholder="Select your country..." />
```

Rendered HTML:

```html
<select>
    <option value="" disabled>Select your country...</option>
    <!-- other options... -->
</select>
```

## Eloquent Relationships

The component has built-in support for `BelongsToMany` and `MorphToMany` relationships. To utilize this feature, you must add both the `multiple` and `relation` attributes to the select element.

In the example below, you can attach one or more tags to the video. Using the `relation` attribute will correctly retrieve the selected options (attached tags) from the database.

```blade
<x-splade-form :default="$video">
    <x-splade-select name="tags[]" :options="$tags" multiple relation />
</x-splade-form>
```

## Choices.js

The [Choices.js](https://github.com/Choices-js/Choices) integration comes with a default stylesheet which you should import into the main JavaScript file. If you've used the automatic installer, it has already done this for you.

```js
import "@protonemedia/laravel-splade/dist/style.css";
import { renderSpladeApp, SpladePlugin } from "@protonemedia/laravel-splade";
```

Now you may add the `choices` attribute to the component:

```blade
<x-splade-select name="framework" :options="$frameworks" choices />
```

It works for selecting multiple values as well:

```blade
<x-splade-select name="frameworks[]" :options="$frameworks" multiple choices />
```

You can instantiate Choices.js with a [custom set of options](https://github.com/Choices-js/Choices#setup) by passing a *JavaScript* object to the `choices` attribute. To pass a PHP array, you may use the `:choices` attribute (note the colon).

```blade
<x-splade-select name="framework" :options="$frameworks" choices="{ searchEnabled: false }" />

<x-splade-select name="framework" :options="$frameworks" :choices="['searchEnabled' => false ]" />
```

### Default settings

If you want to use Choices.js for every select element by default, you may use the static `defaultChoices` method on the `Select` class, for example, in the `AppServiceProvider` class:

```php
use ProtoneMedia\Splade\Components\Form\Select;

Select::defaultChoices();
```

You may even pass an array with default options to the method:

```php
Select::defaultChoices([
    'searchEnabled' => false
]);
```

### Remote Options

The component has support for loading the options using an asynchronous *ajax* request. For this, you may use the `remote-url` attribute:

```blade
<x-splade-select remote-url="/api/locations" />
```

Just like the regular pre-defined options, it supports objects as well:

```blade
<x-splade-select remote-url="/api/servers" option-label="ip" option-value="uuid" />
```

You might want to load the remote options from a nested dataset. In the example below, you want to extract the options from the `data.users` path.

```json
{
    "data": {
        "users": [
            {
                "id": 1,
                "name": "John"
            },
            {
                "id": 2,
                "name": "Jane"
            }
        ]
    }
}
```

You may use the `remote-root` attribute to set a base path for the options:

```blade
<x-splade-select remote-url="/api/users" remote-root="data.users" option-label="name" option-value="id" />
```

### Dynamic Remote URL

The `remote-url` attribute supports *Template literals*, making it perfect for building dependent selects:

```blade
<x-splade-select name="country" :options="$countries" />
<x-splade-select name="region" remote-url="`/api/regions/${form.country}`" />
```

When the user selects a country, it will reload the regions based on the chosen country.

When you're dealing with dynamic options, you probably don't know the option values beforehand. Therefore, you may instruct the component to automatically choose the first option of the freshly loaded options using the `select-first-remote-option` attribute:

```blade
<x-splade-select name="region" remote-url="`/api/regions/${form.country}`" select-first-remote-option />
```

Additionally, you may clear the selected option whenever the Remote URL changes using the `reset-on-new-remote-url` attribute:

```blade
<x-splade-select name="region" remote-url="`/api/regions/${form.country}`" reset-on-new-remote-url />
```

Both options can be configured to be applied by default. You may use the static methods on the `Select` class, for example, in the `AppServiceProvider` class:

```php
Select::defaultResetOnNewRemoteUrl();
Select::defaultSelectFirstRemoteOption();
```

All remote options features work with the Choices.js integration as well. It doesn't support groups yet, but that's coming in a future version of Splade.

### Customize Choices.js styling

Choices.js uses an *SCSS* stylesheet to style the library. Our stylesheet extends the vendor stylesheet (of Choices.js) and adds some Tailwind-specific tweaks. Make sure your bundler handles SCSS stylesheets correctly, for example, by installing `sass`. The `splade:publish-form-stylesheets` Artisan command copies the stylesheet to the `resources` directory of your app.

```bash
npm install sass -D

php artisan splade:publish-form-stylesheets
```

Then import the stylesheet in your main JavaScript file (instead of the default `@protonemedia/laravel-splade/dist/style.css` stylesheet):

```js
import "../css/choices.scss"
```

### Laravel Dusk macro

Splade has two macros that help you test Choices.js instances with [Laravel Dusk](https://laravel.com/docs/10.x/dusk). Instead of calling `select` with the *field* and *value* arguments, you may use the `choicesSelect` method:

```php
$browser->select('size', 'Large');  // [tl! remove]

$browser->choicesSelect('size', 'Large');  // [tl! add]
```

If you want to remove an item from a Choices.js instance with multiple options, you may use the `choicesRemoveItem` method:

```php
$browser->choicesRemoveItem('countries[]', 'NL');
```

You may change the name of the macros with the `dusk.choices_select_macro` and `dusk.choices_remove_item_macro` keys in the `splade.php` [configuration file](/customization.md).
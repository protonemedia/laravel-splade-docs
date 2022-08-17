# X-Splade-Select Component

The **Select Component** can render the options based on an array, with support for groups.

```php
$countries = [
    'be' => 'Belgium',
    'nl' => 'The Netherlands',
];
```

```blade
<x-splade-select name="country_code" :options="$countries" />
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
<x-splade-select name="frameworks[]" :options="$frameworks" mulitple choices />
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

### Customize Choices.js styling

Choices.js uses a *SCSS* stylesheet to style the library. Our stylesheet extends the vendor stylesheet (of Choices.js) and adds some Tailwind-specific tweaks. Make sure your bundler handles SCSS stylesheets correctly, for example, by installing `sass`. The `splade:publish-form-stylesheets` Artisan command copies the stylesheet to the `resources` directory of your app.

```bash
npm install sass -D

php artisan splade:publish-form-stylesheets
```

Then import the stylesheet in your main JavaScript file (instead of the default `@protonemedia/laravel-splade/dist/style.css` stylesheet):

```js
import "../css/choices.scss"
```
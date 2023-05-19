# X-Splade-Wysiwyg Component

The **Wysiwyg Component** provides an integration for [Jodit Editor 3](https://xdsoft.net/jodit/). Since Jodit's stylesheet is quite large, it is not included in the Splade bundle by default. You may include it in your application by adding `jodit.css` to your main `app.js` file:

```js
import "./bootstrap";
import "../css/app.css";
import "@protonemedia/laravel-splade/dist/style.css";
import "@protonemedia/laravel-splade/dist/jodit.css";  // [tl! add]

import { createApp } from "vue/dist/vue.esm-bundler.js";
import { renderSpladeApp, SpladePlugin } from "@protonemedia/laravel-splade";
```

Then you may use the component:

```blade
<x-splade-wysiwyg name="biography" />
```

## Configuration

You can instantiate Jodit with a [custom set of options](https://xdsoft.net/jodit/docs/classes/config.Config.html) by passing a *JavaScript* object to the `jodit` attribute. To pass a PHP array, you may use the `:jodit` attribute (note the colon).

```blade
<x-splade-wysiwyg name="biography" jodit="{ showXPathInStatusbar: true }" />

<x-splade-wysiwyg name="biography" :jodit="['showXPathInStatusbar' => true]" />
```

### Default settings

Suppose you want to specify a default set of options for all Jodit instances. In that case, you may use the static `defaultJoditOptions` method on the `Wysiwyg` class, for example, in the `AppServiceProvider` class:

```php
use ProtoneMedia\Splade\Components\Form\Wysiwyg;

Wysiwyg::defaultJoditOptions([
    'showXPathInStatusbar' => true,
]);
```

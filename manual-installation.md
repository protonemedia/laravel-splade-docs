# Manual Installation

## Server-side

Install the Laravel Splade package using composer:

```bash
composer install protonemedia/laravel-splade
```

Add the Route Middleware to the `Http/Kernel.php` file:

```php
class Kernel extends HttpKernel
{
    protected $routeMiddleware = [
        ...

        'splade' => \ProtoneMedia\Splade\Http\SpladeMiddleware::class,

        ...
    ];
}
```

Then in the routes file, typically `web.php`, you may use the Middleware:

```php
Route::middleware('splade')->group(function () {
    Route::view('/', 'welcome');
    Route::view('/contact', 'contact');
});
```

Lastly, in the `register` method of the `Exceptions/Handler.php` file, you need to register Splade's Exception Handler:

```php
use ProtoneMedia\Splade\SpladeCore;

class Handler extends ExceptionHandler
{
    public function register()
    {
        $this->renderable(SpladeCore::exceptionHandler($this));
    }
}
```

## Client-side

On the frontend, you must ensure [Tailwind CSS 3.0](https://tailwindcss.com) and [Vue 3.0](https://vuejs.org) are correctly configured. Then, install the Splade frontend package:

```bash
npm install @protonemedia/laravel-splade
```

In your Tailwind configuration file, make sure you add the Splade package to the content array:

```js
module.exports = {
    content: [
        ...

        './vendor/protonemedia/laravel-splade/lib/**/*.vue',
        './vendor/protonemedia/laravel-splade/resources/views/**/*.blade.php',

        ...
    ],
};
```

In the `createApp` section of your main JavaScript file, you need to use the Splade plugin, as well as the custom render method:

```js
import "@protonemedia/laravel-splade/dist/style.css";

import { createApp } from 'vue'
import { renderSpladeApp, SpladePlugin } from '@protonemedia/laravel-splade'

const el = document.getElementById('app')

createApp({
    render: renderSpladeApp({ el })
})
    .use(SpladePlugin)
    .mount(el);
```

As you can see at the top, there's also a default stylesheet to support the Choices.js and Flatpickr integrations of the [Form Components](/form-overview.md). Though you probably want to import this default stylesheet into your main JavaScript file, it's completely optional.

In your Blade root layout, you may use the `@splade` directive inside the `body`, and the `@spladeHead` directive inside the `head`. This will render the title and meta tags, and the default `<div id="app"></div>` element where the Vue app will be mounted.

```blade
<head>
    @spladeHead
</head>

<body class="antialiased">
    @splade
</body>
```

Splade assumes the path of this file is `resources/views/root.blade.php`. If you want to change it, you may call the `setRootView` method on the Splade facade, for example, in the `AppServiceProvider` class:

```php
Splade::setRootView('base-layout');
```

Lastly, in the `vite.config.js` file, you need to add an alias for the [Vue template compiler](https://vuejs.org/guide/scaling-up/tooling.html#note-on-in-browser-template-compilation):

```js
export default defineConfig({
    ...

    resolve: {
        alias: {
            vue: 'vue/dist/vue.esm-bundler'
        }
    },

    ...
});
```
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

On the frontend, you need to make sure [Tailwind CSS 3.0](https://tailwindcss.com) and [Vue 3.0](https://vuejs.org) are configured properly. Then, install the Splade frontend package:

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

In the `createApp` section your main JavaScript file, you need to use the Splade plugin, as well as the custom render method:

```js
import { createApp } from 'vue'
import { renderSpladeApp, SpladePlugin } from '@protonemedia/laravel-splade'

const el = document.getElementById('app')

createApp({
    render: renderSpladeApp({ el })
})
    .use(SpladePlugin)
    .mount(el);
```

In your Blade root layout, you may use the `@splade` directive inside the `body`. This will render the default `<div id="app"></div>` element where the Vue app will be mounted:

```blade
<body class="antialiased">
    @splade
</body>
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
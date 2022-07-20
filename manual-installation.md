# Manual Installation

## Server-side

## Client-side

On the frontend, you need to make sure [Tailwind CSS 3.0](https://tailwindcss.com) and [Vue 3.0](https://vuejs.org) are configured properly. In the `createApp` section your main JavaScript file, you need to use the Splade plugin, as well as the custom render method:

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

In your Blade root layout, you may use the `@splade` directive. This will render the default `<div id="app"></div>` element where the Vue app will be mounted:

```blade
<!DOCTYPE html>
<html>
    <head>
        ...
        @vite('resources/js/app.js')
        ...
    </head>

    <body class="antialiased">
        @splade
    </body>
</html>
```
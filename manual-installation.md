# Manual Installation

## Server-side

## Client-side

On the frontend, you need to make sure [Tailwind CSS 3.0](https://tailwindcss.com) and [Vue 3.0](https://vuejs.org) are configured properly. In your Tailwind configuration file, make sure you add the Splade package to the content array:

```js
const defaultTheme = require('tailwindcss/defaultTheme');

/** @type {import('tailwindcss').Config} */
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
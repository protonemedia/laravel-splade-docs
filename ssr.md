# Server-Side Rendering (SSR)

As Vue handles the template rendering client-side, you might want to enable SSR to improve the SEO score of your application. Splade uses the SSR functionality in the [Laravel Vite plugin](https://laravel.com/docs/10.x/vite#ssr).

If you've used the automatic installer, everything is ready for you, and you can start the server almost immediately. However, if you need to set up SSR manually, check out the section below on preparing your app for SSR.

## Build and start the SSR Server

First, enable SSR in the Laravel configuration via the `SPLADE_SSR_ENABLED` environment variable:

```bash
SPLADE_SSR_ENABLED=true
```

By running `npm run build`, Vite will build the SSR entry point and save it at `bootstrap/ssr/ssr.mjs`. Now you can use `node` to start the server:

```bash
node bootstrap/ssr/ssr.mjs
```

By default, it starts on port 9000, but you may choose another port. Don't forget to update the port in the `splade.php` [configuration file](/customization.md).

```bash
node bootstrap/ssr/ssr.mjs --port=4242
```

If you create a daemon that runs the server, don't forget to restart it after every deployment.

## Blade Fallback

If you have problems running the server or whenever it crashes, Splade can fall back to the rendered Blade content. Of course, you should always prefer the rendered content by the SSR server, but it’s nice to have a fallback. This way, search engine crawlers won’t index an empty page.

This feature is enabled by default but can be disabled in the `splade.php` configuration file.

## Manual installation

In the `resources/js` folder, you need to create an `ssr.js` file with the following content.

```js
import { createServer } from "http";
import { createSSRApp } from "vue";
import { renderToString } from "@vue/server-renderer";
import { renderSpladeApp, SpladePlugin, startServer } from "@protonemedia/laravel-splade";

startServer(createServer, renderToString, (props) => {
    return createSSRApp({
        render: renderSpladeApp(props)
    })
        .use(SpladePlugin);
});
```

You must add this file to the `laravel` plugin of the `vite.config.js` file:

```js
export default defineConfig({
    plugins: [
        laravel({
            ...
            ssr: "resources/js/ssr.js",
            ...
        })
    ]
})
```

Lastly, you need to update the `build` script of the `package.json` file:

```json
"scripts": {
    ...
    "build": "vite build && vite build --ssr"
    ...
}
```

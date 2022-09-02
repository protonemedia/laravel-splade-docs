# Progress Bar

Splade's page loading capabilities are asynchronous, so you probably want to display a loading bar whenever it performs requests. You can enable it in the plugin options of the `app.js` file with the `progress_bar` key:

```js
createApp({ render: renderSpladeApp({ el }) })
    .use(SpladePlugin, {
        'progress_bar': true,
    })
    .mount(el);
```

By default, the progress bar appears after a delay of 250 ms. Under the hood, it uses the [NProgress library](https://github.com/rstacruz/nprogress). You can customize the library and the default delay by passing an object instead of a boolean:

```js
'progress_bar': {
    delay: 250,
    color: "#4B5563",
    css: true,
    spinner: false,
}
```
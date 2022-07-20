# State Management

Splade uses Vue's `keep-alive` component to remember the state of a page. When you navigate to the next page, and hit the *back-button* of your browser, the previous page did not loose its state. By default, it remembers up to 10 pages, but you can configure it in the plugin options of the `app.js` file:

```js
createApp({ render: renderSpladeApp({ el }) })
    .use(SpladePlugin, {
        'max_keep_alive': 50,
    })
    .mount(el);
```
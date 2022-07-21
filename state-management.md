# State Management

Splade uses Vue's `keep-alive` component to remember the state of a page. So when you navigate to the next page and hit your browser's back button, the previous page does not lose its state. By default, it remembers up to 10 pages, but you can configure it in the plugin options of the `app.js` file:

```js
createApp({ render: renderSpladeApp({ el }) })
    .use(SpladePlugin, {
        'max_keep_alive': 50,
    })
    .mount(el);
```
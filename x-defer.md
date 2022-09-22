# X-Splade-Defer Component

The **Defer Component** allows you to load data asynchronously. The component exposes a `processing`, `response`, and `reload` property.

```blade
<x-splade-defer url="https://api.com/quote/random">
    <p v-show="processing">Loading random quote...</p>
    <p v-if="response" v-text="response.data.quote" />
    <button @click.prevent="reload">New quote</button>
</x-splade-defer>
```

## Request body

By default, the component performs a `GET` request with an `application/json` accept header. You may change this with the `method` and `accept-header` attributes. In case you need to post data, you may use the `request` attribute:

```blade
<x-splade-defer method="POST" url="/post/increase" request="{ post_id: 1 }">
    ...
</x-splade-defer>
```

## Poll

You may also use this component to poll for new data. With the `poll` attribute, you can specify the interval in milliseconds.

```blade
<x-splade-defer accept="text/html" url="/news/latestHeadline" poll="5000">
    <p v-text="response" />
</x-splade-defer>
```

## Async templates

If you want to load sections of your template asynchronously, check out the [Lazy Loading](/lazy-loading.md) section.
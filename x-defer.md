# X-Splade-Defer Component

The **Defer Component** allows you to load data asynchronously. The component exposes a `processing`, `response`, and `reload` property.

```blade
<x-splade-defer url="https://api.com/quote/random">
    <p v-show="processing">Loading random quote...</p>
    <p v-if="response.data" v-text="response.data.quote" />
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

## Manual

By default, the component starts loading the data right after the browser has loaded the page. You may disable this by adding a `manual` attribute.

```blade
<x-splade-defer manual url="https://api.com/quote/random">
    <button @click.prevent="reload">Load quote</button>
</x-splade-defer>
```

## Watch

The component can watch a passed value for changes using the `watch-value` attribute. Changes to the passed value will trigger a reload of the data. An example of this is reloading the data when form input changes. You may even debounce the execution with the `watch-debounce` attribute.

```blade
<x-splade-form default="{ amount: 1 }">
    <x-splade-defer
        url="/product/reservation"
        method="post"
        request="{ product: 'book', amount: form.amount }"
        watch-value="form.amount"
    >
        <input v-model="form.amount" type="number" />
        <p v-text="response" />
    </x-splade-defer>
</x-splade-form>
```

If you want to watch all form data, you may pass `form.$all` to the `watch-value` attribute.

## Async templates

If you want to load sections of your template asynchronously, check out the [Lazy Loading](/lazy-loading.md) section.
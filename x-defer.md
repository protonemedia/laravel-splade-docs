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

By default, the component performs a `GET` request with an `application/json` accept header. You may change this with the `method` and `accept-header` attributes. In case you need to post data, you may use the `request` attribute. Just like the [Data component](/x-data.md), it allows you to pass a PHP value *or* a JavaScript object. The value passed to the `request` attribute will be parsed by Vue, not by PHP.

```blade
<x-splade-defer method="POST" url="/post/increase" request="{ post_id: 1 }">
    ...
</x-splade-defer>
```

If you want to parse the value by PHP, you may use the `:request` attribute (note the colon).

```blade
<x-splade-defer method="POST" url="/post/increase" :request="['post_id' => 1]">
```

You can do the same for adding headers:

```blade
<x-splade-defer method="POST" url="/post/increase" headers="{ 'In-Background': 1 }">

<x-splade-defer method="POST" url="/post/increase" :headers="['In-Background' => 1]">
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

## Event

The component emits `success` and `error` events, allowing you to interact with the response data within your template. For example, you may set a form value on a successful request. Note that the `url` attribute supports *Template literals*, making it perfect for generating dynamic URLs:

```blade
<x-splade-form>
    <x-splade-input name="zip" label="ZIP code" />
    <x-splade-input name="city" label="City" />

    <x-splade-defer
        url="`https://example-city-api.com/${form.zip}/json/`"
        watch-value="form.zip"
        watch-debounce="100"
        manual
        @success="(response) => form.city = response.city"
    />
</x-splade-form>
```

## Async templates and Rehydration

If you want to load sections of your template asynchronously, check out the [Lazy Loading](/lazy-loading.md) section. There's also a [Rehydrate Component](/x-rehydrate.md) to reload a section of your Blade template.
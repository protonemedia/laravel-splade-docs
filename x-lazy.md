# X-Splade-Lazy Component

The **Teleport Component** allows you to load sections of your template lazily. Wrap the section that you want to lazy loading into the component, and Splade magically handles it for you.

```blade
<h1>Today's news items</h1>

<x-splade-lazy>
    @foreach($items as $item)
        ...
    @endforeach
</x-splade-lazy>
```

## Lazy View Data

While excluding a section from your template is nice, you probably also want to exclude data from the initial page load. Most commonly, this is the data you need in your lazily loaded content. You may use the `onInit` and `onLazy` methods on the `Splade` facade to wrap the data in a closure.

```php
view('news', [
    // This will always be passed to the template (initial + lazy loading)...
    'headline' => 'Laravel Splade is awesome!',

    // This will only be evaluated on the initial page load...
    'categories' => Splade::onInit(fn () => Categories::all()),

    // This will only be evaluated when lazy loading...
    'items' => Splade::onLazy(fn () => NewsItem::latest()->limit(10)->get()),
]);
```

## Placeholder

When the request is processing, you can show a placeholder to indicate the status:

```blade
<x-splade-lazy>
    <x-slot:placeholder> The items are loading... </x-slot:placeholder>

    @foreach($items as $item)
        ...
    @endforeach
</x-splade-lazy>
```

## Conditionally load

Usually, the lazy loading performs right after the initial page is loaded. But sometimes, you want to postpone the lazy loading. You may use the `show` attribute to indicate whether the content should be loaded. This is great for things like toggling a notification section:

```blade
<x-splade-toggle>
    <button @click.prevent="toggle">Toggle notifications</button>

    <x-splade-lazy show="toggled">
        <x-slot:placeholder>
            <p>Loading notifications...</p>
        </x-slot:placeholder>

        @foreach($notifications as $notification)
            ...
        @endforeach
    </x-splade-lazy>
</x-splade-toggle>
```

Note that when the content is loaded, and the user hides the notifications after that, showing the notifications again will perform a new request to refresh the content.

## Load from another URL

If you don't want to include the content directly into the slot, you may define a custom URL to tell Splade where to fetch the content. Not that the endpoint should use the `SpladeMiddleware` as well.

```blade
<x-splade-lazy url="/notifications">
    ...
</x-splade-lazy>
```
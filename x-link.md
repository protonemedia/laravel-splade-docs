# Link Component

Unlike most Splade components, the **Link Component** is a Vue component. It's a wrapper around the `<a>` element, which prevents a full refresh but loads the linked page asynchronously.

```blade
<Link href="/users">All Users</Link>
```

## Confirmation

You may use the `confirm` attribute to show a confirmation dialog before Splade loads the new page:

```blade
<Link confirm href="/danger">Danger Zone</Link>
```

In addition, you may customize the confirmation dialog:

```blade
<Link
    confirm="Enter the danger zone..."
    confirm-text="Are you sure?"
    confirm-button="Yes, take me there!"
    cancel-button="No, keep me save!"
    href="/danger">
    Danger Zone
</Link>
```

## Redirecting To External Domains

When the URL is outside your application pointing to an external domain, you'd typically use a regular `<a>` element. Still, you may use the `away` attribute on the `Link` component. This can be useful when you have wrapped the component into another component and don't want to change the tag dynamically.

```blade
<Link away href="https://www.google.com">Google</Link>
```

## Transform all anchors

If you don't want to use the `Link` component but want Splade to transform all `<a>` elements, you need to update the plugin options in the `app.js` file:

```js
createApp({ render: renderSpladeApp({ el }) })
    .use(SpladePlugin, {
        'transform_anchors': true,
    })
    .mount(el);
```

## Method, Headers, and Request Data

By default, the asynchronous page load is a `GET` request, but you may change this with the `method` attribute:

```blade
<Link href="/template/new" method="POST">Start new template</Link>
```

The component also supports custom headers and request data. While you can use the `headers` and `data` attributes on the `Link` component, there's also a Blade variant of the component. Just like the [Data component](/x-data.md), it allows you to pass a PHP value *or* a JavaScript object:

The value passed to the `data` attribute will be parsed by Vue, not by PHP.

```blade
<x-splade-link href="/template/new" method="POST" data="{ name: 'Untitled Template' }">
```

If you want to parse the value by PHP, you may use the `:data` attribute (note the colon).

```blade
<x-splade-link href="/template/new" method="POST" :data="['name' => 'Untitled Template']">
```

You can do the same for adding headers:

```blade
<x-splade-link href="/template/new" method="POST" headers="{ 'X-Redirect-To-Billing-Portal': 1 }">

<x-splade-link href="/template/new" method="POST" :headers="['X-Redirect-To-Billing-Portal' => 1]">
```

Additionally, a `preserve-scroll` attribute prevents the page from scrolling to the top. This can be useful when submitting a form from a [Table component](/table-overview.md) while returning `redirect()->back()` from the controller:

```blade
<x-splade-link href="/project/touch" method="POST" :data="['id' => $item->id]" preserve-scroll>
```
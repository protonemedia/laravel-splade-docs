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
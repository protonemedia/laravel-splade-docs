# Link Component

The **Link Component** is a Vue component that is a wrapper around the `<a>` element. This element prevents a full refresh, but loads the linked page asynchronously.

```blade
<Link href="/users">All Users</Link>
```

## Confirmation

You may use the `confirm` attribute to show a confirmation dialog before Splade loads the new page:

```blade
<Link confirm href="/danger">Danger Zone</Link>
```

You may customize the confirmation dialog:

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
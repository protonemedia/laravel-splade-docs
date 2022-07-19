# X-Splade-Flash Component

You may use the **Flash Component** to interact with [Flash Data](https://laravel.com/docs/9.x/session#flash-data). This is helpful when you navigate back to the same page, as Splade won't reload the page by default, and thus server-side rendered Flash Data won't be updated.

```blade
<x-splade-flash>
    <p v-if="flash.has('message')" v-text="flash.message" />
</x-splade-flash>
```

> **Warning**
> By default, Splade will share *all* Flash Data to the frontend. You may disable this behaviour with the `splade.share_session_flash_data` configuration key.

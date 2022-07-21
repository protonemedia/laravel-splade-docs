# X-Splade-Flash Component

You may use the **Flash Component** to interact with [Flash Data](https://laravel.com/docs/9.x/session#flash-data). First, of course, you may render Flash Data server-side with Blade, but if you redirect back to the same page, for example, after form submission, Splade won't reload the page by default. So, the browser won't update the server-side rendered Flash Data.

```blade
<x-splade-flash>
    <p v-if="flash.has('message')" v-text="flash.message" />
</x-splade-flash>
```

> **Warning**
> By default, Splade will share *all* Flash Data to the frontend. You may disable this behavior with the `splade.share_session_flash_data` configuration key.

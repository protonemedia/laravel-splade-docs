# Navigation and Routing

To fully leverage Splade's page loading capabilities, you must use the *Splade Middleware* on according routes. There are many ways of [Registering Middlewares](https://laravel.com/docs/10.x/middleware#registering-middleware), but we recommend wrapping all routes into a group:

```php
Route::middleware('splade')->group(function () {
    Route::view('/', 'welcome');
    Route::view('/contact', 'contact');
});
```

Now when you use the `<Link>` component, Splade will prevent a full page reload and only load the new page:

```blade
<h1>Welcome!</h1>

<Link href="/contact">Visit contact page</Link>
```

You may also configure Splade to transform all `<a>` elements as described in the [Link Component section](/x-link.md).

## Preserve scroll position

When the user navigates through the app, the browser will reset the scroll position of scrollable elements. In some cases, this might not be a great user experience.

One example is a sidebar menu that's taller than the window height and thus has a vertical scrollbar. When a user clicks on a menu item in the bottom section and Splade navigates to the next page, you probably want to preserve the scroll position of the sidebar menu.

You may use the `@preserveScroll` directive on an element to tell Splade to keep track of the scroll position. Be sure to pass a unique name to the directive:

```blade
<div>
    <div @preserveScroll('sidebar') class="w-[16rem] absolute top-0 left-0 max-h-screen h-full overflow-y-scroll">
        {{-- scrolling menu --}}
    </div>

    <div class="pl-[16rem]">
        {{-- content --}}
    </div>
</div>
```

Alternatively, you may use the Vue equivalent of the directive:

```blade
<div v-SpladePreserveScroll:sidebar>
    ...
</div>
```

## Redirects

Splade automatically handles redirects, both internally and externally. The default Laravel redirect methods just work:

```php
return redirect('home/dashboard');

return redirect()->route('profile', ['id' => 1]);

return redirect()->away('https://www.google.com');
```

Splade detects when you redirect to an external domain outside of your app. You may use Laravel's [built-in](https://laravel.com/docs/10.x/responses#redirecting-external-domains) `away()` method on the `redirect()` helper method. Still, you may also use the `redirectAway()` method on the Splade facade to explicitly tell Splade to redirect away from the app:

```php
return Splade::redirectAway('https://www.google.com');
```

## View Transitions API

View Transitions is a new API in modern browsers designed to animate page transitions in SPAs like Splade. You can read more about it in the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API). Note that this API is still experimental and not supported in all browsers.

To enable View Transitions in your Splade app, you must update the plugin options in the main `app.js` file by setting the `view_transitions` option to `true`:

```js
createApp({ render: renderSpladeApp({ el }) })
    .use(SpladePlugin, {
        'view_transitions': true,
    })
    .mount(el);
```

Splade automatically detects if the browser supports the API.
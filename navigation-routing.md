# Navigation and Routing

To fully leverage Splade's page loading capabilities, you must use the *Splade Middleware* on according routes. There are many ways of [Registering Middlewares](https://laravel.com/docs/9.x/middleware#registering-middleware), but we recommend wrapping all routes into a group:

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

You may also configure Splade to transform all `<a>` elements as described in the Link Component section.
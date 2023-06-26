# Exception handling

## Exceptions in PHP

Splade uses a custom Exception Handler, registered in the `Exceptions/Handler.php` file by the [automatic installer](/automatic-installation.md).

```php
use ProtoneMedia\Splade\SpladeCore;

class Handler extends ExceptionHandler
{
    public function register()
    {
        $this->renderable(SpladeCore::exceptionHandler($this));
    }
}
```

If you want to add a custom handler, you may pass a second argument with a closure to the `exceptionHandler()` method. This implementation is similar to Laravel's [default implementation](https://laravel.com/docs/10.x/errors#rendering-exceptions).

```php
SpladeCore::exceptionHandler($this, function (Throwable $e, Request $request) {
    if ($e instanceof HttpException && $e->getStatusCode() === 419) {
        return new RedirectResponse('/login?reason=timeout');
    }
});
```

## Exceptions in the browser (JavaScript/Vue)

As Vue renders the templates in the browser, invalid markup might cause an error. The most common error is a missing HTML element, like a closing `</div>` tag. Unfortunately, Vue handles such errors differently in development and production mode. In development mode, Vue will throw an error in the console, but in production mode, Vue will silently fail and most likely render a blank page.

Splade can mimic the development behavior in production mode to prevent rendering a blank page. Note that this will not fix the underlying issue, but it will at least show an error message in the browser console instead of showing a blank page. To enable this behavior, you need to update the plugin options in the main `app.js` file by setting the `suppress_compile_errors` option to `true`:

```js
createApp({ render: renderSpladeApp({ el }) })
    .use(SpladePlugin, {
        'suppress_compile_errors': true,
    })
    .mount(el);
```
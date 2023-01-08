# Exception handling

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

If you want to add a custom handler, you may pass a second argument with a closure to the `exceptionHandler()` method. This implementation is similar to Laravel's [default implementation](https://laravel.com/docs/9.x/errors#rendering-exceptions).

```php
SpladeCore::exceptionHandler($this, function (Throwable $e, Request $request) {
    if ($e instanceof HttpException && $e->getStatusCode() === 419) {
        return new RedirectResponse('/login?reason=timeout');
    }
});
```
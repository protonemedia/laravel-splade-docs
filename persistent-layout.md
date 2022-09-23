# Persistent Layout

When Splade performs a page load, Vue will, under the hood, rerender the page and its components. While it's clever enough to minimize the performance impact and thus give you a seamless *SPA*-experience, it will also rerender elements you might want to keep alive. The most common example is a media player that must continue playing while your users navigate your app.

Luckily, Splade lets you use a Persistent Layout. So, instead of documenting all options, let's build the example with the media player step-by-step.

## Make a Layout

Create a new Blade Component using the Laravel Artisan CLI. You can read more about building Layouts with Blade Components in the [Laravel documentation](https://laravel.com/docs/9.x/blade#layouts-using-components).

```bash
php artisan make:component VideoLayout
```

All Blade Components extend the `Illuminate\View\Component` class by default. The only thing you have to change is extending the Blade Component with the `ProtoneMedia\Splade\Components\PersistentComponent` class:

```php
use ProtoneMedia\Splade\Components\PersistentComponent;

class VideoLayout extends PersistentComponent
{
    public function render()
    {
        return view('components.video-layout');
    }
}
```

Next, make sure the `$slot` is echoed out, and add the elements you want to persist across different pages.

```blade
<div>
    {{ $slot }}

    <div class="fixed ...">
        <video>
            ...
        </video>
    </div>
</div>
```

## Utilizing the Layout

Now you may use the `video-layout` component in other Blade views. To keep the `video` element alive, all navigation should use the [Link component](/x-link.md).

```blade
<x-video-layout>
    <h1> Page Title </h1>

    <Link href="/another-page"> Click here </Link>
</x-video-layout>
```
# Table (DataTables) Component

Splade has an advanced Table component that supports auto-fill, searching, filtering, sorting, toggling columns, and pagination. It's fully integrated and doesn't require any additional dependencies. Though optional, it integrates beautifully with Spatie's [Laravel Query Builder](https://github.com/spatie/laravel-query-builder).

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/FPYNvO7GyoM?controls=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Basic example

You may use the `SpladeTable` class to configure the table in your controller.

```php
$users = User::paginate();

return view('users.index', [
    'users' => SpladeTable::for($users)
        ->column('name')
        ->column('email'),
]);
```

In your Blade template, use the `x-splade-table` component to render the table.

```blade
<x-splade-table :for="$users" />
```

That's all! It will automatically render the head, body, and pagination.

## Custom head and body

If you want to write the head or body yourself, you may use slots.

```blade
<x-splade-table :for="$users">
    <x-slot name="head">
        <thead>
            ...
        </thead>
    </x-slot>

    <x-slot name="body">
        <tbody>
            ...
        </tbody>
    </x-slot>
</x-splade-table>
```


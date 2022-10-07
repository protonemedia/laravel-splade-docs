# Table (DataTables) Component

Splade has an advanced Table component that supports auto-fill, searching, filtering, sorting, toggling columns, and pagination. It's fully integrated and doesn't require any additional dependencies. Though optional, it integrates beautifully with Spatie's [Laravel Query Builder](https://github.com/spatie/laravel-query-builder).

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/FPYNvO7GyoM?controls=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Basic example

You may use the `SpladeTable` class to configure the table in your controller.

```php
$perPage = request()->query('perPage', 15);

$users = User::paginate($perPage);

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

## Pagination

When the dataset is paginated, it will, by default, show a select dropdown to customize the number of rows per page. You may define a custom set of options using the `perPageOptions` method on the `SpladeTable`:

```php
SpladeTable::for($users)
    ->perPageOptions([50, 100, 200])
    ->...
```

You may also set this globally using the static `defaultPerPageOptions` method, for example, in the `AppServiceProvider` class. If you want to disable the select dropdown, you may pass an empty array.

```php
SpladeTable::defaultPerPageOptions([250, 500]);
```

You may want the hide the pagination when the resource contains only one page, resulting in a somewhat cleaner UI. You may use the static `hidePaginationWhenResourceContainsOnePage()` method:

```php
SpladeTable::hidePaginationWhenResourceContainsOnePage();
```

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


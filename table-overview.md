# Table (DataTables) Component

Splade has an advanced Table component that supports auto-fill, searching, filtering, sorting, toggling columns, and pagination. You may also perform bulk actions and exports. Though optional, it integrates beautifully with Spatie's [Laravel Query Builder](https://github.com/spatie/laravel-query-builder).

*This video is from a previous Splade version that did not have the built-in Query Builder yet. An updated instruction video will soon replace it!*
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/FPYNvO7GyoM?controls=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Basic example

You may use the `SpladeTable` class to configure the table in your controller.

```php
return view('users.index', [
    'users' => SpladeTable::for(User::class)
        ->column('name')
        ->column('email')
        ->paginate(15),
]);
```

In your Blade template, use the `x-splade-table` component to render the table.

```blade
<x-splade-table :for="$users" />
```

That's all! It will automatically render the head, body, and pagination.

## Table Class

Instead of using an *inline* `SpladeTable` in the controller, you may create a dedicated Table class. A Table class lets you keep the configuration out of the controller and reuse the Table class. If you want to use Bulk Actions and Exports, a Table class is required.

There's an Artisan command to create a Table class:

```bash
php artisan make:table Users
```

You'll find the new Table class in the `app/Tables` folder. You may use this class in the controller instead of the *inline* instance:

```php
use App\Tables\Users;

return view('users.index', [
    'users' => SpladeTable::for(User::class)      // [tl! remove]
        ->column('name')      // [tl! remove]
        ->column('email')      // [tl! remove]
        ->paginate(15),      // [tl! remove]

    'users' => Users::class,      // [tl! add]
]);
```

The class consists of three methods you'll need to implement: `authorize`, `for`, and `configure`.

### Implementing the `for` method

If you create a new Table class, the `for` method has a default implementation. It returns a new query builder of the corresponding Eloquent Model:

```php
public function for()
{
    return User::query();
}
```

You may add additional constraints and apply scopes to query:

```php
public function for()
{
    return User::query()->where('is_admin', 0);
}
```

Note that this will apply to all results. So, for example, if you want to choose in the frontend between *admins* and *non-admins*, this is not the right place to use the constraint.

### Implementing the `configure` method

The `configure` method gives you an instance of `SpladeTable`. Just like the *inline* example, here you may configure the columns, filters, pagination, search inputs, and more:

```php
public function configure(SpladeTable $table)
{
    $table
        ->column('name')
        ->column('email')
        ->paginate(15);
}
```

### Implementing the `authorize` method

The `authorize` method is only used for Bulk Actions and Exports. Just like [Form Requests](https://laravel.com/docs/9.x/validation#authorizing-form-requests), you may determine if the user has the authority to perform such actions:

```php
public function authorize(Request $request)
{
    return $request->user()->is_admin;
}
```

Instead of always returning `true`, you may also remove the method if you don't want to use authorization.

## Pagination

When the dataset is paginated, it will, by default, show a select dropdown to customize the number of rows per page. In addition, you may define a custom set of options using the `perPageOptions` method on the `SpladeTable`:

```php
SpladeTable::for(User::class)
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

You may use slots if you want to write the head or body yourself.

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


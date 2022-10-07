# Table with Laravel Query Builder

Besides rendering the head, body and pagination of table, the component supports many other features that goes great hand in hand with Spatie's Laravel Query Builder package.

## Search Fields

With the `searchInput` method, you can specify which attributes are searchable. Search queries are passed to the URL query as a filter. This integrates seamlessly with the [filtering feature](https://spatie.be/docs/laravel-query-builder/v5/features/filtering) of the Laravel Query Builder package.

Though it's enough to pass in the column key, you may specify a custom label.

```php
SpladeTable::for($users)
    ->searchInput('name')
    ->searchInput(
        key: 'framework',
        label: 'Find your framework',
    );
```

## Select Filters

Select Filters are similar to search fields but use a `select` element instead of an `input` element. This way, you can present the user a predefined set of options. Under the hood, this uses the same filtering feature of the Laravel Query Builder package.

The `selectFilter` method requires two arguments: the key, and a key-value array with the options.

```php
SpladeTable::for($users)
    ->selectFilter('language_code', [
        'en' => 'English',
        'nl' => 'Dutch',
    ]);
```

The `selectFilter` will, by default, add a *no filter* option to the array. You may disable this or specify a custom label for it.

```php
SpladeTable::for($users)
    ->selectFilter(
        key: 'language_code',
        options: $languages,
        label: 'Language',
        noFilterOption: true
        noFilterOptionLabel: 'All languages'
    );
```


## Columns

With the `column` method, you can specify which columns you want to be toggleable, sortable, and searchable. You must pass in at least a key or label for each column.

```php
SpladeTable::for($users)
    ->column('email', 'User Email')

    ->column(
        key: 'name',
        label: 'User Name',
        canBeHidden: true,
        hidden: false,
        sortable: true,
        searchable: true
    );
```

By default, the `canBeHidden` value is set to `true`, which makes every column toggleable by default. You may change this behaviour by using the static `defaultColumnCanBeHidden` method on the `SpladeTable` class, for example, in the `AppServiceProvider` class:

```php
SpladeTable::defaultColumnCanBeHidden(false);
```

The `searchable` boolean is a shortcut to the `searchInput` method. The example below will essentially call `$table->searchInput('name', 'User Name')`.

## Global Search

You may enable Global Search with the `withGlobalSearch` method, and optionally specify a placeholder.

```php
SpladeTable::for($users)
    ->withGlobalSearch();

SpladeTable::for($users)
    ->withGlobalSearch('Search through the data...');
```

If you want to enable Global Search for every table by default, you may use the static `defaultGlobalSearch` method, for example, in the `AppServiceProvider` class:

```php
SpladeTable::defaultGlobalSearch();
SpladeTable::defaultGlobalSearch('Default custom placeholder');
SpladeTable::defaultGlobalSearch(false); // disable
```

## Example controller

```php
<?php

namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Support\Collection;
use ProtoneMedia\Splade\SpladeTable;
use Spatie\QueryBuilder\AllowedFilter;
use Spatie\QueryBuilder\QueryBuilder;

class UserIndexController
{
    public function __invoke()
    {
        $globalSearch = AllowedFilter::callback('global', function ($query, $value) {
            $query->where(function ($query) use ($value) {
                Collection::wrap($value)->each(function ($value) use ($query) {
                    $query
                        ->orWhere('name', 'LIKE', "%{$value}%")
                        ->orWhere('email', 'LIKE', "%{$value}%");
                });
            });
        });

        $users = QueryBuilder::for(User::class)
            ->defaultSort('name')
            ->allowedSorts(['name', 'email', 'language_code'])
            ->allowedFilters(['name', 'email', 'language_code', $globalSearch])
            ->paginate(request()->query('perPage', 15))
            ->withQueryString();

        return view('users.index', [
            'users' => SpladeTable::for($users),
                ->withGlobalSearch()
                ->defaultSort('name')
                ->column(key: 'name', searchable: true, sortable: true, canBeHidden: false)
                ->column(key: 'email', searchable: true, sortable: true)
                ->column(key: 'language_code', label: 'Language')
                ->column(label: 'Actions')
                ->selectFilter(key: 'language_code', label: 'Language', options: [
                    'en' => 'English',
                    'nl' => 'Dutch',
                ]);
        ]);
    }
}
```

### Custom column cells

When using auto-fill, you may want to transform the presented data for a specific column while leaving the other columns untouched. For this, you may use a cell template. This example is taken from the Example Controller above.

```blade
<x-splade-table :for="$users">
    @cell('actions', $user)
        <a href="/users/{{ $user->id }}/edit"> Edit </a>
    @endcell
</x-splade-table>
```

## Row Link

You may use the `rowLink` method to make one entire row clickable.

```php
SpladeTable::for($users)
    ->rowLink(fn (User $user) => route('users.edit', ['id' => $user->id]))
```

## Debounce

The filter and selects elements have a default debounce time of 350ms. You may customize this:

```blade
<x-splade-table :for="$users" search-debounce="500" />
```

You may also set it globally using the static `defaultSearchDebounce` method, for example, in the `AppServiceProvider` class:

```php
SpladeTable::defaultSearchDebounce(750);
```

## Striped table

You may use the `striped` attribute to add a striped layout to the table.

```blade
<x-splade-table :for="$users" striped />
```
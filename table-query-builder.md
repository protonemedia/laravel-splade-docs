# Table with the built-in Query Builder

Besides rendering the head, body, and pagination, the component supports many other features like filtering, searching, sorting, and more. The built-in Query Builder has been tested with MySQL, PostgreSQL and SQLite.

## Search Fields

With the `searchInput` method, you can specify which attributes are searchable. Search queries are passed to the URL query as a filter.

Though it's enough to pass in the column key, you may specify a custom label.

```php
$table
    ->searchInput('name')
    ->searchInput(
        key: 'framework',
        label: 'Find your framework',
    );
```

## Select Filters

Select Filters are similar to search fields but use a `select` element instead of an `input` element. This way, you can present the user a predefined set of options.

The `selectFilter` method requires two arguments: the key and a key-value array with the options.

```php
$table->selectFilter('language_code', [
    'en' => 'English',
    'nl' => 'Dutch',
]);
```

The `selectFilter` will, by default, add a *no filter* option to the array. You may disable this or specify a custom label for it.

```php
$table->selectFilter(
    key: 'language_code',
    options: $languages,
    label: 'Language',
    noFilterOption: true,
    noFilterOptionLabel: 'All languages'
);
```

## Columns

With the `column` method, you can specify which columns you want to be toggleable, sortable, and searchable. You must pass in at least a key or label for each column.

```php
$table
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

By default, the `canBeHidden` value is set to `true`, making every column toggleable. However, you may change this behavior by using the static `defaultColumnCanBeHidden` method on the `SpladeTable` class, for example, in the `AppServiceProvider` class:

```php
SpladeTable::defaultColumnCanBeHidden(false);
```

The `searchable` boolean is a shortcut to the `searchInput` method. The example above will essentially call `$table->searchInput('name', 'User Name')`.

### Transform values

Sometimes, you want to transform the value in the table without using a custom column cell. You may do this by providing a callback to the `as` argument. The callback takes two arguments: the original value and the item itself (typically the Eloquent Model).

```php
$table->column(
    key: 'email',
    as: fn ($email, $user) => Str::mask($email)
);
```

### Column alignment

You may change the alignment of the column by setting it the `left` (default), `center`, or `right`.

```php
$table->column(
    key: 'actions',
    alignment: 'right'
);
```

### Default sort

You may configure the default sorting with the `defaultSort()` method:

```php
$table->defaultSort('name');
```

If you want to sort in descending order, you may prefix the key with a `-` character or use the `defaultSortDesc` method. Alternatively, you may use the `defaultSort` method with an additional second argument:

```php
$table->defaultSort('-name');

$table->defaultSortDesc('name');

$table->defaultSort('name', 'desc');
```

### Sort by Relationship column

The Table component supports sorting the results by a [Relationship](https://laravel.com/docs/10.x/eloquent-relationships) column. This requires the installation of the [`kirschbaum-development/eloquent-power-joins`](https://github.com/kirschbaum-development/eloquent-power-joins) package.

```php
$table->column(
    key: 'user.organization.name',
    label: 'Organization',
    sortable: true
);
```

## Global Search

You may enable Global Search with the `withGlobalSearch` method, and optionally specify a placeholder.

```php
$table->withGlobalSearch(columns: ['name', 'email']);
```

```php
$table->withGlobalSearch('Search through the data...', ['name', 'email']);
```

## Example Table

```php
<?php

namespace App\Tables;

use App\Models\User;
use ProtoneMedia\Splade\AbstractTable;
use ProtoneMedia\Splade\SpladeTable;

class Users extends AbstractTable
{
    public function for()
    {
        return User::query();
    }

    public function configure(SpladeTable $table)
    {
        $table
            ->withGlobalSearch(columns: ['name', 'email'])
            ->defaultSort('name')
            ->column(key: 'name', searchable: true, sortable: true, canBeHidden: false)
            ->column(key: 'email', searchable: true, sortable: true)
            ->column(key: 'language_code', label: 'Language')
            ->column(label: 'Actions')
            ->selectFilter(key: 'language_code', label: 'Language', options: [
                'en' => 'English',
                'nl' => 'Dutch',
            ])
            ->paginate(15);
    }
}
```

### Custom column cells

When using auto-fill, you may want to transform the presented data for a specific column while leaving the other columns untouched. For this, you may use a `cell` component. This example is taken from the Example Table above.

```blade
<x-splade-table :for="$users">
    <x-splade-cell actions>
        <Link href="/users/{{ $item->id }}/edit"> Edit </Link>
    </x-splade-cell>
</x-splade-table>
```

If you want to change the `$item` variable, you may use use the `as` attribute on the component:

```blade
<x-splade-cell actions as="$user">
    <Link href="/users/{{ $user->id }}/edit"> Edit </Link>
</x-splade-cell>
```

This is a *scoped* component, so variables outside the slot won't be available by default. If you still want to use a variable, you may use the `use` attribute:

```blade
@php $message = 'Hello, world!'; @endphp

<x-splade-cell actions as="$user" use="$message">
    <p> {{ $message }} </p>
    <Link href="/users/{{ $user->id }}/edit"> Edit </Link>
</x-splade-cell>
```

When your template uses multiple custom cells, you may set these variables as defaults on the parent table:

```blade
<x-splade-table :for="$users" as="$user">
    <x-splade-cell name>
        <p class="text-red-500"> {{ $user->name }}</p>
    </x-splade-cell>

    <x-splade-cell actions>
        <Link modal href="/users/{{ $user->id }}/edit"> Edit </Link>
    </x-splade-cell>
</x-splade-table>
```

*Note the `modal` attribute on the Link element in the example above. Instead of navigating to another page, you may open a [Modal or Slideover](/x-modal.md) from a Table.*

## Row Link

You may use the `rowLink` method to make one entire row clickable.

```php
$table->rowLink(fn (User $user) => route('users.edit', ['id' => $user->id]))
```

If you want to open the URL in a [Modal or Slideover](/x-modal.md), you may use the `rowModal` or `rowSlideover` method.

## Debounce

The filter and select elements have a default debounce time of 350ms. You may customize this:

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

## Reset button

By default, a *Reset-button* is rendered when the table is filtered, sorted, or searched. You may disable this button by setting the `reset-button` attribute to `false`:

```blade
<x-splade-table :for="$users" :reset-button="false" />
```

You may also disable it globally using the static `defaultResetButton` method, for example, in the `AppServiceProvider` class:

```php
SpladeTable::defaultResetButton(false);
```

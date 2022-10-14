# Table with Spatie's Laravel Query Builder

The Table component works seamlessly with Spatie's Laravel Query Builder package. You may use both an *inline* SpladeTable instance, as well as a dedicated Table class. Note that a Table class is required in order to use Bulk Actions and Exports.

## Example with Table Instance

```php
<?php

namespace App\Tables;

use App\Models\User;
use Illuminate\Support\Collection;
use ProtoneMedia\Splade\AbstractTable;
use ProtoneMedia\Splade\SpladeTable;
use Spatie\QueryBuilder\AllowedFilter;
use Spatie\QueryBuilder\QueryBuilder;

class Users extends AbstractTable
{
    public function for()
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

        return QueryBuilder::for(User::class)
            ->defaultSort('name')
            ->allowedSorts(['name', 'email', 'language_code'])
            ->allowedFilters(['name', 'email', 'language_code', $globalSearch]);
    }

    public function configure(SpladeTable $table)
    {
        $table
            ->withGlobalSearch()
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

## Example controller with Inline Table

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
            ->allowedFilters(['name', 'email', 'language_code', $globalSearch]);

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
                ])
                ->paginate(15),
        ]);
    }
}
```


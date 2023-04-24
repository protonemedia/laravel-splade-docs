---
description: The Splade Table component supports Bulk Actions. It can perform the action on the selected rows, on all rows on the current page, or on all rows on all pages.
keywords: laravel datatables bulk actions, laravel datatables actions, laravel table bulk action, laravel tables bulk action
---

# Table Bulk Actions

The Table component supports performing Bulk Actions. First, you must register a supporting route using the `spladeTable()` method on the `Route` facade. As of version 0.6, the automatic installer does this for you. If you need to register the route manually, make sure it uses the `web` and `splade` Middleware, for example, in `web.php`:

```php
Route::middleware('splade')->group(function () {
    Route::spladeTable();
});
```

## Configure a Bulk Action

You may configure a Bulk Action on the `SpladeTable` instance:

```php
public function configure(SpladeTable $table)
{
    $table->bulkAction('Touch timestamp', function (Project $project) {
        $project->touch();
    });
}
```

The `bulkAction` method has additional `before` and `after` arguments. You may use this to show a [Toast](/toasts.md) when the action has finished, or for example, to perform some logging:

```php
$table->bulkAction(
    label: 'Touch timestamp',
    each: fn (Project $project) => $project->touch(),
    before: fn () => info('Touching the selected projects'),
    after: fn () => Toast::info('Timestamps updated!')
);
```

The `before` and `after` callbacks receive the selected rows as an argument. You may use this to perform additional actions, for example, to send a notification to the selected users:

```php
$table->bulkAction(
    label: 'Notify users',
    before: function (array $selectedIds) {
        $users = User::whereIn('id', $selectedIds)->get();

        Mail::to($users)->send(new ImportantNotification);
    }
);
```

## Confirmation

You may use the `confirm` argument to show a confirmation dialog before Splade performs the action:

```php
$table->bulkAction(
    label: 'Touch timestamp',
    each: fn (Project $project) => $project->touch(),
    confirm: true
);
```

In addition, you may customize the confirmation dialog:

```php
$table->bulkAction(
    label: 'Touch timestamp',
    each: fn (Project $project) => $project->touch(),
    confirm: 'Touch projects',
    confirmText: 'Are you sure you want to touch the projects?',
    confirmButton: 'Yes, touch all selected rows!',
    cancelButton: 'No, do not touch!',
);
```

### Password Confirmation

It's even possible to require the user to confirm their password within the confirmation dialog. First, you must register a supporting route using the `spladePasswordConfirmation()` method on the `Route` facade. As of version 1.2.2, the automatic installer does this for you. If you need to register the route manually, make sure it uses the `web` Middleware, for example, in `web.php`:

```php
Route::spladePasswordConfirmation();
```

Now you may set the `requirePassword` argument to `true`:

```php
$table->bulkAction(
    label: 'Delete projects',
    each: fn (Project $project) => $project->delete(),
    confirm: true,
    requirePassword: true
);
```

## Authorization

Just like [Form Requests](https://laravel.com/docs/10.x/validation#authorizing-form-requests), you may use the `authorize` method to determine if the user has the authority to perform a Bulk Action:

```php
class Projects extends AbstractTable
{
    public function authorize(Request $request)
    {
        return $request->user()->is_admin;
    }
}
```

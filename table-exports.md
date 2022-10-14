# Table Exports

The Table component supports Exports. First, you must register a supporting route using the `spladeTable()` method on the `Route` facade. As of version 0.6, the automatic installer does this for you. If you need to register the route manually, make sure it uses the `web` and `splade` Middleware, for example, in `web.php`:

```php
Route::middleware('splade')->group(function () {
    Route::spladeTable();
});
```

Lastly, you need to install the [`maatwebsite/excel`](https://github.com/SpartnerNL/Laravel-Excel) package. Read the installation documentation carefully. If you want to export to PDF, you must install [additional libraries](https://docs.laravel-excel.com/3.1/exports/export-formats.html).

## Configure an Export

You may enable exporting with the `export()` method on the `SpladeTable` instance. It will configure an Excel Export by default.

```php
public function configure(SpladeTable $table)
{
    $table->export();
}
```

Of course, you may customize the export or add multiple exports.

```php
use Maatwebsite\Excel\Excel;

public function configure(SpladeTable $table)
{
    $table->export(
        label: 'CSV export',
        filename: 'projects.csv',
        type: Excel::CSV
    );
}
```

## Customizing the export

The `column` method has additional arguments you may use to customize the export. With the `exportAs` argument, you may transform the value or exclude the column from the export:

```php
$table
    ->column('name')
    ->column('email', exportAs: false)
    ->column('reference', exportAs: fn ($reference) => "#REF-{$reference}");
```

The `exportFormat` allows you to [format](https://docs.laravel-excel.com/3.1/exports/column-formatting.html) the column, for example, to indicate that the column represents a currency:

```php
use PhpOffice\PhpSpreadsheet\Style\NumberFormat;

$table->column('amount', NumberFormat::FORMAT_CURRENCY_EUR_SIMPLE);
```

Lastly, with the `exportStyling` method, you may style the column with an array or a callable:

```php
use PhpOffice\PhpSpreadsheet\Style\Style;

$table
    ->column('name', exportStyling: ['font' => ['bold' => true]])
    ->column('email', exportStyling: function (Style $style) {
        $style->getFont()->setBold(true);
    });
```

## Authorization

Just like [Form Requests](https://laravel.com/docs/9.x/validation#authorizing-form-requests), you may use the `authorize` method to determine if the user has the authority to perform a Bulk Action:

```php
class Projects extends AbstractTable
{
    public function authorize(Request $request)
    {
        return $request->user()->is_admin;
    }
}
```
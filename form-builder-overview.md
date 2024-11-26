---
description: Splade has an advanced Form Builder that enables you to build forms from your controllers instead of in your templates.
keywords: laravel form, laravel forms, laravel formbuilder
---

# Form Builder

Splade has an advanced Form Builder that enables you to build forms from your controllers instead of in your templates. Check out the [Form Components](/form-overview.md) section to learn more about Splade's form capabilities.

## Basic example

You may use the `SpladeForm` class to configure the form.

```php
<?php

namespace App\Http\Controllers;

use ProtoneMedia\Splade\FormBuilder\Input;
use ProtoneMedia\Splade\FormBuilder\Password;
use ProtoneMedia\Splade\FormBuilder\Submit;
use ProtoneMedia\Splade\SpladeForm;

class UsersController
{
    public function create()
    {
        $form = SpladeForm::make()
            ->action(route('users.store'))
            ->fields([
                Input::make('name')->label('User Name'),
                Password::make('password')->label('Password'),
                Submit::make()->label('Create'),
            ]);

        return view('users.create', [
            'form' => $form,
        ]);
    }
}
```

Then in the Blade template, you must pass the form to the `for` attribute. That's it!

```blade
<x-splade-form :for="$form" />
```

Besides the `action()` and `fields()` methods, there are additional methods to configure the form:

```php
SpladeForm::make()
    ->id('create-user-form')
    ->class('space-y-4')
    ->method('POST');
```

To show a confirmation dialog before Splade submits the form, simply use the `confirm()` method. You may customize the confirmation dialog as well.

```php
SpladeForm::make()->confirm();

SpladeForm::make()->confirm(
    confirm: 'Delete user',
    text: 'Are you sure you want to delete this user?',
    confirmButton: 'Delete',
    cancelButton: 'Cancel',
    danger: true,
    requirePassword: true,
);
```

Instead of requiring a password using the `confirm()` method, you may also use the dedicated `requirePassword()` method:

```php
SpladeForm::make()->requirePassword();

SpladeForm::make()->requirePassword(
    requirePasswordOnce: false,
    heading: 'Please enter your password',
    text: 'Please confirm your password before continuing',
    confirmButton: 'Confirm',
    cancelButton: 'Cancel',
    danger: false
);
```

### Passing default data

You may use the `fill` method to provide a set of default data to the form:

```php
$project = Project::firstOrFail();

SpladeForm::make()
    ->fields([...])
    ->fill($project);
```

Note that the Eloquent attributes are passed to the frontend, so be careful with sensitive attributes. By default, it only passes the attributes to the frontend that you use in the fields. You may use a [Transformer](/transformers.md) to help you safely pass data to the frontend. Also, check out the [Model Binding Attributes](/form-model-binding-attributes.md) section.

## Form Class

Instead of using an *inline* `SpladeForm` in the controller, you may also use a dedicated class. There's an Artisan command to generate a Form class:

```bash
php artisan make:form CreateUserForm
```

You'll find the new Form class in the `app/Forms` folder. You may use this class in the controller instead of the *inline* instance:

```php
use App\Forms\CreateUserForm;

return view('users.create', [
    'form' => $form,      // [tl! remove]
    'form' => CreateUserForm::class,      // [tl! add]
]);
```

The generated `CreateUserForm` will have a `configure()` and `fields()` method:

```php
<?php

namespace App\Forms;

use ProtoneMedia\Splade\AbstractForm;
use ProtoneMedia\Splade\FormBuilder\Input;
use ProtoneMedia\Splade\FormBuilder\Password;
use ProtoneMedia\Splade\FormBuilder\Submit;
use ProtoneMedia\Splade\SpladeForm;

class CreateUserForm extends AbstractForm
{
    public function configure(SpladeForm $form)
    {
        $form->action(route('users.store'));
    }

    public function fields(): array
    {
        return [
            Input::make('name')->label('User Name'),
            Password::make('password')->label('Password'),
            Submit::make()->label('Create'),
        ];
    }
}
```

### Validation

You may pass validation rules to the fields using the `rules()` method:

```php
Input::make('name')->rules('required', 'max:255');

// or:

Input::make('name')->rules(['required', 'max:255']);

// or:

Input::make('name')->rules('required|max:255');
```

There's a `required()` helper method that adds the *required* rule to the field:

```php
Input::make('name')->required();
```

You may gather all rules using the static `rules()` method on the form class, which you can use in the controller to validate an incoming request:

```php
public function store(Request $request)
{
    $data = $request->validate(CreateUserForm::rules());
}
```

Alternatively, you may call the `validate()` method on the form instance. This works great with a type-hinted form variable:

```php
public function store(Request $request, CreateUserForm $form)
{
    $data = $form->validate($request);
}
```

### Asterisk on required fields

Splade can automatically add an asterisk to required fields. You may enable this feature by setting the `blade.asterisk_on_required_form_elements` key in the `splade.php` [configuration file](/customization.md) to `true`.

### Using Form Requests

Instead of validating the request in the controller, you may also use Laravel's [Form Requests](https://laravel.com/docs/10.x/validation#form-request-validation) feature. To may use the form's validation rules in the Form Request, and to make it even more seamless, there's an Artisan command to generate a Form Request class:

```bash
php artisan make:form-request CreateUserFormRequest --form=CreateUserForm
```

Alternatively, you can generate both at once using `-r` or `--request` option on the `make:form` command:

```bash
php artisan make:form CreateUserForm --request
```

This will generate a Form Request with the `rules()` method already implemented:

```php
<?php

namespace App\Http\Requests;

use App\Forms\CreateUserForm;
use Illuminate\Foundation\Http\FormRequest;

class CreateUserFormRequest extends FormRequest
{
    public function rules()
    {
        return CreateUserForm::rules();
    }
}
```

Now this class can be used as a regular FormRequest:

```php
public function store(CreateUserFormRequest $request)
{
    $data = $request->validated();
}
```

### Extending Form classes

Instead of providing the class name, you may also use an instance of the form using the `make()` method. This allows you to add additional fields to the form:

```php
return view('users.create', [
    'form' => CreateUserForm::make()->fields([
        Text::make('additional_field')->label('This is an additional field'),
    ]),
]);
```

## Extending Form Fields

All Form Fields are *macroable*, allowing you to easily add new methods:

```php
use ProtoneMedia\Splade\FormBuilder\Input;

Input::macro('autocomplete', function ($value) {
    return $this->attributes(['autocomplete' => $value]);
});

Input::make('password')->autocomplete('current-password');
```

## Frontend behavior

In addition to configuring the form, fields, rules, and filled data, you may also configure its frontend behavior:

### Prevent navigation on submit

You may instruct Splade to stay on the same page after a successful request:

```php
SpladeForm::make()->stay();
```

In additional, you may specify to reset or restore the form on success:

```php
SpladeForm::make()->stay(actionOnSuccess: 'reset');
SpladeForm::make()->stay(actionOnSuccess: 'restore');
```

### Preserve Scroll

When you stay on the same page after a succesful request, you may prevent the page from scrolling to the top:

```php
SpladeForm::make()->stay()->preserveScroll();
```

### Submit on change

You may watch form values, and automatically submit the form on changes.

```php
SpladeForm::make()->submitOnChange(watchFields: 'theme');

// or:

SpladeForm::make()->submitOnChange(watchFields: ['theme', 'scale']);
```

You may omit the fields to watch all fields, as well as customize the debounce value and performing the request in the background:

```php
SpladeForm::make()->submitOnChange();
SpladeForm::make()->submitOnChange(background: true);
SpladeForm::make()->submitOnChange(debounce: 1000);
```

> **Warning**
> The `submitOnChange()` method will not show a confirmation or password dialog.
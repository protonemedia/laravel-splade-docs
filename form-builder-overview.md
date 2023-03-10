---
description: Splade has an advanced Form Builder that enables you to build forms from your controllers instead of in your templates.
keywords: laravel form, laravel forms, laravel formbuilder
---

# Form Builder

Splade has an advanced Form Builder that enables you to build forms from your controllers instead of in your templates.

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

### Passing default data

You may use the `fill` method to provide a set of default data to the form:

```php
$project = Project::firstOrFail();

SpladeForm::make()
    ->fields([...])
    ->fill($project);
```

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

You may gather all rules using the static `rules()` method on the form class:

```php
$rules = CreateUserForm::rules();
```

You can use this in the controller to validate in an incoming request:

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

### Extending Form classes

Instead of providing the class name, you may also use an instance of the form using the `make()` method. This allows you to add additional fields to the form:

```php
return view('users.create', [
    'form' => CreateUserForm::make()->fields([
        Text::make('additional_field')->label('This is an additional field'),
    ]),
]);
```

## Other inputfield options



### Hiding or showing fields
A `v-if="..."`-condition can be added with the `->if('...')` option:
```php
    // Adds v-if="modal" / v-if="!modal"
    ->if('modal')           // The field will only be visible when the form is loaded within a modal
    ->if('!modal')          // or when not in a modal

    // Only show a field when another field is not empty or checked:
    ->if("form.otherFieldsName != ''")
```

### Appends, prepends, placeholders, disabled and readonly
```php
    ->append('Text')        // Adds an append text to an input
    ->prepend('Text')       // Adds a prepend text to an input
    ->placeholder('Text')   // Adds a placeholder to the input
    ->disabled()            // Makes an input disabled
    ->readonly()            // Makes an input readonly
```
These options may be combined.

### Classes
Classes can be added to both the form and to the input fields:
```php
    SpladeForm::build()
        ->action(...)
        ->name('form1')
        ->class('space-y-4') // One or more classes may be added to a form...
        ->fields([
             // or to any field:
            Submit::make()
                ->class('text-white bg-blue-600 hover:bg-blue-700 rounded-full')
                ->label('Send'),
         ])
```

## Confirmation
You may use the confirm option to show a confirmation dialog before the form is submitted:
```php
    ->confirm()
    // or add one or more options:
    ->confirm(
        confirm: 'Delete profile',
        text: 'Are you sure you want to delete your profile?',
        confirm_button: 'Yes, delete everything!',
        cancel_button: 'No, I want to stay!',
        danger: true,               // If true, the conformation modal will render a red confirmation button
        require_password: true,     // Require the user to confirm their password within the confirmation dialog
        require_password_once: true // Prevents users from re-entering their password over and over
    )
```

## Password Confirmation
For the password confirmation dialog the `->requirePassword(...)`-alias may be used instead of `->confirm(require_password: true)`:
```php
    ->requirePassword(
        require_password_once: true,
        heading: 'Please enter your password',
        text: 'Please confirm your password before continuing',
        confirm_button: 'Confirm',
        cancel_button: 'Cancel',
        danger: false
    )
```

## Submit-on-change
Submit the form whenever a value changes:
```php
    ->submitOnChange(
        // watch_fields are optional, all formfields will be watched if none are provided
        watch_fields: ['fieldname1', 'fieldname2'], // or as a string: 'fieldname1, fieldname2'

        // These defaults may be overwritten:
        background: true,
        debounce: 500
    )
```
_Be carefull when combining `->submitOnChange()` with `->confirm()` or `->requirePassword()` because it will save the data in the background without the confirmation!_

## Prevent navigation on submit
Stay on the same page after a successful request by using one of these options:
```php
    ->stay()
    ->stay(action_on_success: 'reset')      // Reset on success
    ->stay(action_on_success: 'restore')    // Restore on success
```

## Preserve-scroll
The preserve-scroll attribute prevents the page from scrolling to the top:
```php
    ->preserveScroll()
```

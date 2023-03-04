---
description: Splade has an advanced FormBuilder component that enables you to build forms from your controllers without writing any HTML.
keywords: laravel form, laravel forms, laravel formbuilder
---

# FormBuilder Component

Splade has an advanced FormBuilder component that enables you to build forms from your controllers without writing any HTML.

## Basic example

You may use the `SpladeForm` class to configure the form in your controller.

```php
<?php

namespace App\Http\Controllers;

use ProtoneMedia\Splade\Components\FormBuilder;
use ProtoneMedia\Splade\SpladeForm;
use Illuminate\Http\Request;

class ExampleController extends Controller
{
    public function create(): View
    {
        return view('formbuilder', [
            'example' => SpladeForm::build()
                ->action(route('formbuilder.store'))
                ->name('exampleform')
                ->method('POST')
                ->class('space-y-4')
                ->fields([
                    FormBuilder\Input::make('inputText')
                        ->label('Standard input text field')
                        ->help('Test help 1')
                        ->rules('required'),
        
                    FormBuilder\Password::make('inputPassword')
                        ->label('Password field')
                        ->rules('required', 'string', 'max:255'),
        
                    FormBuilder\Textarea::make('testTextarea2')
                        ->label('Textarea (with autosize)')
                        ->autosize()
                        ->rules(['required', 'string', 'max:255']),
        
                    FormBuilder\Submit::make()
                        ->label('Send'),
                ])
                ->data([
                    'inputText' => 'Test value input text field 1',
                ]),
        ]);
    }
    
    public function store(Request $request)
    {
        $rules = SpladeForm::build()->name('exampleform')->getRules();

        dd($request->validate($rules));
    }
}
```

All you need In the `formbuilder.blade.php` is:

```blade
    <x-splade-formbuilder :for="$example" />
```

That's all! It will automatically render the form with the fields, classes, method and route.
For the back-end validation all provided `->rules(...)` will be extracted from the fields.

## FormBuilder Class

Instead of using an *inline* `SpladeForm` in the controller, you may create a dedicated FormBuilder class. 
A FormBuilder class lets you keep the configuration out of the controller and reuse the FormBuilder class.

There's an Artisan command to create a FormBuilder class:

```bash
php artisan make:form ExampleForm
```

You'll find the new Form class in the `app/Forms` folder. You may use this class in the controller instead of the *inline* instance:

```php
use App\Forms\ExampleForm;

return view('formbuilder', [
    'example' => SpladeForm::build()                   // [tl! remove]
        ->action(route('formbuilder.store'))           // [tl! remove]
        ->name('exampleform')                          // [tl! remove]
        ->fields([                                     // [tl! remove]
            // ...                                        [tl! remove]
            FormBuilder\Submit::make()->label('Send'), // [tl! remove]
        ])                                             // [tl! remove]
    'example' => ExampleForm::class,      // [tl! add]
]);
```

The generated ExampleForm will look like:

```php
<?php

namespace App\Forms;

use ProtoneMedia\Splade\AbstractForm;
use ProtoneMedia\Splade\Components\FormBuilder\Submit;
use ProtoneMedia\Splade\Components\FormBuilder\Text;
use ProtoneMedia\Splade\SpladeForm;

class ExampleForm extends AbstractForm
{
    public function configure(SpladeForm $form)
    {
        $form
            ->action(route('...'))
            ->method('POST')
            ->name('exampleform')
            ->class('space-y-4')
            ->data([
                //
            ]);
    }

    public function fields(): array
    {
        return [
            Text::make('...')
                ->label(__('...')),

            //

            Submit::make()
                ->label(__('Save')),
        ];
    }
}
```

## Validation with the FormBuilder FormRequest Class

There's an Artisan command to create a FormBuilder class as well:

```bash
php artisan make:form-request ExampleFormRequest --form=ExampleForm
```

Both can be generated together by providing the `-r` or `--request` option:

```bash
php artisan make:form ExampleForm --request
```

A Splade FormBuilder FormRequest may look like:

```php
<?php

namespace App\Http\Requests;

use App\Forms\ExampleForm;
use Illuminate\Foundation\Http\FormRequest;

class ExampleFormRequest extends FormRequest
{
    public function rules()
    {
        return ExampleForm::rules('exampleform');
    }
}
```

and the store-method:

```php
public function store(ExampleFormRequest $request)
{
    $validated = $request->validated();

    dd($validated);
}
```
That's all that is needed for the formvalidation! All rulles will be extracted from the `->rules(...)` in the fields of your form.

## Passing data to a FormBuilder form

Default formdata may be passed in the `$form->data([...])` array. The keys of this array correspond to the fieldnames as provided in the `make('...')`s.

Using Eloquent Models is possible as well: `SpladeForm::build()->data(Post::first())`.

## Multiple forms on the same page

Adding multiple forms to the same page is possible, even from the same FormBuilder-class.
All options like action, method, classes and name (in the `->build('...')`-method) may be overwritten and additional fields can be added:
```php
return view('form.formbuilder', [
    'forms' => [
        'example1' => FormClass::class,
        'example2' => FormClass::build('form2')
            ->action(route('form2.store'))
            ->fields([
                Text::make('additional_field')->label('This is an additional field'),
            ]),
    ],
]);
```
And the blade:
```blade
<x-splade-formbuilder :for="$form['example1']" />
<x-splade-formbuilder :for="$form['example2']" />

{{-- or: --}}
    
@foreach($forms as $form)
    <x-splade-formbuilder :for="$form" />
@endforeach
```

## Available fields

At this moment there is support for the fields types as shown below in the `ProtoneMedia\Splade\Components\FormBuilder` namespace.

*Most of the display options are optional.*

### Hidden
```php
    Hidden::make('hidden1'),
    // or:
    Input::make('hidden2')->hidden(),
```

### Text
```php
    Input::make('text')
        ->label('Standard input text field')
        ->rules('required', 'max:255') // or: ->rules(['required', 'max:255']) or: ->rules('required|max:255')
        ->help('Help text') 
        ->placeholder('Placeholder...')
        ->minLength(2)
        ->maxLength(255),
    // or:
    Text::make('textField')
        ->label('Text-input (Input-alias)'),
```

### Textarea
```php
    Textarea::make('textarea')
        ->label('Textarea')
        ->autosize(),
```

### Number
```php
    Number::make('number')
        ->label('Number input')
        ->minValue(1)
        ->maxValue(100)
        ->unsigned(), // alias for ->minValue(0)
        ->step(0.01),
```

### Email
```php
    Email::make('email1')
        ->label('Email'),
    // or:
    Input::make('email2')
        ->label('Input ->email() field')
        ->email(),
```

### Passwords
```php
    Password::make('password1')
        ->label('Password field with validation: ->min(8) and ->symbols()')
        ->rules('required', 'string', 'max:255', \Illuminate\Validation\Rules\Password::min(8)->symbols()),
    // or:
    Input::make('password2')
        ->label('Input ->password() field')
        ->password(),
```

### Date
```php
    Date::make('date')
        ->label('Input type: date - with extra FlatPicker date-options: { showMonths: 2 }')
        ->date(['showMonths' => 2]),
```

### Time
```php
    Time::make('time')
        ->label('Input time field - with extra FlatPicker time-options: { time_24hr: false }')
        ->time(['time_24hr' => false])
```

### Datetime
```php

    Datetime::make('datetime1')
        ->label('Input type: date with time'),
    // or:
    Date::make('datetime2')
        ->label('Input type: date with time')
        ->time(),
```

### Files
```php
    File::make('file1')
        ->label('File field 1')
        ->multiple() // Enables selecting multiple files
        ->filepond() // Enables filepond
        ->server()   // Enables asynchronous uploads of files
        ->preview()  // Only available with ->filepond() option
        ->accept(['image/png', 'image/jpeg']) // or for only one type: ->accept('image/png')
        ->rules([
            'required',
            FileRule::image()->min(1024)->max(12 * 1024)
        ]),
```
there are several other options available for files:
```php
        ->minSize('1Mb')
        ->maxSize('10Mb')

        ->width(120)
        ->height(120)

        ->minWidth(150)
        ->maxWidth(500)

        ->minHeight(150)
        ->maxHeight(500)

        ->minResolution(150)
        ->maxResolution(99999)
```

### Checkboxes
Checkboxes can be defined as separate inputs:
```php
    Checkbox::make('checkbox[]')->label('Checkbox 1')->value('checkbox-1'),
    Checkbox::make('checkbox[]')->label('Checkbox 2')->value('checkbox-2'),
```
or at once:
```php
    Checkboxes::make('multicheckbox')
        ->label('MultiCheckbox')
        ->options([
            'option-1' => 'Option 1',
            'option-2' => 'Option 2',
            'option-3' => 'Option 3',
        ])
        ->inline(), // inline is optional here
```

### Radios
Radios can be defined as separate inputs:
```php
    Radio::make('radio')->label('Radio 1')->value('radio-1'),
    Radio::make('radio')->label('Radio 2')->value('radio-2'),
    Radio::make('radio')->label('Radio 3')->value('radio-3')->help('Test help 5'),
```
or at once:
```php
    Radios::make('multiradio')
        ->label('Choose a theme')
        ->options([
            'dark' => 'Dark theme',
            'light' => 'Light theme',
            'system' => 'System theme',
        ])
        ->inline(), // inline is optional here
```

### Selects
```php
    $options = [
            'be' => 'Belgium',
            'de' => 'Germany',
            'fr' => 'France',
            'lu' => 'Luxembourg',
            'nl' => 'The Netherlands',
        ];

    Select::make('select1')
        ->label('Choose a country')
        ->options($options)
        ->choices(false), // This disables Choices.js which is on by default

    Select::make('select2')
        ->label('Choose multiple countries')
        ->options($options)
        ->multiple(), // Enables choosing multiple options

    Select::make('select3[]')
        ->label('Choose multiple countries - with extra Choices.js-options: { searchEnabled: false }')
        ->options($options)
        ->multiple()
        ->choices(['searchEnabled' => false]),

    Select::make('selectFromRemoteUrl')
        ->label('Select with data from a remote URL')
        ->remoteUrl('/api/users1'),

    Select::make('selectFromRemoteUrlWithRemoteRoot')
        ->label('Select with data from a remote URL with a remote root')
        ->remoteUrl('/api/users2')
        ->remoteRoute('data.users')
        ->optionLabel('name')
        ->optionValue('id')),

    Select::make('selectWithOptionLabelAndValue')
        ->label('Select with Option Label and Value')
        ->options([
            ['id' => 10, 'name' => 'Pascal'],
            ['id' => 20, 'name' => 'Johan'],
            ['id' => 30, 'name' => 'Olaf'],
            ['id' => 40, 'name' => 'Kristof'],
        ])
        ->optionLabel('name')
        ->optionValue('id'),
```

### Colorpicker
```php
    Color::make('color1')
        ->label('Color::make()'),
    // or:
    Input::make('color2')
        ->label('Input ->color()')
        ->color(),
```

### Submit
```php
    Submit::make()->label('Send')
```

### Button
```php
    Button::make('button1')
        ->class('bg-green-500 text-white')
        ->click('modal.close')  // @click("modal.close")
        ->if('modal')           // v-if="modal"
//        ->danger()
//        ->secondary()
        ->label('Close modal'),
```

## Other inputfield options

### Rules
You may pass one or more [Laravel validation rules](https://laravel.com/docs/10.x/validation#available-validation-rules) to the `->rules(...)` option.
The following formats are supported:
```php
->rules('required|unique:posts|max:255')
->rules('required', 'unique:posts', 'max:255')
->rules(['required', 'unique:posts', 'max:255'])
->rules('required', 'string', 'max:255', \Illuminate\Validation\Rules\Password::min(8)->symbols()),
```

### Required
The `->required()` option is an alias for `->rules('required')`.

### Classes
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

### v-if
```php
    ->if('!modal')          // Adds v-if="!modal"
```

### Appends, prepends, placeholders, disabled and readonly
```php
->append('Text')        // Adds an append text to an input
->prepend('Text')       // Adds a prepend text to an input
->placeholder('Text')   // Adds a placeholder to the input
->disabled()            // Makes an input disabled
->readonly()            // Makes an input readonly
```
All these options may be combined.
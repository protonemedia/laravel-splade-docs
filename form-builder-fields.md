# Form Builder Fields

Here's an overview of all fields that are available in the [Form Builder](/form-builder-overview.md).

## Text Input Field

Renders an [Input Component](/form-input.md).

```php
Input::make('name')
    ->label('Enter your name')
    ->rules('required', 'max:255')
    ->help('I need somebody!')
    ->placeholder('John Doe')
    ->minLength(2)
    ->maxLength(255);

// or:

Text::make('name');
```

You may also define the validation rules as an array or a piped string:

```php
Input::make('text')
    ->rules(['required', 'max:255'])
    ->rules('required|max:255');
```

There are some additional methods that you may call on the field:

```php
Input::make('text')
    ->append('@splade.dev')    // Appends a text to the field
    ->prepend('www.')          // Prepends a text to the field
    ->placeholder('Text')      // Placeholder value for the field
    ->disabled()               // Disables an input field
    ->readonly()               // Makes an input read-only
    ->class('w-1/2')           // Add additional classes to the field

    ->attributes(['data-custom' => 'foo']);     // Add additional attributes
```

### Hidden Field

```php
Hidden::make('hidden_field');

// or:

Input::make('hidden_field')->hidden();
```

### Number

```php
Number::make('number')
    ->label('Number input')
    ->minValue(1)
    ->maxValue(100)
    ->unsigned(), // alias for ->minValue(0)
    ->step(0.01);
```

### Email

```php
Email::make('email');

// or:

Input::make('email')->email();
```

### Passwords

```php
Password::make('password');

// or:

Input::make('password')->password();
```

## Colorpicker

```php
Color::make('color');

// or:

Input::make('color')->color();
```

## Textarea

Renders a [Textarea Component](/form-textarea.md).

```php
Textarea::make('textarea')->autosize();
```

## Date and Time

Renders an [Input Component](/form-input.md) with Flatpickr.js integation.

### Date

```php
Date::make('date');

Date::make('date')->date(['showMonths' => 2]);  // Flatpickr options
```

### Time

```php
Time::make('time');

Time::make('time')->time(['time_24hr' => false]);   // Flatpickr options
```

### Datetime

```php
Datetime::make('datetime');
```

## Files

Renders an [File Component](/form-file.md) with optional Filepond integation.

```php
File::make('photo')
    ->multiple() // Enables selecting multiple files
    ->filepond() // Enables filepond
    ->accept('image/jpeg')
    ->accept(['image/png', 'image/jpeg']);
```

Additional Filepond configuration:

```php
File::make('photo')
    ->filepond()
    ->server()   // Enables asynchronous uploads of files
    ->preview()  // Show image preview

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

## Checkboxes

Renders a [Checkbox Component](/form-checkbox.md).

Checkboxes can be defined as separate fields:

```php
Checkbox::make('options[]')->label('Checkbox 1')->value('checkbox-1');
Checkbox::make('options[]')->label('Checkbox 2')->value('checkbox-2');
```

Or as an options array:

```php
Checkboxes::make('options')
    ->label('Choose your options')
    ->options([
        'option-1' => 'Option 1',
        'option-2' => 'Option 2',
        'option-3' => 'Option 3',
    ])
    ->inline(); // inline is optional
```

## Radios

Renders a [Radio Component](/form-radio.md).

Radios can be defined as separate inputs:

```php
Radio::make('option')->label('Radio 1')->value('radio-1'),
Radio::make('option')->label('Radio 2')->value('radio-2'),
Radio::make('option')->label('Radio 3')->value('radio-3')->help('I need somebody!'),
```

Or as an array:

```php
Radios::make('theme')
    ->label('Choose a theme')
    ->options([
        'dark' => 'Dark theme',
        'light' => 'Light theme',
        'system' => 'System theme',
    ])
    ->inline(); // inline is optional
```

## Selects

Renders a [Select Component](/form-select.md).

```php
$options = [
    'be' => 'Belgium',
    'de' => 'Germany',
    'fr' => 'France',
    'lu' => 'Luxembourg',
    'nl' => 'The Netherlands',
];

Select::make('country')
    ->label('Choose a country')
    ->options($options);

Select::make('countries[]')
    ->label('Choose multiple countries')
    ->options($options)
    ->multiple()    // Enables choosing multiple options
    ->choices();    // Enables the Choices.js integration

Select::make('countries[]')
    ->options($options)
    ->multiple()
    ->choices(['searchEnabled' => false]);  // Pass Choices.js options

Select::make('user')
    ->label('Select with data from a remote URL')
    ->remoteUrl('/api/users');

Select::make('user')
    ->label('Select with data from a remote URL with a remote root')
    ->remoteUrl('/api/users')
    ->remoteRoute('data.users')
    ->optionLabel('name')
    ->optionValue('id');

Select::make('user')
    ->label('Select with Option Label and Value')
    ->options([
        ['user_id' => 10, 'user_name' => 'John'],
        ['user_id' => 20, 'user_name' => 'Jane'],
        ['user_id' => 30, 'user_name' => 'Mary'],
        ['user_id' => 40, 'user_name' => 'Peter'],
    ])
    ->optionLabel('user_name')
    ->optionValue('user_id');
```

## Submit

Renders a [Submit Component](/form-submit.md).

```php
Submit::make()->label('Send')
```

### Button

```php
Button::make('close_button')
    ->label('Close Modal')
    ->attributes(['@click.prevent' => 'form.restore'])
    ->danger()  // optional
    ->secondary();  // optional
```


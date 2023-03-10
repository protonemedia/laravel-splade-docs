# Form Builder Fields

## Text Input Field
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

## Hidden Field
```php
Hidden::make('hidden_field');

// or:

Input::make('hidden_field')->hidden();
```

## Textarea
```php
Textarea::make('textarea')->autosize();
```

## Number
```php
Number::make('number')
    ->label('Number input')
    ->minValue(1)
    ->maxValue(100)
    ->unsigned(), // alias for ->minValue(0)
    ->step(0.01);
```

## Email
```php
Email::make('email');

// or:

Input::make('email')->email();
```

## Passwords
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

## Date
```php
Date::make('date');

Date::make('date')->date(['showMonths' => 2]);  // Flatpickr options
```

## Time
```php
Time::make('time');

Time::make('time')->time(['time_24hr' => false]);   // Flatpickr options
```

## Datetime
```php
Datetime::make('datetime');
```

## Files
```php
File::make('photo')
    ->multiple() // Enables selecting multiple files
    ->filepond() // Enables filepond
    ->server()   // Enables asynchronous uploads of files
    ->preview()  // Only available with ->filepond() option
    ->accept('image/jpeg')
    ->accept(['image/png', 'image/jpeg']);
```

Additional Filepond configuration:

```php
File::make('photo')
    ->filepond()

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

Checkboxes can be defined as separate inputs:

```php
Checkbox::make('options[]')->label('Checkbox 1')->value('checkbox-1');
Checkbox::make('options[]')->label('Checkbox 2')->value('checkbox-2');
```

Or as an array:

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

```php
Submit::make()->label('Send')
```

## Button
```php
Button::make('close_button')
    ->label('Close Modal'),
    ->if('modal')                   // becomes: v-if="modal"
    ->click('modal.close')          // becomes: @click("modal.close")
    ->clickPrevent('form.restore')  // becomes: @click.prevent="form.restore"
    ->danger()  // optional
    ->secondary();  // optional
```


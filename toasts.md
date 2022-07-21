# Toasts

Splade has built-in support for toasts. Use the `Toast` facade to send a toast to the frontend:

```php
<?php

namespace App\Http\Controllers;

use ProtoneMedia\Splade\Facades\Toast;

class UserController
{
    public function update()
    {
        Toast::title('Your profile was updated!');

        return redirect()->route('user.show', auth()->id());
    }
}
```

The instance has several methods to position (`leftTop`, `centerTop`, `rightTop`, `leftCenter`, `center`, `rightCenter`, `leftBottom`, `centerBottom`, and `rightBottom`) and style (`info`, `success`, `warning` and `danger`) of the toast. The default is `rightTop` and `success`.

Other options include a backdrop filter, an auto-dismiss timeout, and an additional message line:

```php
Toast::title('Whoops!')
    ->message('No space left')
    ->warning()
    ->leftTop()
    ->backdrop()
    ->autoDismiss(15);
```
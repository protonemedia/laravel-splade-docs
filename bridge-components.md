# Bridge Components (in beta)

As of version 1.3, Splade supports bridging Blade Components and Vue Templates. This means there's two-way binding of the public PHP properties and the templates' properties, and you may call public methods from within the template as if they were JavaScript methods.

> **Warning**
> This feature is still in beta and therefore experimental. Use with care!

## Prerequirement

First, you must register a supporting route using the `spladeWithVueBridge()` method on the `Route` facade. As of version 1.3.0, the automatic installer does this for you. If you need to register the route manually, make sure it uses the `web` Middleware, for example, in `web.php`:

```php
Route::spladeWithVueBridge();
```

## Example component

Let's start with an example component, and later on, we'll dive into the technical details. We'll start with creating a component to quickly fake a user's email address:

```bash
php artisan make:component FakeEmail
```

The component has two properties: a User model and an optional prefix. There's a method that performs the update, and lastly, there is the `WithVue` trait:

```php
<?php

namespace App\View\Components;

use App\Models\User;
use Illuminate\View\Component;
use ProtoneMedia\Splade\Components\WithVue;

class FakeEmail extends Component
{
    use WithVue;

    public function __construct(
        public User $user,
        public ?string $prefix = ''
    ) {
    }

    public function fake()
    {
        $this->user->update([
            'email' => $this->prefix . fake()->email,
        ]);
    }

    public function render()
    {
        return view('components.fake-email');
    }
}
```

In the template, all public properties of the Blade Component are available through the `props` object. So instead of echoing the email address using Blade's curly braces, we'll use Vue's syntax to ensure the two-way binding. Finally, we'll attach the method to the button's click handler and use the `v-model` directive to set the prefix:

```blade
<p>User Email: <span v-text="props.user.email" /></p>

<input v-model="props.prefix" placeholder="Prefix..." />

<button @click="fake">Fake Email</button>
```

That's it! You don't have to register a route or controller for each new component. Instead, everything is handled for you using the `WithVue` trait. Now you may use the Blade Component:

```blade
<x-fake-email :user="$user" />
```

## Renderless example

Instead of using the view template, you may also omit the `render` method and use the component directly in your template. The great thing about that is that you may reuse the same logic repeatedly but still be able to fully customize the UI. So first, let's remove the method:

```php
<?php

namespace App\View\Components;

use App\Models\User;
use Illuminate\View\Component;
use ProtoneMedia\Splade\Components\WithVue;

class FakeEmail extends Component
{
    use WithVue;

    public function __construct(
        public User $user,
        public ?string $prefix = ''
    ) {
    }

    public function fake()
    {
        $this->user->update([
            'email' => $this->prefix . fake()->email,
        ]);
    }
}
```

Now you can use the component in other templates and provide the UI with a slot:

```blade
<h1>Page Title</h1>

<x-fake-email :user="$user">
    <p>User Email: <span v-text="props.user.email" /></p>

    <input v-model="props.prefix" placeholder="Prefix..." />

    <button @click="fake">Fake Email</button>
</x-fake-email>
```

## Collections

...

## Inline Component View

Instead of using a dedicated file for the template, you may also use an [Inline Component View](https://laravel.com/docs/10.x/blade#inline-component-views):

```php
<?php

namespace App\View\Components;

use Illuminate\View\Component;
use ProtoneMedia\Splade\Components\WithVue;

class FindIp extends Component
{
    use WithVue;

    public function __construct(
        public $hostname = '',
        public $ip = '',
    ) {
    }

    public function find()
    {
        $this->ip = gethostbyname($this->hostname);
    }

    public function render()
    {
        return <<<'blade'
            <input v-model="props.hostname" />
            <button @click="find">Find IP</button>
            <p v-text="props.ip" />
        blade;
    }
}
```

## Toasts and Redirects

You may send a [Toast](./toasts.md) from a component method to the frontend:

```php
use ProtoneMedia\Splade\Facades\Toast;

public function fake()
{
    $this->user->update([
        'email' => $this->prefix . fake()->email,
    ]);

    Toast::success('User updated');
}
```

Similarly, you may return a Redirect:

```php
public function fake()
{
    $this->user->update([
        'email' => $this->prefix . fake()->email,
    ]);

    return redirect()->route('users.index');
}
```

## Middleware and Security

By default, component methods called from the frontend will use the same Middleware stack as the original route. In addition, you may call the `middleware` method to apply additional constraints:

```php
public function fake()
{
    $this->middleware('can:update-email');

    $this->user->update([
        'email' => $this->prefix . fake()->email,
    ]);
}
```

Please be very aware that all public properties are exposed to the browser and thus visible to the end user. Be sure sensitive Model attributes are [hidden](https://laravel.com/docs/10.x/eloquent-serialization#hiding-attributes-from-json).

Also, just like controller methods, public component methods may be manually invoked by users. Therefore, always validate incoming data and, when necessary, use a [Rate Limiter](https://laravel.com/docs/10.x/rate-limiting#main-content).
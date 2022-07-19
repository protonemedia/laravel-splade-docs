# X-Splade-Modal Component

With the **Modal Component**, Splade has built-in support for modals and slideover. This allows you to load *any* route into a modal. To prepare a view for use inside a modal, you simply have to wrap the content in a `<x-splade-modal>` component. Nothing changes when requesting the view *outside* of the modal, everything will work as used to be.

For example, here's a page to create a new user. This is a regular, full page view that extends the base layout.

```blade
@extends('layout')

@section('content')

<h1>Create new user</h1>

<x-form>
    <input v-model="form.name" placeholder="Name" />
    <button type="submit">Create</button>
</x-form>

@endsection
```

Now wrap the content:

```blade
@extends('layout')

@section('content')

<x-splade-modal>
    <h1>Create new user</h1>

    <x-form>
        <input v-model="form.name" placeholder="Name" />
        <button type="submit">Create</button>
    </x-form>
</x-splade-modal>

@endsection
```

And that's it! Now when you want to load this route into a view, simply use the `modal` attribute on the `Link` component:

```blade
<Link modal href="/users/create">Create new user</Link>
```

## Slideover

To load the page into a slideover, replace the `modal` attribute with the `slideover` attribute. You don't have to change your view.

```blade
<Link slideover href="/users/create">Create new user</Link>
```

## Size

You can control the size of the modal with the `max-width` attribute. Valid values are `sm`, `md`, `lg`, `xl`, `2xl`, `3xl`, `4xl`, `5xl`, `6xl` and `7xl`. The default is `2xl` for a modal, and `md` for a slideover.

```blade
<x-splade-modal max-width="lg">
```
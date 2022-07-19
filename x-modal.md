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

That's it! Now when you want to load this route into a view, simply use the `modal` attribute on the `Link` component:

```blade
<Link modal href="/users/create">Create new user</Link>
```
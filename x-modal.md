# X-Splade-Modal Component

With the **Modal Component**, Splade has built-in support for modals and slideover. This component allows you to load *any* route into a modal. To prepare a view so you can use it inside a modal, you have to wrap the content in a `<x-splade-modal>` component. Nothing changes when requesting the view *outside* of the modal. Everything will work as it used to be.

For example, here's a page to create a new user. This page is a regular, full-page view that extends the base layout.

```blade
@extends('layout')

@section('content')

<h1>Create new user</h1>

<x-splade-form>
    <input v-model="form.name" placeholder="Name" />
    <button type="submit">Create</button>
</x-splade-form>

@endsection
```

Now wrap the content:

```blade
@extends('layout')

@section('content')

<x-splade-modal>
    <h1>Create new user</h1>

    <x-splade-form>
        <input v-model="form.name" placeholder="Name" />
        <button type="submit">Create</button>
    </x-splade-form>
</x-splade-modal>

@endsection
```

And that's it! Now when you want to load this route into a view, use the `modal` attribute on the `Link` component:

```blade
<Link modal href="/users/create">Create new user</Link>
```

## Slideover

To load the page into a slideover, replace the `modal` attribute with the `slideover` attribute. You don't have to change your view.

```blade
<Link slideover href="/users/create">Create new user</Link>
```

## Size

You can control the size of the modal with the `max-width` attribute. Valid values are `sm`, `md`, `lg`, `xl`, `2xl`, `3xl`, `4xl`, `5xl`, `6xl` and `7xl`. The default is `2xl` for a modal and `md` for a slideover.

```blade
<x-splade-modal max-width="lg">
```

## Close Button

The modal and slideover have a *close button*, which you can disable with the `close-button` attribute.

```blade
<x-splade-modal :close-button="false">
```

You can manually close the modal or slideover with the `modal.close()` or `modal.setIsOpen()` methods. Under the hood, it uses [HeadlessUI's dialog component](https://headlessui.com/vue/dialog#showing-and-hiding-your-dialog).


```blade
<x-splade-modal>
    <button type="button" @click="modal.close">Cancel</button>

    <button type="button" @click="modal.setIsOpen(false)">Cancel</button>
</x-splade-modal>
```


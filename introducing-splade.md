# Introducing Splade

Splade provides a **super easy** way to build *Single Page Applications* (SPA) using regular [Laravel Blade](https://laravel.com/docs/9.x/blade) templates, enhanced with [renderless Vue 3 components](https://adamwathan.me/renderless-components-in-vuejs/). In essence, you can just write your app using the simplicity of Blade, and besides that magic *SPA-feeling*, you can sparkle it to make it interactive. All without ever leaving Blade.

**Wow! Sounds interesting? Let's take a closer look.**

## The easiest example: a toggle

In the example below we show an excerpt of the blog post. When the user clicks on the *Expand* button, it hides the excerpt and shows the full content. By default, all Splade components are prefixed with `splade`, but you may configure it without a prefix, resulting in a more readable `<x-toggle>` component.

```blade
@extends('layout')

@section('content')

<h1>Blog Post</h1>

<x-splade-toggle>
    <div v-show="toggled">{{ $blog->full_content }}</div>

    <div v-show="!toggled">
        <p>{{ $blog->excerpt }}</p>
        <button @click="toggle">Expand</button>
    </div>
</x-splade-toggle>

@endsection
```

As you can see, inside the component you can use both Blade and Vue markup. Keep in mind that Blade is rendered first.

## Form example

The dedicated form component allows you to send forms asynchronously. It catches validation errors and it supports two-way binding. You don't even need to specify a structure for the data.

```blade
@extends('layout')

@section('content')

<h1>Create new user</h1>

<x-form action="/user/new">
    <input type="name" v-model="form.name" placeholder="Full name" />
    <p v-text="form.errors.name" />

    <input type="email" v-model="form.email" placeholder="Email address" />
    <p v-text="form.errors.email" />

    <button type="submit">Save</button>
</x-form>

@endsection
```

**Want to see something even cooler?**

First, let's explain what the `x-form` component actually is: it's a wrapper around a Vue component. Yes, each Splade component consists of a renderless Vue component (where all interactive magic happens), wrapped in a Blade component.

In the example below, we give the form component some default data, that of the authenticated user. Note that the user data is passed to the frontend, so make sure sensitive attributes are [hidden](https://laravel.com/docs/9.x/eloquent-serialization#hiding-attributes-from-json).

```blade
<x-form :action="route('user.current.update')" :default="auth()->user()">
    <input type="name" v-model="form.name" placeholder="Full name" />
    <p v-text="form.errors.name" />

    <button type="submit">Update</button>
</x-form>
```

## Navigation example

To leverage the SPA capabilities of Splade, you may use the `<Link>` component instead of the `<a>` element. This prevents full page reloads, and only fetches the new page. The `<Link>` component is essentially a wrapper around `<a>`, so there's not much to learn:

```blade
<Link href="/">Home</Link>
<Link href="{{ route('contact') }}">Contact</Link>
```

If you're using an existing app, or maybe you don't like the `Link` component, you may configure Splade to automatically transform all `<a>` elements.

## What's more?

Here's a summary of all cool things Splade can do:

* `<Link>` component to prevent full page reloads, giving you that *SPA-feeling*.
* `<x-data>` component to interact with reactive data (two-way binding). Support for default values, remembering its state between pages, and even remembering its state in local storage.
* `<x-defer>` component to asynchronously load or poll data.
* `<x-errors>` component to show validation errors *outside* of a form.
* `<x-event>` component to listen for broadcasted events. Support for redirecting, refreshing, showing toasts, and interacting with the event data.
* `<x-flash>` component to interact with flashed data.
* `<x-form>` component to send forms asynchronously. Supports default values, validation errors, two-way binding, file uploads, confirmation popup, and state management.
* `<x-modal>` component to load *any* route into a modal or slideover. Supports nested modals as well.
* `<x-state>` component to interact with validation errors, flashed data, and shared data.
* `<x-toggle>` component to easily toggle boolean values on your page.
* `Toast` component to display toasts on your page. Supports nine positions, four styles, backdrop and auto-dismiss.
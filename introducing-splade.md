# Introducing Splade

Splade provides a super easy way to build *Single Page Applications* (SPA) using regular [Laravel Blade](https://laravel.com/docs/9.x/blade) templates, enhanced with [renderless Vue 3 components](https://adamwathan.me/renderless-components-in-vuejs/). In essence, you can just write your app using the simplicity of Blade, and besides that magic *SPA-feeling*, you can sparkle it to make it interactive. All without ever leaving Blade.

**Wow! Sounds interesting? Let's take a closer look.**

## The easiest example: a toggle

In the example below we show a excerpt of the blog post. When the user clicks on the *Expand* button, it hides the excerpt and shows the full content. By default, all Splade components are prefixed with `splade`, but you may configure it without a prefix, resulting in a more readable `<x-toggle>` component.

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

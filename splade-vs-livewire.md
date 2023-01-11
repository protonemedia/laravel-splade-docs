# Splade and Livewire comparison

Splade and Livewire are hard to compare as they are quite different, both technically and in their philosophies. However, since many people have asked about their differences, here's a little overview.

Most notably, as of January 2023, Livewire has no single-page application (SPA) capabilities. It was demoed at Laracon Online 2022, and it's planned for the next major version of Livewire, though there's no release date yet.

## Template engine

Both Livewire and Splade primarily use Laravel Blade as a templating engine. In addition to Blade, Splade integrates *Vue's render engine*, which gives you access to all of Vue's features and its wide range of third-party libraries. To sprinkle Livewire apps with JavaScript behavior, it is advised to use [AlpineJS](https://alpinejs.dev). AlpineJS is clean and powerful, but you might want to consider Vue as its community is much larger, as is its ecosystem of third-party libraries.

To summarize:
* Splade allows you to use the Laravel Blade template engine to build a single-page application.
* In addition to Blade templates, you may use custom and third-party Vue components and libraries with Splade. Livewire recommends using AlpineJS.

## Components

Livewire works by building **Livewire-specific** Components. It has its own syntax in Blade templates with `wire:*` attributes and directives. Splade works with Laravel's *native* [Blade Components](https://laravel.com/docs/9.x/blade) and Vue Components. Though some features of Splade require a single Middleware (like the SPA-navigation features), it doesn't need a custom compiler or engine to work.

To summarize:
* Splade doesn't need the creation of *special* components as it uses native Laravel and Vue implementations to achieve reactivity.
* Livewire achieves reactivity using `wire:*` attributes and directives.

## Passing data

By default, data stored in public properties of Livewire components will be available client-side (in the browser). Of course, you may use a transformation layer (like [Fractal](https://fractal.thephpleague.com) or Laravel's [API Resources](https://laravel.com/docs/9.x/eloquent-resources)), but you still have to worry about the data size and not sharing sensitive data with the browser.

In Splade applications, almost all data is rendered using standard server-side Blade templates. Once rendered in Blade, the raw data won't be available client-side (in the browser). Of course, you may pass data to the frontend, for example, with the [Data](/x-data.md) and [Form](/x-form.md) components, but that’s very explicit. The Form component even helps you guard data against being passed to the browser.

To summarize:
* With Livewire, you should be very aware that data in public properties will be visible client-side.
* This is less of a concern with Splade, as almost everything is rendered server-side, and most raw data will never leave the application.
* In every application, and also with Splade, there are cases where you still want to pass data to the frontend. Splade provides components to help you do this.

## Data binding

Livewire will perform a re-render using an AJAX request for all `input` events, like typing in a text field. Luckily, there's a default 150ms debounce, and there are tweaks to minimize the number of updates (like the `defer` and `lazy` modifiers). You don't have to worry about this with Splade, as it uses [Vue's two-way binding](https://vuejs.org/guide/essentials/forms.html) in the browser to bind or synchronize properties. Therefore, it doesn't perform AJAX requests except in some components that are very explicit, like the [Defer component](/x-defer.md).

To summarize:
* With Livewire, you must take care of minimizing re-renders and AJAX requests to avoid performance issues.
* Splade doesn't need network activity to do two-way binding, as the widely praised Vue engine handles it.
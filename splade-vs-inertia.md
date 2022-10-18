# Splade and Inertia.js comparison

Splade and Inertia.js share many features and goals but are fundamentally different. This overview is definitely **not** intended to disregard Inertia.js in any way but even more to help you choose a direction for your next application.

## Template engine

The key difference between Splade and Inertia.js is the template engine and how you pass data to templates. While Splade lets you combine the power of the Blade and the Vue template engine, Inertia.js templates should be written in React, Vue, or Svelte, in addition to requiring a backend framework like Laravel.

To summarize:
* Splade allows you to use the Laravel Blade template engine and build a single-page application using regular server-side routing without building an API.
* Inertia.js requires a separate frontend *and* backend framework and acts as a bridge between them. It's framework agnostic, so you may use frameworks other than Laravel and Vue, like Rails and React.

## Passing data

Passing data from the Controller to templates is handled very differently. With Inertia.js, **all data** you pass from the Controller to a template will be available client-side (in the browser). Of course, you may use a transformation layer (like [Fractal](https://fractal.thephpleague.com) or Laravel's [API Resources](https://laravel.com/docs/9.x/eloquent-resources)), but you still have to worry about the data size and not sharing sensitive data with the browser.

In Splade applications, almost all data is rendered using standard server-side Blade templates. Once rendered in Blade, the raw data won't be available client-side (in the browser). Of course, you may pass data to the frontend, for example, with the [Data](/x-data.md) and [Form](/x-form.md) components, but that’s very explicit. The Form component even helps you guard data against being passed to the browser.

Another inconvenience in development with Inertia.js + Vue is having to define the data and properties twice. First, you pass the data from the Controller, and then you *also* have to explicitly specify the data client-side, for example, in the `props` declaration of the Vue template. With Splade, you can use the data directly in the Blade template without defining it.

To summarize:
* With Inertia.js, you should be very aware that every piece of data you return from the Controller will be visible client-side.
* This is less of a concern with Splade, as almost everything is rendered server-side, and most raw data will never leave the application.
* In every application, and also with Splade, there are cases where you still want to pass data to the frontend. Splade provides components to help you do this.
* Besides returning the server-side, with Inertia.js + Vue, you must also define the data as props *again* in the front-end template. This is not needed with Splade.

## Laravel integration

Splade aims to stick as close as possible to the default Laravel features and development flow:

* You don't need a *special* method in Controller methods, like `Inertia::render()`. With Splade, you can use Laravel's `view()` method.
* You don't need to make your route definitions available to the frontend in order to generate URLs in templates. Inertia.js recommends the Ziggy library for this. With Splade, you can use Laravel's `route()` method and `Route` facade.
* You don't need to modify a Middleware class or use a dedicated method like `Inertia::share()` to pass flash or shared data to templates. With Splade, you can use Laravel's `session()->flash()` and `View::share()` methods.

## Misc

* Besides using 3rd-party Vue libraries, Splade allows you to use Laravel packages with Blade components and directives. For example, you can use the [Blade Icons](https://blade-ui-kit.com/blade-icons) package in your application.
* Inertia.js is framework agnostic, and while that’s a great foundation, it also seems to slow down development. Splade focuses on Laravel and Vue and can fully integrate with both without worrying about compatibility and cross-platform implementation challenges.
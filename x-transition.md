# X-Splade-Transition Component

The **Transition Component** allows you to animate elements that should be shown or hidden. It comes with a few default animations, but you can also use custom animations and define presets.

## Toggle Example

This example uses the [Toggle component](/x-toggle.md) to show or hide the *Welcome!*-message. Instead of using a `v-show` to toggle the message, you may now use the `show` attribute:

```blade
<x-splade-toggle>
    <button @click.prevent="toggle">Toggle message</button>

    <x-splade-transition show="toggled">
        Welcome!
    </x-splade-transition>
</x-splade-toggle>
```

## CSS Classes

You may use classes to style a transition. While you can use any class you want, here are the links to the Tailwind CSS documentation on [Transitions](https://tailwindcss.com/docs/transition-duration) and [Transforms](https://tailwindcss.com/docs/scale).

```blade
<x-splade-transition
    show="toggled"
    enter="transition-opacity duration-75"
    enter-from="opacity-0"
    enter-to="opacity-100"
    leave="transition-opacity duration-150"
    leave-from="opacity-100"
    leave-to="opacity-0"
>
    ...
</x-splade-transition>
```

## Presets

With the `Animate` Facade, you may define presets, so you don't have to copy the CSS classes across your project.

```php
Animation::new(
    name: 'slide-left',
    enter: 'transform transform ease-in-out duration-300',
    enterFrom: 'opacity-0 -translate-x-full',
    enterTo: 'opacity-100 translate-x-0',
    leave: 'transform transform ease-in-out duration-300',
    leaveFrom: 'opacity-100 translate-x-0',
    leaveTo: 'opacity-0 -translate-x-full',
);
```

If you add new animations, for example, in the `AppServiceProvider` class, make sure you add the path to the `content` section of your `tailwind.config.js` file. This way, the Tailwind JIT engine knows where to find the classes.

Now you can use the preset in your template:

```blade
<x-splade-transition animation="slide-left" show="toggled">
    ...
</x-splade-transition>
```

## Included animations

The included animations are `default`, `opacity`, `fade`, and `slide-right`.
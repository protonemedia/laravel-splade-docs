# X-Splade-Script Component

In addition to building [custom Vue components](/custom-vue-components.md), you may use the **Script Component** to add inline scripts to your Blade template. Usually, the Vue render engine would throw an error on script tags, but this component magically injects your script as if it was called in the [`mounted()`](https://vuejs.org/api/options-lifecycle.html#mounted) hook of a Vue component. This way, we can avoid the typical error, and you get access to the global `$splade` object, which you may use to navigate and access the Event Bus.

Note that this component is quite limited, and it is generally recommended to use custom Vue components.

```blade
<x-splade-script>
    document.body.classList.add('bg-confetti');
</x-splade-script>
```

Here's another example of using Splade's navigation capabilities:

```blade
<x-splade-script>
    setTimeout(() => $splade.visit('/destination'), 5000);
</x-splade-script>
```


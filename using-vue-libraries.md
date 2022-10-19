---
description: You may Vue libraries in a Laravel Splade app by install the library using npm, and then import and register it in the main app.js file. You must import and register the component by passing both a name string and a component definition.
keywords: laravel blade vue, laravel vue blade, laravel vue library, laravel vue libraries
---

# Using Vue libraries

Using Vue libraries in a Splade app works the same as any other Vue application. You may install the library using `npm`, and then import and register it in the main `app.js` file. If you're using [SSR](/ssr.md), make sure to import the component in `ssr.js` as well.

```bash
npm install vue3-carousel
```

You must import and register the component by passing both a name string and a component definition. Note how the name can be different from the component definition.

```js
import { Carousel, Slide, Pagination, Navigation } from 'vue3-carousel';      // [tl! add]

const el = document.getElementById("app");

createApp({
    render: renderSpladeApp({ el }),
})
    .use(SpladePlugin, {
        "max_keep_alive": 10,
        "transform_anchors": false,
        "progress_bar": true
    })
    .component('Carousel', Carousel)      // [tl! add]
    .component('CarouselSlide', Slide)    // [tl! add]
    .component('CarouselPagination', Pagination)      // [tl! add]
    .component('CarouselNavigation', Navigation)      // [tl! add]
    .mount(el);
```

Instead of calling the `component` method for each component, you may also use the `components` key and pass an object:

```js
import { Carousel, Slide, Pagination, Navigation } from 'vue3-carousel';      // [tl! add]

createApp({
    render: renderSpladeApp({ el })
})
    .use(SpladePlugin, {
        "max_keep_alive": 10,
        "transform_anchors": false,
        "progress_bar": true,
        "components": { // [tl! add]
            Carousel,   // [tl! add]
            CarouselSlide: Slide,   // [tl! add]
            CarouselPagination: Pagination, // [tl! add]
            CarouselNavigation: Navigation,  // [tl! add]
        },  // [tl! add]
    })
    .mount(el);
```

Now you may use the library in a Blade template. Note how we loop over the `$products` items with the Blade `@foreach` directive, *inside* the Vue Carousel component.

```blade
<x-layout>
    <h1>Products</h1>

    <Carousel>
        @foreach($products as $key => $product)
            <CarouselSlide key="{{ $key }}">
                {{ $product->name }}
            </CarouselSlide>
        @endforeach

        <template #addons>
            <CarouselNavigation />
            <CarouselPagination />
        </template>
    </Carousel>
</x-layout>
```
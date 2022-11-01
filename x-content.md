# X-Splade-Content Component

```blade
<x-splade-content :html="$renderedMarkdown" />
```

By default, the element is rendered as a `div`, but you may customize it, as well as pass other attributes:

```blade
<x-splade-content as="article" class="prose" :html="$renderedMarkdown" />
```
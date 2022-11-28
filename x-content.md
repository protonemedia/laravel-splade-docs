# X-Splade-Content Component

All templates are passed to the Vue render engine. Still, sometimes you want to output raw, pre-rendered HTML, and you might want to bypass Vue's interpolation using the **Content Component**. A typical example is pre-rendered Markdown content. Note that the content is static, and you can't pass Splade or Vue components.

```blade
<x-splade-content :html="$renderedMarkdown" />
```

By default, the element is rendered as a `div`, but you may customize it, as well as pass other attributes:

```blade
<x-splade-content as="article" class="prose" :html="$renderedMarkdown" />
```

As [stated in the Vue documentation](https://vuejs.org/api/built-in-directives.html#v-text), only use this component with trusted content and never on user-provided content, as it may lead to XSS attacks.
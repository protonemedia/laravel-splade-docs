# X-Splade-Submit Component

The **Submit Component** has a default label and spinner visible when processing the form. You may change the default text using the `label` attribute, and you may disable the spinner as well.

```blade
<x-splade-submit label="Apply settings" :spinner="false" >
```

You may also use a slot:

```blade
<x-splade-submit>
    <svg>...</svg>
    <span>Send now</span>
</x-splade-submit>
```
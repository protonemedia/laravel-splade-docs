# X-Splade-Submit Component

The **Submit Component** has a default label and spinner visible when processing the form. You may change the default text using the `label` attribute, and you may disable the spinner as well.

```blade
<x-splade-submit label="Apply settings" :spinner="false" />
```

You may also use a slot:

```blade
<x-splade-submit>
    <svg>...</svg>
    <span>Send now</span>
</x-splade-submit>
```

In addition to providing a custom template with a slot, you may change the styling of the default submit button with the `danger` or `secondary` attributes:

```blade
<x-splade-submit danger label="Remove account" />

<x-splade-submit secondary label="Save for later" />
```
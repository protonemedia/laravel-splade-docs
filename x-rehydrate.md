# X-Splade-Rehydrate Component

The **Rehydrate Component** can reload a section of your Blade template. You may combine this component with the [Event Bus](/event-bus.md) to trigger the reload on certain events.

To get started, wrap the section you want to be *reloadable* in the component. For example, imagine a list of team members:

```blade
<ul>
    @foreach($team->members as $member)
        <li>{{ $member->name }}</li>
    @endforeach
</ul>
```

You may wrap the list and pass the name of the event to the `on` attribute:

```blade
<x-splade-rehydrate on="team-member-added">
    <ul>
        @foreach($team->members as $member)
            <li>{{ $member->name }}</li>
        @endforeach
    </ul>
</x-splade-rehydrate>
```

If there's a form that stays on this page after a successful request, you may emit an event that reloads the list:

```blade
<x-splade-form action="/team/member" stay @success="$splade.emit('team-member-added')">
    <x-splade-input name="name" />
    <x-splade-submit />
</x-splade-form>
```

Of course, when the form doesn't have the `stay` attribute and redirects back to the same page, it will reload the complete page, including the list, but you'll lose the state of other components on the page. This pattern can help avoid that.

## Poll

You may also use this component to poll for new data. With the `poll` attribute, you can specify the interval in milliseconds.

```blade
<x-splade-rehydrate poll="5000">
    Today's score: {{ $score }}
</x-splade-rehydrate>
```
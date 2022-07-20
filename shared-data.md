# Shared Data

Sometimes you might want to share additional data from the backend to the frontend. You can do this by using the `share` method on the Splade facade:

```php
Splade::share('adminLastSeenAt', Admin::value('last_seen_at')->toIso8601String());
```

In the frontend, you can access shared data using the State Component:

```blade
<x-splade-state>
    <p>Admin last seen at: <span v-text="state.shared.adminLastSeenAt" /></p>
</x-splade-state>
```
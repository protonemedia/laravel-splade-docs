# X-Splade-Event Component

The **Event Component** might be the coolest Splade component of all. It allows you to listen for broadcasted events using [Laravel Echo](https://laravel.com/docs/9.x/broadcasting#client-side-installation).

## Setup

This component assumes you've setup Laravel Echo as in the official docs. Basically, you need to make sure the Echo instance is accessable through `window.Echo`. Future releases of Splade might allow for other implementations.

## Refresh on event

Imagine your app allows customers to make payments. The payment providers redirects back to a *status* page, and it performs a webhook request once the payment has succeeded. On the status page, you may poll for an updated status, but you may also listen for a broadcasted event. With Splade, you may instruct the status page to listen to the `OrderWasPaid` event and redirect to another page when the event is fired.

In the `broadcastWith` method of your event, use `Splade::redirectOnEvent()` to generate an URL to redirect to. Under the hood, it uses Laravel's `Redirector` class, so you can use common methods like `route` and `to`:

```php
<?php

use App\Models\Order;
use Illuminate\Broadcasting\PrivateChannel;
use Illuminate\Contracts\Broadcasting\ShouldBroadcast;
use ProtoneMedia\Splade\Facades\Splade;

class OrderWasPaid implements ShouldBroadcast
{
    public function __construct(public Order $order)
    {
    }

    public function broadcastOn()
    {
        return new PrivateChannel('customer.' . $this->order->customer_id);
    }

    public function broadcastWith()
    {
        return [
            Splade::redirectOnEvent()->route('order.summary', $this->order->id),
        ];
    }
}
```

Now in your Blade template, specify the channel and event you want to listen to. You may use the `private` attribute to indicate that a channel is private.

```blade
<x-splade-event private channel="customer-1" listen="OrderWasPaid" />
```

## Refresh on event

Similar to redirecting, you may also just refresh the current page.

```php
<?php

use Illuminate\Contracts\Broadcasting\ShouldBroadcast;
use ProtoneMedia\Splade\Facades\Splade;

class OrderStatusWasUpdated implements ShouldBroadcast
{
    public function broadcastWith()
    {
        return [
            Splade::refreshOnEvent(),
        ];
    }
}
```

## Toast on event

Splade allows you to send toasts to the frontend using broadcasted events. Use the `Splade::toastOnEvent()` method to instantiate a new toast.

```php
<?php

use Illuminate\Contracts\Broadcasting\ShouldBroadcast;
use ProtoneMedia\Splade\Facades\Splade;

class HighServerLoadDetected implements ShouldBroadcast
{
    public function broadcastWith()
    {
        return [
            Splade::toastOnEvent('High server load detected')->warning(),
        ];
    }
}
```

## Listen for multiple events

You may specify multiple events to listen to:

```blade
<x-splade-event private channel="customer-1" listen="OrderWasCancelled, OrderWasPaid" />
```

## Using raw event data

Instead of the features above, you may also use the raw event data. There are `subscribed` and `events` props:

```blade
<x-splade-event private channel="admins" listen="IncomingEmail, IncomingSupportBubble">
    <p v-if="subscribed">Subscribed!</p>

    <div v-for="event in events">
        <p v-text="event.name" />
        <p v-text="event.data.username" />
    </div>
</x-splade-event>
```
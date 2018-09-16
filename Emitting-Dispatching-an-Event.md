The `event_bus` library provides a generic event data structure to standardize what an event is for all of its listeners/subscribers. Before delivering an event, it is good to know what an event is for the `event_bus` library. If this is your fist moments with the library, it is highly recommended to read the data structure of the [`EventBus.Model.Event`](https://github.com/otobus/event_bus/wiki/EventBus.Model.Event-Data-Structure) structure.

Let's create a sample event using mandatory attributes:
```elixir
# Build the event structure
event = 
  %EventBus.Model.Event{
    id:    EventBus.Util.Base62.unique_id(), # If you add `uuid` library then good to use `UUID.uuid4()`.
    topic: :user_created,
    data:  %{email: "sample@example.com", ...} # an aggregated data for the event    
  }

# Emit the event
EventBus.notify(event)
> 23:49:27.316 [warn]  Topic(:user_created) doesn't have subscribers
> :ok
```

Congratulations!!! You emitted your first event with the `event_bus` library. But, you might think nothing happened: you just an `:ok` as result and also a warning message which tells there are no subscribers for the topic. That is fine because you didn't implement any subscribers and also you didn't subscribe to `user_created` topic yet. So, time to implement your first consumers/listeners/subscribers.
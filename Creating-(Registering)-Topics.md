The `event_bus` library emits all events with topic names. Topic name is simply an `atom` type variable. For example; if you want to emit an event after creating users, then `:user_created` could be a good name for the event topic. And before delivering this events for `:user_created` topic, you will need to register the topic name(`:user_created`) once in the app life cycle.

The `event_bus` library allows creating/registering topics in two ways:
* Using your project configuration file `config.exs` (recommended)
* Using `register_topic/1` function (on demand)

### Creating topics using your project configuration file

If you will use the same topic name on all environments, then `config.exs` would be a suitable place for registering your topic names. If you are planning to use a topic name only in `production` environment then you can choose to register on `prod.exs` file:

```elixir
config :event_bus,
  topics: [
    :checkout_completed, 
    :email_sent,
    :payment_failed, 
    :user_created, 
    :user_activated,  
    # more topics...
  ]
```

### Creating topics on demand using `register_topic/1` function

On any part of application you can register topics using `register_topic/1` function. Please note that the function only accepts `atom()` as input type:

```elixir
EventBus.register_topic(:user_created)
EventBus.register_topic(:user_activated)
EventBus.register_topic(:email_sent)
...
```

Congratulations!!! With the registration of an event topic, `event_bus` library become ready to emit events. The next step is [creating an event using the registered topics](https://github.com/otobus/event_bus/wiki/Emitting-(Dispatching)-an-Event) in the application.
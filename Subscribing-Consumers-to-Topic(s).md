Assume that you [registered topics](https://github.com/otobus/event_bus/wiki/Creating-(Registering)-Topics), [created consumers (listeners)](https://github.com/otobus/event_bus/wiki/Creating-Event-Consumers) that can handle these topics and your application is ready to [emit events](https://github.com/otobus/event_bus/wiki/Emitting-(Dispatching)-an-Event) for those topics. Ok, it looks it is time to subscribe your consumers to the right topics.

The `event_bus` library handles topic subscription using the regular expression patterns. It allows you to write multiple regex patterns to subscribe consumers to one topic or many topics.

**Subscribe a consumer to all registered topics:**
```elixir
listener = MyFirstConsumer
topics = [".*"]
EventBus.subscribe({listener, topics})
```

**Subscribe a consumer to only `:user_registered` topic:**
```elixir
listener = MyFirstConsumer
topics = ["^user_registered$"]
EventBus.subscribe({listener, topics})
```

**Subscribe a consumer to multiple topics (`:user_registered` and `:email_sent`):**
```elixir
listener = MyFirstConsumer
topics = ["^user_registed$", "^email_sent$"]
EventBus.subscribe({listener, topics})
```

**Subscribe a consumer to topics starts with the `:user_*` :**
```elixir
listener = MyFirstConsumer
topics = ["^user_*"]
EventBus.subscribe({listener, topics})
```

Congratulations!!! You subscribed your listeners(consumers) to the registered topics. Now you can [emit events](https://github.com/otobus/event_bus/wiki/Emitting-(Dispatching)-an-Event) and see the consumers in action. 
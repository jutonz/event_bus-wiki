EventBus optionally allows you to track `:register_topic`, `:unregister_topic`, `:subscribe` and `:unsubscribe`, `:mark_as_completed` and `mark_as_skipped` action calls by the configuration. To track these events you need to enable them on the configuration and then subscribe to `:eb_action_called` topic.

Note: Enabling optional system events decreases the EventBus performance because it at least doubles the action calls. It is not recommended to enable these events unless you certainly require tracking these events especially `mark_as_completed` and `mark_as_skipped`.

Enabling observable events can only be done on *compile time* (It's good idea to delete your cached build via `rm -rf _build`). Thus, you need to add it to your app configuration. Here is sample configuration to subscribe optional system events:

```elixir
config :event_bus,
  observables: ~w(register_topic unregister_topic subscribe unsubscribe mark_as_completed mark_as_skipped)a,
  id_generator: fn -> :base64.encode(:crypto.strong_rand_bytes(8)) end,
  #id_generator: fn -> UUID.uuid4() end
  ...
```
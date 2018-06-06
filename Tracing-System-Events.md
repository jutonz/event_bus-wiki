Tracing system events is a niche use case for EventBus. But in case you need to trace EventBus system events, you can wrap current EventBus core functions and create new events via them. For instance, if you need to notify some other parts of your app on a new topic registration:

```elixir
# Register a system topic:
EventBus.register_topic(:eb_sys_event_occurred) # regular way to register a topic

# Register a topic via your own wrapper
EventBus.Extended.register_topic(:user_registered) # with the extended 

defmodule EventBus.Extended do
  use EventBus.EventSource

  @sys_topic :eb_sys_event_occurred
  @source "EventBus.Extended"

  def register_topic(topic) do
    EventSource.notify sys_params() do
      EventBus.register(topic)
      %{action: :register_topic, topic: topic}
    end
    :ok
  end

  defp sys_params do
    id = UUID.uuid4() # Assumed you are using UUID for id generation
    %{id: id, transaction_id: id, topic: @sys_topic, source: @source}
  end
end
```
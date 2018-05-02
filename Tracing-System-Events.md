Tracing system events is a niche use case for EventBus. But in case you need to trace EventBus system events, you can wrap current EventBus core functions and create new events from them. For instance, if you need to notify some other parts of your app on a new topic registration:

```elixir
# Register a system topic:
EventBus.register_topic(:eb_system_called) # regular way to register a topic

# Register a topic via your own wrapper
EventBus.SampleWrapper.register_topic(:some_event_occurred) # with the sample wrapper

defmodule EventBus.SampleWrapper do
  use EventBus.EventSource

  def register_topic(topic) do
    unless topic_exist?(topic) do
      EventSource.notify sys_params() do
        EventBus.register(topic)
        %{action: :register_topic, topic: topic}
      end
    end
    :ok
  end

  defp sys_params do
    id = UUID.uuid4() # Assumed you are using UUID for id generation
    %{id: id, transaction_id: id, topic: @sys_topic, source: @source}
  end
end
```
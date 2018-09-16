The `event_bus` library comes with an efficient solution to internal process communication. Unlike other event bus implementations, `event_bus` doesn't deliver the event data directly to its subscribers. It delivers the identifier of the event and topic name which can be queued with minimal memory footprint. With this behavior, subscriber query the event data when they are ready to process the event.

### Simple Event Consumer Implementation

All modules that implements `process/1` function can be a consumer for `event_bus` events.

```elixir
defmodule MyFirstConsumer do
  def process({_topic, _id} = event_shadow) do
    # Fetch event data
    event = EventBus.fetch(event_shadow)

    # Do sth with the event
    Logger.info("I am handling the event with a Simple module #{__MODULE__}")
    Logger.info(fn -> inspect(event) end)

    # Set the event as completed for `MyFirstConsumer`
    EventBus.mark_as_completed({MyFirstConsumer, event_shadow})
  end
end
```

### Simple Asynchronous Event Consumer Implementation with Spawn

The important thing while consuming events is not to block other events and consumers. It is always good idea to use a non-blocking way to handle events. Spawning is one of the core ways in Elixir to handle concurrency. Here is a sample consumer which use spawn to handle `event_bus` events:

```elixir
defmodule MySecondConsumer do
  def process({_topic, _id} = event_shadow) do
    spawn(fn -> do_sth(event_shadow) end)
  end

  def do_sth(event_shadow) do
    # Fetch event data
    event = EventBus.fetch(event_shadow)

    # Do sth with the event
    # Or just log for the sample
    Logger.info("I am handling the event with Spawn")
    Logger.info(fn -> inspect(event) end)

    # Set the event as completed for `MySecondConsumer`
    EventBus.mark_as_completed({MySecondConsumer, event_shadow})
  end
end
```

### Simple Asynchronous Event Consumer Implementation with GenServer

GenServer is a most popular abstraction on Elixir world. Majority of the processes use GenServer abstraction to implement concurrent processing. Here is a sample named GenServer consumer for `event_bus` events:

```elixir
defmodule MyThirdConsumer do
  use GenServer

  @doc false
  def start_link do
    GenServer.start_link(__MODULE__, [], name: __MODULE__)
  end

  @doc false
  def init(_opts) do
    {:ok, []}
  end

  def process({_topic, _id} = event_shadow) do
    GenServer.cast(__MODULE__, event_shadow)
  end

  def handle_cast({topic, id}, state) do
    event = EventBus.fetch_event({topic, id})

    # Do sth with the event
    # Or just log for the sample
    Logger.info("I am handling the event with GenServer #{__MODULE__}")
    Logger.info(fn -> inspect(event) end)

    # mark the event as completed for this consumer
    EventBus.mark_as_completed({__MODULE__, topic, id})
    {:noreply, state}
  end
end
```

### Sample Asynchronous Event Consumer Implementation with GenStage

GenStage and EventBus are great couple to handle backpressure and also consuming large queues. Here is a simple GenStage consumer for `event_bus` events:

```elixir
defmodule MyFourthConsumer do
  @moduledoc false

  alias MyFourthConsumer.Queue

  @doc """
  Enqueue EventBus events to a GenStage consumer
  """
  def process({_topic, _id} = event_shadow) do
    Queue.push(event_shadow)
  end

  defmodule Queue do
    use GenStage

    def init(state) do
      {:producer, state, buffer_size: :infinity}
    end

    def start_link(state \\ []) do
      GenStage.start_link(__MODULE__, state, name: __MODULE__)
    end

    @doc """
    Push event shadows to queue
    """
    def push({_topic, _id} = event_shadow) do
      GenServer.cast(__MODULE__, {:push, event_shadow})
    end

    @doc false
    def handle_cast({:push, event_shadow}, state) do
      {:noreply, [event_shadow], state}
    end

    @doc false
    def handle_demand(_demand, state) do
      {:noreply, [], state}
    end
  end

  defmodule Worker do
    @moduledoc false

    use GenStage

    def init(:ok) do
      {
        :consumer,
        :ok,
        subscribe_to: [
          {
            MyFourthConsumer.Queue,
            min_demand: 0, max_demand: 1
          }
        ]
      }
    end

    def start_link do
      GenStage.start_link(__MODULE__, :ok)
    end

    @doc false
    def handle_events([{topic, id}], _from, state) do
      event = EventBus.fetch_event({topic, id})

      # Do sth with the event
      # Or just log for the sample
      Logger.info("I am handling the event with GenStage #{__MODULE__}")
      Logger.info(fn -> inspect(event) end)

      EventBus.mark_as_completed({MyFourthConsumer, topic, id})
      {:noreply, [], state}
    end
  end
end

```

Congratulations!!! You implemented four different consumers for `event_bus` events. Now, time to subscribe this consumers to topics.
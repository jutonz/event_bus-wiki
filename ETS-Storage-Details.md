When an event configured in `config` file or registered with the `EventBus.register_topic/1` function, 2 ETS tables are created to save and track `EventBus` events.

All event data is temporarily saved to the ETS tables with the name `:eb_es_<<topic>>` until all subscribers processed the data. This table is a *read heavy* table. When a subscriber needs to process the event data, it queries this table to fetch event data.

To watch event status, a separate watcher table is created for each event type with the name `:eb_ew_<<topic>>`. This table is used for keeping the status of the event. `Observation Manager` updates this table frequently with the notification of the event listeners/subscribers.

When all subscribers process the event data, data in the event store and watcher, automatically deleted by the `Observation Manager`. If you need to see the status of unprocessed events, event watcher table is one of the good places to query.

For example; to get the list unprocessed events for `:hello_received` event:

```elixir
# The following command will return a list of tuples with the `id`, and `event_subscribers_list` where `subscribers` is the list of event subscribers, `completers` is the subscribers those processed the event and notified `Observation Manager`, and lastly `skippers` is the subscribers those skipped the event without processing.

# Assume you have an event with the name ':hello_received'
:ets.tab2list(:eb_ew_hello_received)
> [{id, {subscribers, completers, skippers}}, ...]
```

ETS storage SHOULD NOT be considered as a persistent storage. If you need to store events to a persistant data store, then subscribe to all event types by a module with `[".*"]` event topic then save every event data.

For example;

```elixir
EventBus.subscribe({MyDataStore, [".*"]})

# then in your data store save the event
defmodule MyDataStore do
  ...

  def process({topic, id} = event_shadow) do
    GenServer.cast(__MODULE__, event_shadow)
    :ok
  end

  ...

  def handle_cast({topic, id}, state) do
    event = EventBus.fetch_event({topic, id})
    # write your logic to save event_data to a persistant store

    EventBus.mark_as_completed({__MODULE__, topic, id})
    {:noreply, state}
  end
end
```

An example consumer for event storage is [event_bus_postgres](https://github.com/otobus/event_bus_postgres) library, it uses `GenStage` to store `EventBus` events into `Postgres` database.
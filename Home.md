EventBus library is a traceable, extendable and minimalist event bus implementation for Elixir with built-in event store and event watcher based on ETS. It allows basic pub/sub for internal process communication. It is designed for low memory footprint and speed. 

EventBus comes with an efficient solution to internal process communication. Unlike other EventBus implementations, EventBus doesn't deliver the event data directly to its subscribers. It delivers the identifier of the event and topic name which can be queued with minimal memory footprint. With this behavior, subscriber query the event data when they are ready to process the event. 

This wiki includes documentation and samples related to the `event_bus` library and its behaviors. 

# Table of Contents

**Getting Started**
- [Installing Library Package](https://github.com/otobus/event_bus/wiki/Installing-Library-Package)
- [Creating/Registering Topics](https://github.com/otobus/event_bus/wiki/Creating-(Registering)-Topics)
- [Emitting/Dispatching an Event](https://github.com/otobus/event_bus/wiki/Emitting-(Dispatching)-an-Event)
- Emitting/Dispatching an Event using `EventSource` Helper
- [Creating Simple Event Consumers/Listeners/Subscribers](https://github.com/otobus/event_bus/wiki/Creating-Event-Consumers)
- Creating a Configurable Event Consumer/Listener/Subscriber
- Subscribing Consumer/Listener/Subscriber for a Topic
- More...

**Documents**
- [EventBus Commands](https://github.com/otobus/event_bus/wiki/EventBus-Commands)
- [EventBus.Model.Event Data Structure](https://github.com/otobus/event_bus/wiki/EventBus.Model.Event-Data-Structure)

**Debugging**
- [EventBus Metrics and UI](https://github.com/otobus/event_bus/wiki/EventBus-Metrics-and-UI)
- [Tracing System Events](https://github.com/otobus/event_bus/wiki/Tracing-System-Events)

**Architecture**
- [Workflow](https://github.com/otobus/event_bus/wiki/Workflow)
- [ETS Storage Details](https://github.com/otobus/event_bus/wiki/ETS-Storage-Details)

**Extensions**
- [Addons](https://github.com/otobus/event_bus/wiki/Addons)

**Samples**
- [Libraries using EventBus](https://hex.pm/packages?search=depends%3Aevent_bus)
- [Using EventBus with GenStage](https://github.com/otobus/event_bus_postgres)

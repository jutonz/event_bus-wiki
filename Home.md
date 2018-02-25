EventBus library is a traceable, extendable and minimalist event bus implementation for Elixir with built-in event store and event watcher based on ETS. It allows basic pub/sub for internal process communication. It is designed for low memory footprint and speed. 

EventBus comes with an efficient solution to internal process communication. Unlike other EventBus implementations EventBus doesn't deliver the event data directly to its subscribers. It delivers the identifier of the event and topic name which can be queued with minimal memory footprint. With this behaviour, subscriber query the event data when they are ready to process the event. 

This wiki includes documentation and samples related to EventBus library and its behaviours. 

# Table of Contents

**Usage**
- [EventBus Commands](https://github.com/otobus/event_bus/wiki/EventBus-Commands)
- [EventBus.Model.Event Data Structure](https://github.com/otobus/event_bus/wiki/EventBus.Model.Event-Data-Structure)
- [Tracing System Events](https://github.com/otobus/event_bus/wiki/Tracing-System-Events)

**Architecture**
- [Workflow](https://github.com/otobus/event_bus/wiki/Workflow)
- [ETS Storage Details](https://github.com/otobus/event_bus/wiki/ETS-Storage-Details)
- Pub/Sub

**Extensions**
- [Addons](https://github.com/otobus/event_bus/wiki/Addons)

**Samples**
- [Libraries using EventBus](https://hex.pm/packages?search=depends%3Aevent_bus)
- Using EventBus with GenStage

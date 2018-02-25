## Attributes

`%EventBus.Model.Event{}` struct type consist of 8 attributes including 3 attributes (`id`, `topic` and `data`) as mandatory and 5 attributes (`transaction_id`, `initialized_at`, `occurred_at`, `ttl` and `source`) as optional.

### Mandatory Attributes

**`id`**

Identifier for the `Event` struct type, it must be unique in all over the system. It can be a type of `String.t()` or `integer()`. A good way to generate it is to use a unique identifier generator like `UUID.uuid4()` function.

**`topic`**

The topic name for the `Event` struct type. It can be a type of `atom()`. As the best practice of event-driven development, it is a good convention to set the topic name as past tense of actions like `:user_created`, `:email_sent`, or `webhook_received`.

**`data`**

The event data, it can be `any()` type. It might be considered as an aggregated content of an event. For example,
if you created a user, then the `User` struct might be a good candidate for the data attribute.

---

### Optional Attributes

**`initialized_at`**

Optional, but good to have the field for all events to track when the event generator started to process for generating this event. It can be a type of `integer()`.

**`occurred_at`**

Optional, but good to have the field for all events to track when the event occurred with unixtimestamp value. The library does not automatically set this value since the value depends on the timing choice. It can be a type of `integer()`.

**`source`**

Source attribute is used to determine the source of the Event. It usually answers the question `who created this event`.

**`transaction_id`**

The transaction identifier attribute is an optional field for the struct. It can be a type of `String.t()` or `integer()`. If you need to store any meta-identifier related to event transaction, it is the place to store. Secondly, `transaction_id` is one of the good ways to track events related to the same transaction on a chain of events. If you have time, have a look at the [story](https://hackernoon.com/trace-monitor-chain-of-microservice-logs-in-the-same-transaction-f13420f2d42c). 

**`ttl`**

Optional, but might have the field for all events to invalidate an event after a certain amount of time. Currently, the `event_bus` library does not do any specific thing using this field. If you need to discard an event in a certain amount of time, that field would be very useful. It can be a type of `integer()`.

Note: If you set the `ttl` field, then `occurred_at` field also becomes a required field.
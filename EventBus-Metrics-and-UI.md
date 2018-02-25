# EventBus.Metrics

Metrics library and metrics RESTFul endpoints for `event_bus`

## Installation

The package can be installed by adding `event_bus_metrics` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:event_bus_metrics, "~> 0.2"}
  ]
end
```

Update your `config.exs`

```elixir
config :event_bus_metrics,
  cross_origin: {:system, "EB_CROSS_ORIGIN", "off"},
  http_server: {:system, "EB_HTTP_SERVER", "off"},
  http_server_port: {:system, "PORT", "4000"}
```

## Endpoints

In your `router.ex`, forward an unused path to `EventBus.Metrics.Web.Router` like below:

```elixir
forward "/event_bus", to: EventBus.Metrics.Web.Router
```

### UI

HTTP Path for UI: `/event_bus/ui/`

To open and use the UI open the `https://{your_host_name}/event_bus/ui/` and put `https://{your_host_name}/event_bus` to endpoint url on the openned page. 

### Topics

`GET /event_bus/topics` # All topic metrics

`GET /event_bus/topics/:topic_name` # Specific topic metrics

`GET /event_bus/topics/:topic_name/events` # Topic event metrics (only unprocessed events)

`GET /event_bus/topics/:topic_name/events/:event_id` # Event details (only unprocessed events)

### Subscribers

`GET /event_bus/subscribers` # All subscriber metrics

### Server-Sent-Events (stream live metrics)

`GET /event_bus/sse` # Streaming endpoint for metrics

## Screen Shots

- [Node setting](https://drive.google.com/open?id=1bGYRe_QDUCjnRsmUSiF_LQNCuvDt_712)

- [List of topics](https://drive.google.com/open?id=16sKULRj00_OeWGcAxunQTE7NkDkar4tm)

- [Topic details](https://drive.google.com/open?id=13kWW133A_l1vFf8mzeZfPDxvDXAYe89S)
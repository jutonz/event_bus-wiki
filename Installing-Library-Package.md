### Add package to your project `Mixfile`:
The library package can be installed by adding `event_bus` to your list of dependencies in `mix.exs` file:

```elixir
def deps do
  [
     # ...,
     {:event_bus, "~> 1.5.1"}
  ]
end
```

### Fetch the package as usual

In your project's root directory, run `mix deps.get` shell command to fetch the library from hex packages. On successful fetch you will see an output like:
```shell
Resolving Hex dependencies...
Dependency resolution completed:
New:
  event_bus 1.5.1
* Getting event_bus (Hex package)

```

### Start the `event_bus` app before your app

Add `:event_bus` to your `mix.exs` file's `application/0` function inside the `applications` list:

```elixir
def application do
  [
    applications: [
      # ...,
      :event_bus
    ]
  ]
end
```

Congratulations!!! You have successfully installed the `event_bus` library. The next step is defining and creating the topic names which will be used in your project.
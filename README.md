# OpenApiSpexTypedStruct

Automatically generate api specs for your typed structs. 

This library is a plugin for [OpenApiSpex](https://github.com/open-api-spex/open_api_spex) that allows you to define typed structs and have the schema automatically generated for you. You can then easily reference these schemas in your OpenApiSpex operations. This allows you to keep your api specs in sync with your typed structs without having to constantly update two different versions of what is effectively the same schema. 

- Give your struct a title and you're all set, the rest is generated for you.
- You can optionally give `default` values to your fields and they will also be included in the schema.
- Same for `example`s. Can be combined with default values.
- Additionally, you can override the generated schema property for specific fields by providing a
  `property` option to the field. This is for when you want to reference another schema.

## Usage

```elixir
defmodule MySpec do
  use TypedStruct

  typedstruct do
    plugin TypedSpec, title: "my spec"
    field :id, :string, default: "123", example: "456"
    field :qty, :integer
    field :other_schema, :object, property: MyOtherSchema
  end
end
```

Generates a schema that looks like:
```elixir
%OpenApiSpex.Schema%{
  properties: %{
    id: %OpenApiSpex.Schema%{type: :string, default: "123", example: "456"},
    qty: %OpenApiSpex.Schema%{type: :integer},
    other_schema: OtherSchema
  },
  required: [:id, :qty],
  title: "my spec",
  type: :object
}
```

That can then be used with OpenApiSpex in your controller:
```elixir
operation(:index,
  summary: "Get fancy new spec-ed things",
  responses: [
    ok: {"Successful", "application/json", MySpec},
  ]
)
```

## Installation

If [available in Hex](https://hex.pm/docs/publish), the package can be installed
by adding `open_api_spex_typed_struct` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:open_api_spex_typed_struct, "~> 0.1.0"}
  ]
end
```

Documentation can be generated with [ExDoc](https://github.com/elixir-lang/ex_doc)
and published on [HexDocs](https://hexdocs.pm). Once published, the docs can
be found at <https://hexdocs.pm/open_api_spex_typed_struct>.


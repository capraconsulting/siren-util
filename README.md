# siren-util

[![CircleCI](https://circleci.com/gh/capraconsulting/siren-util.svg?style=svg&circle-token=b3a05ec51ec2d53208a202f77edad5048d47bc67)](https://circleci.com/gh/capraconsulting/siren-util)

Build and parse [Siren hypermedia](https://github.com/kevinswiber/siren)
compliant JSON representations of entities.

## Example

```java
Map<Object, Object> properties = new LinkedHashMap<>();
properties.put("orderNumber", 42);
properties.put("itemCount", 3);
properties.put("status", "pending");

// Building.
Root siren = Root
    .newBuilder()
    .clazz("order")
    .properties(properties)
    .links(
        Link.newBuilder("self", URI.create("https://example.com/orders/42")).build()
    )
    .build();

String json = siren.toJson();

// Parsing.
Root parsed = Root.fromJson(json);
if (parsed.getProperties() != null) {
    parsed.getProperties().get("status");
}
```

Result:

```json
{"class":["order"],"properties":{"orderNumber":42,"itemCount":3,"status":"pending"},"links":[{"rel":["self"],"href":"https://example.com/orders/42"}]}
```

See tests for more examples.

## Contributing

### Snapshot tests

Files in `src/test/resources` that ends with `.snapshot` are generated by
the tests and updated automatically by setting `REGENERATE_SNAPSHOTS=true`
system property.

Example:

```bash
mvn test -DREGENERATE_SNAPSHOTS=true
```

When a snapshot fails the easiest way to compare the changes are by regenering
snapshots and using Git diff.

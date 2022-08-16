# Apollo Federation Example

This is a lightweight example of how using a data loader from within the `.resolve_reference` method does not work.

Setup
```shell
bundle install
```

Run
```shell
bundle exec ruby lib/products.rb
```

Test
```shell
curl --location --request POST 'http://localhost:8080/graphql' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data-raw '{"query":"query($representations: [_Any!]!) {\n    _entities(representations: $representations){\n        __typename\n        ...on Product\n        {\n            upc\n            name\n            price\n            weight\n        }\n    }\n}","variables":{"representations":[{"__typename":"Product","id":"1"},{"__typename":"Product","id":"2"}]}}'
```

Then check the logs from the server

Actual output:
```
Product.resolve_reference reference: {:__typename=>"Product", :id=>"1"}
ProductsByUpc.fetch ["1"]
Product.resolve_reference reference: {:__typename=>"Product", :id=>"2"}
ProductsByUpc.fetch ["2"]
```

Expected output:
```
Product.resolve_reference reference: {:__typename=>"Product", :id=>"1"}
Product.resolve_reference reference: {:__typename=>"Product", :id=>"2"}
ProductsByUpc.fetch ["1", "2"]
```

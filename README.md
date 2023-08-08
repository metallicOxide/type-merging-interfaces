# Why

This repo is to highlight a potential issue on `graphql-tools/stitch` where inline fragments are not getting resolved in the subschema.

## Setup

```shell
yarn install
yarn start
```

The following service is available for interactive queries:

- **Stitched gateway:** http://localhost:4000/graphql

For simplicity, all subservices in this example are run locally by the gateway server. You could easily break out any subservice into a standalone remote server following the [combining local and remote schemas](../combining-local-and-remote-schemas) example.

## Summary

Visit the [stitched gateway](http://localhost:4000/graphql) and try running the following query:

```graphql
query test {
  storefront(id: "1") {
    name
    __typename
    productOfferings {
      name
      __typename
      ... on Product {
        __typename
        fieldForProduct
      }
    }
  }
}
```

The result of the above query is

```
{
  "data": {
    "storefront": {
      "name": "eShoppe",
      "__typename": "Storefront",
      "productOfferings": [
        {
          "name": "iPhone",
          "__typename": "Product",
          "fieldForProduct": null
        },
        {
          "name": "iPhone + Survival Guide",
          "__typename": "ProductDeal"
        },
        {
          "name": "Apple Watch",
          "__typename": "Product",
          "fieldForProduct": null
        }
      ]
    }
  }
}
```

Where the `fieldForProduct` is null even on `Product`

Using the following versions of `@graphql/stitch` and `@graphql-tools/schema` makes populates the `fieldForProduct` with `lala` as expected

```
    "@graphql-tools/schema": "^7.0.0",
    "@graphql-tools/stitch": "^7.1.0",
```

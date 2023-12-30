# after modifying data model

---

## When adding a new field

[JavaScript client code generation](https://docs.amplify.aws/javascript/build-a-backend/graphqlapi/client-code-generation/)

E.g. by adding field `lowerCaseName` in graphql schema below, file schema.graphql:

```graphql
type Share003DynUser001Detail
@model
@auth(rules: [{ allow: groups, groups: ["authenticated"] }])
{
    id: ID!
    name: String!
    lowerCaseName: String!
    shortName: String!
    uid: String!
}
```

In order to have mutations and queries like `listShare003DynUser001Details` up to date with new added field available:

```console
amplify codegen
amplify push
```

Then, check in `queries.js` that field is now available in query:

```js
export const listShare003DynUser001Details = /* GraphQL */ `
  query ListShare003DynUser001Details(
    $filter: ModelShare003DynUser001DetailFilterInput
    $limit: Int
    $nextToken: String
  ) {
    listShare003DynUser001Details(
      filter: $filter
      limit: $limit
      nextToken: $nextToken
    ) {
      items {
        id
        name
        lowerCaseName
        shortName
        uid
        createdAt
        updatedAt
        __typename
      }
      nextToken
      __typename
    }
  }
`;
```

---

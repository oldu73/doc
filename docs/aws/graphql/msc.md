# msc

---

## Readers array

How to have an array of authorized readers in a graphql schema?

Based on: - [GraphQL Authorization Schemas for AWS Amplify](https://iamondemand.com/blog/6-graphql-authorization-schemas-for-aws-amplify/)

Here is a tested solution that works:

```graphql
type ToDo 
@model
@auth(rules: [ # owner-based authorization
    { allow: owner }, # defines that the creator of the todo is authorized to read, edit, and delete it
    { allow: owner, ownerField: "readers", operations: [read]} # defines that all users in the readers field of the todo have permission to view it
])
{
    id: ID!
    content: String
    readers: [String] # String array to configure multi-readers authorizations
}
```

---

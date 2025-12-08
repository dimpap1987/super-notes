- [[Api]]
- [[Rest]] vs [[gRPC]]

## Key Concepts
- Single endpoint (usually `/graphql`)
- Client specifies required fields
- Strongly typed schema
- Query language for APIs
- Self-documenting with introspection

## Operations

### Query
```graphql
query {
  user(id: "1") {
    name
    email
    posts {
      title
      comments {
        text
      }
    }
  }
}
```

### Mutation
```graphql
mutation {
  createUser(input: {
    name: "John"
    email: "john@example.com"
  }) {
    id
    name
  }
}
```

### Subscription
```graphql
subscription {
  messageAdded(channelId: "123") {
    id
    text
    author {
      name
    }
  }
}
```

## Advantages
- Fetch only needed data (solves over-fetching)
- Single request for multiple resources
- Type safety with schema
- Introspection for documentation
- Strong developer experience

## Disadvantages
- Caching complexity (HTTP caching harder)
- Over-fetching prevention can be overkill
- Learning curve
- N+1 query problem without proper resolvers
- File uploads require extensions

## Best Practices
- Use DataLoader for batching
- Implement query complexity limits
- Use pagination (cursor-based)
- Validate query depth
- Use persisted queries for production


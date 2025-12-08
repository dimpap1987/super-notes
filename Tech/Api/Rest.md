- [[Api]]
- [[GraphQL]] vs [[gRPC]]

## REST Principles
- **Stateless** - each request contains all info needed
- **Client-server** - separation of concerns
- **Uniform interface** - consistent resource identification
- **Cacheable** - responses can be cached
- **Layered system** - intermediaries (proxies, gateways)

## HTTP Methods
- **GET** - retrieve resource (safe, idempotent)
- **POST** - create resource (not idempotent)
- **PUT** - update/replace resource (idempotent)
- **PATCH** - partial update (idempotent)
- **DELETE** - remove resource (idempotent)

## Status Codes

### Success (2xx)
- `200 OK` - successful GET, PUT, PATCH
- `201 Created` - resource created
- `204 No Content` - successful DELETE

### Client Error (4xx)
- `400 Bad Request` - invalid request
- `401 Unauthorized` - authentication required
- `403 Forbidden` - insufficient permissions
- `404 Not Found` - resource doesn't exist
- `409 Conflict` - resource conflict
- `422 Unprocessable Entity` - validation error

### Server Error (5xx)
- `500 Internal Server Error` - server error
- `502 Bad Gateway` - upstream server error
- `503 Service Unavailable` - temporary unavailable

## Best Practices
- Use proper HTTP verbs
- [[Versioning]]
- [[Api#Idempotence key|Idempotence key]] for POST operations
- [[Api#backward-compatible|Backward-compatible]] changes
- Use nouns for resources, not verbs
- Return appropriate status codes
- Include error details in response body
- Use pagination for collections
- Support filtering, sorting, field selection

## Example
```http
GET /api/v1/users?page=1&limit=10&sort=name
Authorization: Bearer <token>

200 OK
{
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 100
  }
}
```


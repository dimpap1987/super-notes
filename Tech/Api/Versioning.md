- [[Api]]
- [[Rest]]

## Strategies

### URL Versioning (Most Common)
```
/api/v1/users
/api/v2/users
```
- **Pros**: Clear, cacheable, easy to understand
- **Cons**: URL pollution

### Header Versioning
```http
Accept: application/vnd.api+json;version=1
API-Version: 2
```
- **Pros**: Clean URLs
- **Cons**: Less discoverable, caching complexity

### Query Parameter
```
/api/users?version=1
```
- **Pros**: Simple
- **Cons**: Not RESTful, can conflict with query params

## When to Version
- Breaking changes to existing endpoints
- Removing fields
- Changing field types
- Changing required fields
- Changing response structure

## Best Practices
- Version early (before public release)
- Maintain backward compatibility
- Deprecate gracefully (with timeline)
- Document breaking changes
- Use semantic versioning (v1, v2)
- Support multiple versions during transition
- Communicate deprecation timeline
- Provide migration guides

## Deprecation Example
```http
GET /api/v1/users
Deprecation: true
Sunset: Sat, 31 Dec 2024 23:59:59 GMT
Link: <https://api.example.com/docs/v2/migration>; rel="deprecation"
```


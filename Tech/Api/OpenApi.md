- [[Api]]
- [[Rest]]

## Overview
- Specification for RESTful APIs (formerly Swagger)
- Machine-readable API documentation
- Code generation support
- Contract-first API development

## Benefits
- Interactive API documentation (Swagger UI)
- Client SDK generation
- API testing tools
- Contract validation
- Mock server generation
- API design validation

## Structure
```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0
paths:
  /users:
    get:
      summary: List users
      parameters:
        - name: page
          in: query
          schema:
            type: integer
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
```

## Tools
- **Swagger UI** - interactive documentation
- **Swagger Editor** - spec editor
- **OpenAPI Generator** - code generation
- **Postman** - import/export OpenAPI specs

## Best Practices
- Version your API spec
- Include examples
- Document all endpoints
- Define error responses
- Use reusable components
- Validate spec before publishing


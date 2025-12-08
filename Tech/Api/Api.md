- [[Tech Fundamentals]]

## API Types
- [[Rest]] vs [[GraphQL]] vs [[gRPC]]
- [[Oauth2]]
- [[OpenApi]]

## Core Concepts

### Idempotence Key
- Ensures operations can be safely retried
- Client sends unique key with request
- Server stores key, returns same response for duplicate requests
- Prevents duplicate charges, double-processing
- Example: Payment processing, order creation

#### HTTP Header Example
```http
POST /api/orders
Idempotency-Key: 550e8400-e29b-41d4-a716-446655440000
```

#### JavaScript Client Example
```javascript
// Generate idempotency key
function generateIdempotencyKey() {
    return crypto.randomUUID(); // or use uuid library
}

// Make idempotent request
async function createOrder(orderData) {
    const idempotencyKey = generateIdempotencyKey();
    
    try {
        const response = await fetch('/api/orders', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Idempotency-Key': idempotencyKey
            },
            body: JSON.stringify(orderData)
        });
        
        return await response.json();
    } catch (error) {
        // Retry with same key - safe to retry
        return createOrder(orderData); // Uses same key
    }
}

// Usage
const order = await createOrder({
    productId: '123',
    quantity: 2,
    price: 99.99
});
```

#### JavaScript Server Example (Express)
```javascript
const express = require('express');
const app = express();

// Store idempotency keys (use Redis in production)
const idempotencyStore = new Map();

app.post('/api/orders', async (req, res) => {
    const idempotencyKey = req.headers['idempotency-key'];
    
    if (!idempotencyKey) {
        return res.status(400).json({ error: 'Idempotency-Key header required' });
    }
    
    // Check if we've seen this key before
    if (idempotencyStore.has(idempotencyKey)) {
        const cachedResponse = idempotencyStore.get(idempotencyKey);
        return res.status(cachedResponse.status).json(cachedResponse.body);
    }
    
    // Process order
    try {
        const order = await processOrder(req.body);
        const response = {
            status: 201,
            body: order
        };
        
        // Store response for future requests
        idempotencyStore.set(idempotencyKey, response);
        
        // Expire after 24 hours
        setTimeout(() => {
            idempotencyStore.delete(idempotencyKey);
        }, 24 * 60 * 60 * 1000);
        
        res.status(201).json(order);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});
```

### Backward Compatibility
- New API versions work with old clients
- Add fields, don't remove required fields
- Don't change existing field types
- Use optional fields for new features
- Deprecate old fields gradually

### Versioning
- [[Versioning]] strategies
- Version early, maintain compatibility
- Document breaking changes

## Security
- [[Security]] - XSS, CSRF protection
- [[Oauth2]] - Authentication & authorization
- Encryption - jasypt for property encryption

## Related Topics
- [[Hibernate]] - ORM framework
- [[CQRS]] - Command Query Separation
- [[Akka Actors]] - Actor model concurrency
- [[Workflow Engines]] - Business process automation
- [[Code Obfuscators]] - Code protection and minification
- [[SonarQube]] - Code quality analysis
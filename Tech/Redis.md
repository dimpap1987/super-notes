- [[Tech Fundamentals]]

## Overview
- In-memory data structure store
- Key-value database
- Supports multiple data structures
- High performance (sub-millisecond latency)
- Used as cache, database, message broker

## Core Concepts

### Key-Value Store
- Keys are strings
- Values can be strings, hashes, lists, sets, etc.
- Keys can expire (TTL)
- Atomic operations

### Data Structures
- **Strings** - simple key-value
- **Hashes** - field-value maps
- **Lists** - ordered collections
- **Sets** - unordered unique collections
- **Sorted Sets** - ordered sets with scores
- **Streams** - log-like data structure
- **Bitmaps** - bit operations
- **HyperLogLog** - cardinality estimation

## Basic Operations

### Strings
```bash
# Set value
SET user:1 "John Doe"

# Get value
GET user:1

# Set with expiration (seconds)
SETEX user:1 3600 "John Doe"
SET user:1 "John Doe" EX 3600

# Set if not exists
SETNX user:1 "John Doe"

# Increment/Decrement
INCR counter
DECR counter
INCRBY counter 5

# Append
APPEND user:1 " - Admin"

# Get multiple
MGET user:1 user:2 user:3
```

### Hashes
```bash
# Set hash field
HSET user:1 name "John" age 30 email "john@example.com"

# Get hash field
HGET user:1 name

# Get all fields
HGETALL user:1

# Get multiple fields
HMGET user:1 name email

# Check if field exists
HEXISTS user:1 name

# Delete field
HDEL user:1 age

# Get all keys
HKEYS user:1

# Get all values
HVALS user:1

# Increment numeric field
HINCRBY user:1 age 1
```

### Lists
```bash
# Push to left (head)
LPUSH queue "task1"
LPUSH queue "task2" "task3"

# Push to right (tail)
RPUSH queue "task4"

# Pop from left
LPOP queue

# Pop from right
RPOP queue

# Get range
LRANGE queue 0 -1  # All elements
LRANGE queue 0 2   # First 3 elements

# Get length
LLEN queue

# Get element by index
LINDEX queue 0

# Blocking pop (wait for element)
BLPOP queue 10  # Wait 10 seconds
```

### Sets
```bash
# Add members
SADD tags "redis" "database" "cache"

# Get all members
SMEMBERS tags

# Check membership
SISMEMBER tags "redis"

# Remove member
SREM tags "cache"

# Get size
SCARD tags

# Intersection
SINTER set1 set2

# Union
SUNION set1 set2

# Difference
SDIFF set1 set2

# Random member
SRANDMEMBER tags
SPOP tags  # Remove and return random
```

### Sorted Sets
```bash
# Add with score
ZADD leaderboard 100 "player1"
ZADD leaderboard 200 "player2" 150 "player3"

# Get score
ZSCORE leaderboard "player1"

# Get rank (0-based, lowest to highest)
ZRANK leaderboard "player1"

# Get reverse rank (highest to lowest)
ZREVRANK leaderboard "player1"

# Get range by rank
ZRANGE leaderboard 0 2  # Top 3
ZREVRANGE leaderboard 0 2  # Bottom 3

# Get range by score
ZRANGEBYSCORE leaderboard 100 200

# Increment score
ZINCRBY leaderboard 50 "player1"

# Count members in score range
ZCOUNT leaderboard 100 200

# Remove members
ZREM leaderboard "player1"
```

### Keys Management
```bash
# Check if key exists
EXISTS user:1

# Delete key
DEL user:1

# Delete multiple
DEL user:1 user:2 user:3

# Set expiration (seconds)
EXPIRE user:1 3600

# Set expiration (milliseconds)
PEXPIRE user:1 3600000

# Set expiration at timestamp
EXPIREAT user:1 1735689600

# Remove expiration
PERSIST user:1

# Get TTL
TTL user:1  # Returns seconds, -1 if no expiry, -2 if doesn't exist

# Get keys matching pattern
KEYS user:*

# Scan keys (safer than KEYS)
SCAN 0 MATCH user:* COUNT 100
```

## JavaScript Examples

### Using Redis Client (node-redis)
```javascript
const redis = require('redis');
const client = redis.createClient({
    url: 'redis://localhost:6379'
});

await client.connect();

// Strings
await client.set('user:1', 'John Doe');
const user = await client.get('user:1');

// With expiration
await client.setEx('session:123', 3600, 'session-data');

// Hashes
await client.hSet('user:1', {
    name: 'John',
    age: '30',
    email: 'john@example.com'
});
const userData = await client.hGetAll('user:1');

// Lists
await client.lPush('queue', 'task1', 'task2');
const task = await client.rPop('queue');

// Sets
await client.sAdd('tags', 'redis', 'database');
const tags = await client.sMembers('tags');

// Sorted Sets
await client.zAdd('leaderboard', {
    score: 100,
    value: 'player1'
});
const topPlayers = await client.zRange('leaderboard', 0, 2, {
    REV: true
});

// Transactions
const multi = client.multi();
multi.set('key1', 'value1');
multi.set('key2', 'value2');
multi.exec();
```

### Pub/Sub (Publish-Subscribe)
```javascript
// Publisher
const publisher = redis.createClient();
await publisher.connect();

await publisher.publish('channel', 'Hello World');

// Subscriber
const subscriber = redis.createClient();
await subscriber.connect();

await subscriber.subscribe('channel', (message) => {
    console.log('Received:', message);
});
```

### Streams (Message Queue)
```javascript
// Producer
await client.xAdd('orders', '*', {
    orderId: '123',
    amount: '99.99'
});

// Consumer
const messages = await client.xRead(
    { key: 'orders', id: '$' },
    { BLOCK: 1000, COUNT: 10 }
);

// Consumer Group
await client.xGroupCreate('orders', 'mygroup', '$', {
    MKSTREAM: true
});

const messages = await client.xReadGroup(
    'mygroup',
    'consumer1',
    [
        { key: 'orders', id: '>' }
    ],
    { BLOCK: 1000, COUNT: 10 }
);

// Acknowledge
await client.xAck('orders', 'mygroup', messageId);
```

## Common Patterns

### Caching
```javascript
async function getCachedUser(userId) {
    const cached = await client.get(`user:${userId}`);
    
    if (cached) {
        return JSON.parse(cached);
    }
    
    // Fetch from database
    const user = await db.getUser(userId);
    
    // Cache for 1 hour
    await client.setEx(
        `user:${userId}`,
        3600,
        JSON.stringify(user)
    );
    
    return user;
}
```

### Rate Limiting
```javascript
async function rateLimit(userId, limit, window) {
    const key = `rate_limit:${userId}`;
    const current = await client.incr(key);
    
    if (current === 1) {
        await client.expire(key, window);
    }
    
    return current <= limit;
}

// Usage
const allowed = await rateLimit('user:123', 10, 60); // 10 requests per minute
```

### Distributed Lock
```javascript
async function acquireLock(lockKey, timeout = 10) {
    const identifier = crypto.randomUUID();
    const result = await client.set(
        lockKey,
        identifier,
        {
            EX: timeout,
            NX: true  // Only set if not exists
        }
    );
    
    return result ? identifier : null;
}

async function releaseLock(lockKey, identifier) {
    const script = `
        if redis.call("get", KEYS[1]) == ARGV[1] then
            return redis.call("del", KEYS[1])
        else
            return 0
        end
    `;
    
    return await client.eval(script, {
        keys: [lockKey],
        arguments: [identifier]
    });
}
```

### Session Storage
```javascript
async function setSession(sessionId, data, ttl = 3600) {
    await client.setEx(
        `session:${sessionId}`,
        ttl,
        JSON.stringify(data)
    );
}

async function getSession(sessionId) {
    const data = await client.get(`session:${sessionId}`);
    return data ? JSON.parse(data) : null;
}
```

### Leaderboard
```javascript
async function updateScore(playerId, points) {
    await client.zIncrBy('leaderboard', points, playerId);
}

async function getTopPlayers(limit = 10) {
    return await client.zRange('leaderboard', 0, limit - 1, {
        REV: true,
        WITHSCORES: true
    });
}

async function getPlayerRank(playerId) {
    return await client.zRevRank('leaderboard', playerId);
}
```

## Best Practices

### Key Naming
- Use colons for hierarchy: `user:1:profile`
- Be consistent: `user:1` not `user_1` or `User:1`
- Include namespace: `app:user:1`

### Expiration
- Set TTL for temporary data
- Use `SETEX` or `EXPIRE` for automatic cleanup
- Refresh TTL on access for sliding expiration

### Memory Management
- Monitor memory usage
- Use appropriate data structures
- Set `maxmemory` and eviction policy
- Use `SCAN` instead of `KEYS` for large datasets

### Performance
- Use pipelines for multiple commands
- Use transactions (MULTI/EXEC) when needed
- Connection pooling
- Use Lua scripts for complex operations

### Security
- Use AUTH password
- Bind to specific interfaces
- Use SSL/TLS in production
- Disable dangerous commands (FLUSHDB, CONFIG)

## Lua Scripts

### Atomic Operations
```lua
-- Increment and get if exists
local current = redis.call('GET', KEYS[1])
if current then
    return redis.call('INCR', KEYS[1])
else
    return nil
end
```

```javascript
const script = `
    local current = redis.call('GET', KEYS[1])
    if current then
        return redis.call('INCR', KEYS[1])
    else
        return nil
    end
`;

const result = await client.eval(script, {
    keys: ['counter'],
    arguments: []
});
```

## Configuration

### Common Settings
```conf
# Memory limit
maxmemory 2gb
maxmemory-policy allkeys-lru

# Persistence
save 900 1      # Save if 1+ keys changed in 900s
save 300 10     # Save if 10+ keys changed in 300s
save 60 10000   # Save if 10000+ keys changed in 60s

# AOF (Append Only File)
appendonly yes
appendfsync everysec

# Network
bind 127.0.0.1
port 6379
requirepass yourpassword
```

## Use Cases
- **Caching** - Fast data access
- **Session Storage** - User sessions
- **Rate Limiting** - API throttling
- **Leaderboards** - Sorted sets
- **Message Queue** - Lists or Streams
- **Pub/Sub** - Real-time notifications
- **Distributed Locks** - Coordination
- **Counting** - Page views, likes
- **Real-time Analytics** - Counters, sets


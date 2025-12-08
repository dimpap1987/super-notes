- [[Tech Fundamentals]]

## Overview
- Message queue for asynchronous communication
- Decouples producers and consumers
- Enables reliable message delivery
- Supports distributed systems

## Core Concepts

### Sequential Processing
- Messages processed **one at a time** per consumer
- FIFO (First In, First Out) order
- Each message consumed by **one consumer only**
- Next consumer reads **after current message is acknowledged**

### Reading from Queue

#### Basic Read (POP)
```javascript
// Redis example
const message = await redis.rpop('queue:orders');
// Message removed from queue immediately
```

#### Blocking Read (BLPOP)
```javascript
// Wait for message, blocks until available
const result = await redis.blpop('queue:orders', 10); // timeout 10s
const [queueName, message] = result;
```

### Message Lifecycle

1. **Producer** sends message → Queue
2. **Consumer** reads message → Message becomes "in-flight"
3. **Consumer** processes message
4. **Consumer** acknowledges (ACK) → Message removed
5. If no ACK → Message returns to queue (failure handling)

### Failure Handling

#### No Acknowledgment
- Message remains in queue
- Can be reprocessed by same or different consumer
- Prevents message loss

#### Acknowledgment Required
```javascript
// Redis Streams example
const messages = await redis.xreadgroup(
    'GROUP', 'mygroup', 'consumer1',
    'COUNT', 1,
    'BLOCK', 1000,
    'STREAMS', 'orders', '>'
);

// Process message
await processOrder(messages[0]);

// Acknowledge after processing
await redis.xack('orders', 'mygroup', messageId);
```

#### Failure Scenarios

**Consumer Crashes:**
- Message stays in "processing" state
- After timeout → returns to queue
- Another consumer can pick it up

**Processing Fails:**
- Consumer sends NACK (negative acknowledgment)
- Message returns to queue immediately
- Can set retry limit

**Timeout:**
- If processing takes too long
- Message automatically returns to queue
- Prevents stuck messages

### Redis Queue Types

#### List-based Queue (Simple)
```javascript
// Producer
await redis.lpush('queue:orders', JSON.stringify(order));

// Consumer
const message = await redis.brpop('queue:orders', 0);
// Process message
// No ACK needed - message removed on read
```

**Limitations:**
- No acknowledgment mechanism
- Message lost if consumer crashes
- No visibility into processing state

#### Redis Streams (Advanced)
```javascript
// Producer
await redis.xadd('orders', '*', 'order', JSON.stringify(order));

// Consumer Group
await redis.xgroup('CREATE', 'orders', 'mygroup', '$');

// Consumer reads
const messages = await redis.xreadgroup(
    'GROUP', 'mygroup', 'consumer1',
    'COUNT', 1,
    'BLOCK', 1000,
    'STREAMS', 'orders', '>'
);

// Process and ACK
await processOrder(messages[0]);
await redis.xack('orders', 'mygroup', messageId);
```

**Features:**
- Consumer groups
- Acknowledgment (ACK)
- Pending Entry List (PEL) for unacknowledged messages
- Message IDs for ordering
- Claiming messages from failed consumers

### Consumer Behavior

#### Single Consumer
- Reads messages sequentially
- Processes one at a time
- ACKs before reading next

#### Multiple Consumers
- **Same consumer group**: Messages distributed (load balancing)
- **Different groups**: Each group gets all messages (broadcast)
- **Round-robin** distribution within group

### Example: Order Processing

```javascript
// Producer
async function enqueueOrder(order) {
    await redis.xadd('orders', '*', 
        'orderId', order.id,
        'data', JSON.stringify(order)
    );
}

// Consumer
async function processOrders() {
    while (true) {
        const messages = await redis.xreadgroup(
            'GROUP', 'order-processors', 'worker1',
            'COUNT', 1,
            'BLOCK', 1000,
            'STREAMS', 'orders', '>'
        );
        
        if (messages && messages.length > 0) {
            const [stream, messageList] = messages[0];
            
            for (const [messageId, fields] of messageList) {
                try {
                    const order = JSON.parse(fields[1]);
                    await processOrder(order);
                    
                    // ACK successful processing
                    await redis.xack('orders', 'order-processors', messageId);
                } catch (error) {
                    // NACK - return to queue
                    await redis.xack('orders', 'order-processors', messageId);
                    // Or leave unacked for retry
                }
            }
        }
    }
}
```

### Key Properties

#### At-Least-Once Delivery
- Message delivered at least once
- Possible duplicates if ACK fails
- Requires idempotent processing

#### Exactly-Once Delivery
- More complex, requires transactions
- Prevents duplicates
- Higher overhead

#### Ordering
- **FIFO**: Maintains order within partition
- **Partitioning**: Distributes across partitions
- **Global ordering**: Single partition (slower)

### Best Practices
- Always ACK after successful processing
- Handle failures gracefully (NACK or retry)
- Set appropriate timeouts
- Use consumer groups for load balancing
- Monitor pending messages
- Implement dead letter queue for failed messages
- Make consumers idempotent

### Common Patterns

#### Dead Letter Queue
- Failed messages after N retries
- Moved to separate queue
- Manual inspection and handling

#### Priority Queue
- Multiple queues by priority
- Consumer checks high-priority first
- Or use sorted sets in Redis

#### Delayed Messages
- Schedule messages for future processing
- Use Redis sorted sets with scores (timestamps)
- Consumer polls for ready messages


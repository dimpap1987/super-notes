- [[Api]]

## Overview
- Command Query Responsibility Segregation
- Separate read and write models

## Concepts
- **Command** - write operations (mutations)
- **Query** - read operations
- Different data stores for read/write
- Event sourcing often combined

## Benefits
- Optimize read/write independently
- Scale read/write separately
- Clear separation of concerns

## Trade-offs
- Increased complexity
- Eventual consistency
- More infrastructure


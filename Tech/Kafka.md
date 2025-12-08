- [[Tech Fundamentals]]

## Overview
- Distributed event streaming platform
- Publish-subscribe messaging system
- High throughput, fault-tolerant

## Key Concepts
- **Topic** - stream of records
- **Producer** - publishes messages
- **Consumer** - subscribes to topics
- **Broker** - Kafka server
- **Partition** - topic split for parallelism

## Use Cases
- Event sourcing
- Log aggregation
- Real-time data pipelines
- Microservices communication

## Consumer Groups
- Multiple consumers process partitions
- Each partition consumed by one consumer in group
- Enables horizontal scaling


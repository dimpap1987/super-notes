- [[Api]]

## Overview
- Actor model concurrency framework for Java/Scala
- Message-passing concurrency
- Isolated state, no shared mutable state
- Built-in fault tolerance and supervision

## Key Concepts

### Actor
- Encapsulates state and behavior
- Processes messages one at a time
- Communicates via asynchronous messages
- No shared state between actors

### Actor System
- Manages actors lifecycle
- Provides configuration
- Handles message routing

### Message Passing
- Asynchronous communication
- Fire-and-forget or request-response
- Type-safe message handling

## Example (Scala)
```scala
import akka.actor.{Actor, ActorSystem, Props}

class Greeter extends Actor {
  def receive = {
    case "hello" => println("Hello!")
    case _ => println("Unknown message")
  }
}

val system = ActorSystem("MySystem")
val greeter = system.actorOf(Props[Greeter], "greeter")
greeter ! "hello"
```

## Example (Java)
```java
public class Greeter extends AbstractActor {
    @Override
    public Receive createReceive() {
        return receiveBuilder()
            .matchEquals("hello", msg -> {
                System.out.println("Hello!");
            })
            .build();
    }
}

ActorSystem system = ActorSystem.create("MySystem");
ActorRef greeter = system.actorOf(Props.create(Greeter.class), "greeter");
greeter.tell("hello", ActorRef.noSender());
```

## Benefits
- No locks or synchronization needed
- Natural fault tolerance
- Scalable (millions of actors)
- Location transparency
- Backpressure handling

## Use Cases
- High-concurrency systems
- Event-driven architectures
- Microservices communication
- Real-time processing
- Distributed systems

## Supervision
- Parent actors supervise children
- Failure handling strategies:
  - Resume - continue processing
  - Restart - restart actor
  - Stop - terminate actor
  - Escalate - let parent handle


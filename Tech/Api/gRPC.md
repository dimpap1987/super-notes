- [[Api]]
- [[Rest]] vs [[GraphQL]]

## Overview
- RPC framework by Google
- Uses Protocol Buffers (protobuf)
- HTTP/2 based
- Strongly typed contracts
- Binary serialization

## Service Definition (protobuf)
```protobuf
syntax = "proto3";

service UserService {
  rpc GetUser (UserRequest) returns (User);
  rpc ListUsers (Empty) returns (stream User);
  rpc CreateUser (CreateUserRequest) returns (User);
}

message User {
  int32 id = 1;
  string name = 2;
  string email = 3;
}
```

## Streaming Types
- **Unary** - single request, single response
- **Server streaming** - single request, stream of responses
- **Client streaming** - stream of requests, single response
- **Bidirectional streaming** - stream both ways

## Advantages
- High performance (binary, HTTP/2)
- Streaming support (real-time)
- Language agnostic (code generation)
- Built-in code generation
- Strong typing with contracts
- Efficient for high-throughput

## Use Cases
- Microservices communication
- Real-time streaming
- High-performance APIs
- Mobile apps (reduced bandwidth)
- IoT devices

## Disadvantages
- Browser support limited (need gRPC-Web)
- Less human-readable than JSON
- Steeper learning curve
- Requires code generation step
- Debugging more complex

## Best Practices
- Use streaming for large datasets
- Implement proper error handling
- Use deadlines/timeouts
- Version your proto files
- Use interceptors for cross-cutting concerns


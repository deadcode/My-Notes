# What is gRPC
In gRPC, a client application can directly call a method on a server application on a different machine as if it were a local object, making it easier for you to create distributed applications and services. As in many RPC systems, gRPC is based around the idea of defining a service, specifying the methods that can be called remotely with their parameters and return types. On the server side, the server implements this interface and runs a gRPC server to handle client calls. On the client side, the client has a stub (referred to as just a client in some languages) that provides the same methods as the server.
[gRPC Introduction](https://grpc.io/docs/what-is-grpc/introduction/)

By default, gRPC uses [Protocol Buffers](https://protobuf.dev/overview), Google’s mature open source mechanism for serializing structured data (although it can be used with other data formats such as JSON).

The first step when working with protocol buffers is to define the structure for the data you want to serialize in a _proto file_: this is an ordinary text file with a `.proto` extension. Protocol buffer data is structured as _messages_, where each message is a small logical record of information containing a series of name-value pairs called _fields_.
Then, once you’ve specified your data structures, you use the protocol buffer compiler `protoc` to generate data access classes in your preferred language(s) from your proto definition. These provide simple accessors for each field, like `name()` and `set_name()`, as well as methods to serialize/parse the whole structure to/from raw bytes.

# Transport
* Always runs over [[HTTP2]]
* Always secure
# Types of APIs
* Unary API: 1 Request + 1 Reponse - similar to HTTP/1
* Server Streaming: 1 Req + Multiple Resp. use case - real time updates from server
* Client Streaming: Multiple Req + 1 Resp. use case - like big uploads
* Bi Dir Streaming: Multiple Req + Multiple Resp. There is no ordering between Req & Resp.

# Scalability
* Server side: always Async
* Client: can be Async or blocking

# gRPC vs REST

| Param        | gRPC                      | REST                                   |
| ------------ | ------------------------- | -------------------------------------- |
| Data         | Protocol Buffers / binary | JSON / plaintext                       |
| Schema       | Strict schema             | dynamic schema                         |
| Transport    | HTTP/2                    | HTTP/1                                 |
| Type of APIs | Streaming + Unary         | Unary Only                             |
| Direnction   | Bi Directional            | Uni Dir (Client -> Server)             |
| Actions      | Free form                 | Limited verbs (GET/POST/UPDATE/DELETE) |

# Links
* [gRPC Go Basic Tutorial](https://grpc.io/docs/languages/go/basics/)
* [gRPC course & Sample](https://github.com/Clement-Jean/grpc-go-course)
* [Evans CLI - Universal gRPC Client ](https://github.com/ktr0731/evans) - Implemented using gRPC Reflection capabilities
* [GRPC server Reflection Protocol](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md)
* [GRPC C++ Reflection and grcp_cli tool](https://grpc.github.io/grpc/cpp/md_doc_server_reflection_tutorial.html)
* [gRPC Curl](https://github.com/fullstorydev/grpcurl) - Use like Curl or Postman
* [Google gRPC Pub/Sub Service](https://github.com/googleapis/googleapis/blob/master/google/pubsub/v1/pubsub.proto)
* [Google gRPC Spanner APIs](https://github.com/googleapis/googleapis/blob/master/google/spanner/v1/spanner.proto)

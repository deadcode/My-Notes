# What is HTTP/2?

HTTP/2 is a replacement for how HTTP is expressed “on the wire.” It is **not** a ground-up rewrite of the protocol; HTTP methods, status codes and semantics are the same, and it should be possible to use the same APIs as HTTP/1.x (possibly with some small additions) to represent the protocol.

The focus of the protocol is on performance; specifically, end-user perceived latency, network and server resource usage. One major goal is to allow the use of a single connection from browsers to a Web site.

The basis of the work was [SPDY](http://tools.ietf.org/html/draft-mbelshe-httpbis-spdy-00), but HTTP/2 has evolved to take the community’s input into account, incorporating several improvements in the process.

# Specifications

HTTP/2 is comprised of two specifications:

- Hypertext Transfer Protocol version 2 - [RFC9113](https://httpwg.org/specs/rfc9113.html)
- HPACK - Header Compression for HTTP/2 - [RFC7541](https://httpwg.org/specs/rfc7541.html)

# HTTP/1 vs HTTP/2 Performance
* TCP connections: HTTP/1 uses separate TCP connection for each request. HTTP/2 single connection for multiple request.
* Header Compression: Plaintext headers in HTTP/1. HTTP/2 has binary header so they are compressed.
* Request / Response: in HTTP/1 only one Request / Response per connection. HTTP/2 support multiplexing of Request/Response on a connection.
* Server Push: HTTP/1 server can only respond to request but cannot push data. HTTP/2 supports push.
* Security: HTTP/2 requires SSL while HTTP/1 can be plaintext.

## Summary: Why HTTP/2 is better than HTTP/1
* Less Chatter: Multiple request/responses over fewer connections and server can push.
* Less Bandwidth: Binary headers mean less bandwidth and also fewer connections mean less overhead.
* More Secure: SSL is required so HTTP/2 is always secure.

# Links
* [Website](https://http2.github.io/)

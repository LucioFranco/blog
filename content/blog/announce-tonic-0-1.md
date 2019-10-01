---
title: "Tonic: gRPC has come to async/await!"
url: "/tonic-grpc-has-come-to-async-await"
date: 2019-10-01T10:00:00-04:00
draft: false
---

# Tonic: gRPC has come to async/await!

I am pleased to finally announce a crate that I have been working very hard on for the
past few months. [`tonic`] has finally hit the initial [`0.1.0-alpha.1`] release! Stable
releases will follow in the coming months.

### What is Tonic?

> Tonic is a gRPC over HTTP/2 implementation focused on _**high performance**_, _**interoperability**_, and _**flexibility**_. This library was created to have first class support of _async/await_ and to act as a core building block for production systems written in Rust.

Tonic's previous iteration was called [`tower-grpc`] and was built for the need to have a production
ready gRPC implementation for [linkerd]'s proxy. This spawned the creation of the [`tower`] library
which is now being used in production in both [`linkerd-proxy`] and [`vector`]. As well as being used in
research systems such as the [`noria`]database developed at MIT. The `tower` libraries have also seen a lot of maturing with the likes of [`warp`], [`tower-web`], [`tower-grpc`], and [`actix-net`] using them.

Now that async/await is going to land onto stable Rust very soon we have completely rewritten the
library to support the new syntax _natively_. This means client's out ofthe box will support
async/await and server implementations can be defined via [`async_trait`]. This provides an
unparalleled experience for writing async based services quickly and efficiently. Not only
does Tonic provide a fully featured gRPC implementation but it also comes with a
fully featured, batteries included, HTTP/2 client/server built around [`hyper`], [`tokio`]
and [`tower`]. Both the client and server implementation provide TLS backed by either [`openssl`]
or [`rustls`]. The client provides load balancing, interceptors, timeouts, rate limiting, concurrency
limiting and more!

The library also boasts strong interoperability with [`grpc-go`] which contains tests for the majority
of gRPC features. If you find a bug or would like to request a new feature please file an [issue]!

### Features

Out of the gate the goal for Tonic has been to provide a good batteries included experience. It
already supports many features with many more planned! Here is a list of features:

- Implemented in pure Rust (minus `openssl`)
- Interoperability tested via [`tonic-interop`]
- Bi-directional streaming
- Custom metadata
- Trailing metadata
- Codegen via [`prost`]
- Tracing support (more to come)
- Fully featured HTTP/2 Client/Server
- TLS backed by either [`openssl`] or [`rustls`]
- Load balancing powered by [`tower`]
- Timeouts, rate limiting, concurrency limiting, etc
- gRPC interceptors
- And others, with more being added

### Overview

_helloworld **client** example:_

{{< highlight rust >}}
let mut client = GreeterClient::connect("http://[::1]:50051")?;

let request = Request::new(HelloRequest {
    name: "hello".into(),
});

let response = client.say_hello(request).await?;

println!("RESPONSE={:?}", response);
{{< /highlight >}}

This shows how easy it is to build a simple gRPC client using generated code. Internally,
`connect` will create the most basic "channel" but it is possible to configure this to support
tls, load balancing, timeouts and more.


_helloworld **server** example:_

{{< highlight rust >}}
#[tonic::async_trait]
impl Greeter for MyGreeter {
    async fn say_hello(&self, req: Request<HelloRequest>)
        -> Result<Response<HelloReply>, Status>
    {
        println!("Got a request: {:?}", req);

        let reply = HelloReply {
            message: "Zomg, it works!".into(),
        };

        Ok(Response::new(reply))
    }
}
{{< /highlight >}}

Server's are just as easy to implement! Using [`dtolnay`]'s amazing [`async_trait`] crate
to allow server implementations to use `async fn` in a trait.

More examples including ones with tls, authentication, and streaming are included
in the [`tonic-examples`] crate.

### Moving forward

Today marks Tonic's first alpha release, over the next few months we will be working hard
to ensure that Tonic is ready for production usage. A lot of the implementation has already
been proven in production environments via [`tower-grpc`]. Over the next few months as [`tokio`]
comes closer to a full stable release Tonic will follow.

### Conclusion

[`tonic`] is now available! There is [documentation], [examples][`tonic-examples`] and
an [issue] tracker! Please provide any feedback as we would love to improve this library
anyway we can!

Thank you to all the super awesome contributors that have helped over the past few months! Your
feedback has been immensely important and valuable!


[`tonic`]: https://github.com/hyperium/tonic
[issue]: https://github.com/hyperium/tonic/issues/new
[`hyper`]: https://github.com/hyperium/hyper
[`tower`]: https://github.com/tower-rs/tower
[`tokio`]: https://github.com/tokio-rs/tokio
[`rustls`]: https://github.com/ctz/rustls
[`openssl`]: https://github.com/sfackler/rust-openssl
[`prost`]: https://github.com/danburkert/prost
[`tower-grpc`]: https://github.com/tower-rs/tower-grpc
[`0.1.0-alpha.1`]: https://crates.io/crates/tonic/0.1.0-alpha.1
[`async_trait`]: https://crates.io/crates/async-trait
[`dtolnay`]: https://github.com/dtolnay
[linkerd]: https://linkerd.io/
[`vector`]: https://github.com/timberio/vector
[`linkerd-proxy`]: https://github.com/linkerd/linkerd2-proxy
[`tonic-interop`]: https://github.com/hyperium/tonic/tree/master/tonic-interop
[`tonic-examples`]: https://github.com/hyperium/tonic/tree/master/tonic-examples
[`warp`]: https://github.com/seanmonstar/warp
[`tower-web`]: https://github.com/seanmonstar/warp
[`actix-net`]: https://github.com/actix/actix-net
[`noria`]: https://github.com/mit-pdos/noria
[`grpc-go`]: https://github.com/grpc/grpc-go
[documentation]: https://docs.rs/tonic/0.1.0-alpha.1/tonic/
[`tower`]: https://github.com/tower-rs/tower

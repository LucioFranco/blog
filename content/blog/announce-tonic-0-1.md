---
title: "Tonic: 0.1 has arrived!"
url: "/tonic-0-1-release"
date: 2020-01-14T10:00:00-04:00
draft: false
---

# Tonic: 0.1 has arrived!

[`tonic`] is a gRPC over HTTP/2 implementation focused on high performance, 
interoperability, and flexibility. 
It has been a few months since I originally released the `0.1.0-alpha.1` version. Since then there has been a ton of growth and improvements. We've seen 32 new [`contributors`], two new 
[`guides`], 8 more [`examples`] and 5  [`releases`]. Not only have we seen a lot of growth internally but we've also seen a large amount of adoption. 

[`contributors`]: https://github.com/hyperium/tonic/graphs/contributors
[`guides`]: https://github.com/hyperium/tonic/blob/master/examples
[`examples`]: https://github.com/hyperium/tonic/tree/master/examples/src
[`releases`]: https://github.com/hyperium/tonic/releases
[`tonic`]: https://github.com/hyperium/tonic

## Changes

Since the first `0.1.0-alpha.1` release, Tonic made several ergonomic changes.

### Upgrades EVERYWHERE

Tonic was originally released as an alpha because a majority of the crates it depended on were also alphas. `0.1` signifies that all our dependencies are as lean
as possible.
For instance: `syn` and `quote` are at 1.0,  `bytes`  is set to 0.5 and `hyper` is at 0.13
Thanks to all the maintainers for helping push out all these releases! Also, I would like to note that [`cargo-deny`] has been very helpful in ensuring that we have no duplicate
dependencies!

[`cargo-deny`]: https://github.com/EmbarkStudios/cargo-deny

### Goodbye openssl, hello rustls

The build-in transport module no longer supports `openssl`. Instead, Tonic defaults to `rustls`, which should simplify building Tonic-based applications and libraries. However, I recognize that Tonic users might want to use a different TLS library, so Tonic supports customization via [constructor client] and [constructor server].

[constructor server]: https://docs.rs/tonic/0.1.0/tonic/transport/server/struct.Router.html#method.serve_with_incoming 
[constructor client]: https://docs.rs/tonic/0.1.0/tonic/transport/struct.Endpoint.html#method.connect_with_connector 

### Interceptors

Tonic also supports gRPC interceptors (non-gRPC ecosystems might refer to “interceptors” as “middleware”). Like the name suggests, interceptors allow clients and servers to intercept a request and perform an arbitrary action, like adding headers to sign a request or logging a request. See the example below:

{{< highlight rust >}}
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let channel = Endpoint::from_static("http://[::1]:50051")
        .connect()
        .await?;
    let mut client = GreeterClient::with_interceptor(channel, intercept);
   // snip
}
/// This function will get called on each outbound request. Returning a
/// `Status` here will cancel the request and have that status returned to
/// the client.
fn intercept(req: Request<()>) -> Result<Request<()>, Status> {
    println!("Intercepting request: {:?}", req);
    Ok(req)
}
{{< /highlight >}}

One key thing to note here is that these interceptors are transport agnostic. They are pure gRPC, it does
not matter where you get the request from, it could be via `grpc-web` or `http2`.

More examples of this usage can be found [`here`].

[`here`]: https://github.com/hyperium/tonic/tree/master/examples/src/interceptor 

## v0.1.0

I'd like to give a special shoutout to all those that helped the project grow by opening issues, trying
Tonic out and opening PRs.   

That said, I am super happy to finally release the `0.1` release of `tonic`. This is the first stepping
stone in a great ecosystem built on top of [`tower`]. As always, there is a  [changelog] and
an [issue] tracker if you run into any issues. Please, feel welcome to also join us on [discord]
if you need any help! 

[`tower`]: https://github.com/tower-rs/tower
[changelog]: https://github.com/hyperium/tonic/blob/master/CHANGELOG.md
[issue]: https://github.com/hyperium/tonic/issues
[discord]: https://discord.gg/tokio

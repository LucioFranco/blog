---
title: "Tonic: 0.1 has arrived!"
url: "/tonic-0-1-release"
date: 2020-01-13T10:00:00-04:00
draft: false
---

# Tonic: 0.1 has arrived!

[`tonic`] is a gRPC over HTTP/2 implementation focused on high performance, 
interoperability, and flexibility. 

It has been a few months since I originally released the `0.1.0-alpha.1` version. Since, then 
there has been a ton of growth and improvements. We've seen 32 new [`contributors`], two new 
[`guides`], 8 more [`examples`] and 5  [`releases`]. Not only have we seen a lot growth internally 
but we've also seen a large amount of adoption. 

## Changes

Since, the first `0.1.0-alpha.1` release there have been a couple changes that have
improved the API and ease of use.

### Upgrades EVERYWHERE

The original alpha releases we released as alphas because a lot of the crates that `tonic`
depends on did not have proper releases. `0.1` signifies that all our dependencies are as lean
as possible. All proc macro crates are now at `1.0`, we've upgraded to `bytes 0.5` and `hyper 0.13`.
Thanks to all the maintainers for helping push out all these releases!

Also I would like to note that [`cargo-deny`] has been very helpful in ensuring that we have no duplicate
dependencies!

[`cargo-deny`]: https://github.com/EmbarkStudios/cargo-deny

### No more openssl

The built in transport module no longer supports `openssl` and instead has opted for 
a more pure rust solution by only supporting `rustls`. While this doesn't always satisfy
every usecase it should support most. Because of this change, we've also introduced ways 
of supplying your own IO type for both the [`server`] and [`client`] side.

[`server`]: http://linktodocs
[`client`]: http://linktodocs

### Interceptors

Another larger change has been the introduction of proper gRPC interceptors. There is now
a proper `Interceptors` type which wraps a closure. This strikes a balance between ease of use
and simplicity. This also means that interceptors are now no longer a transport concept but a
generic gRPC concept within `tonic`. `tonic-build` will now include a `with_interceptor` function
when creating your client or server side service that will accept a interceptor. As you can tell
from the examples they are quite easy to use since they are just a simple function/closure!


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

More examples of this usage can be found [`here`].

[`here]: http://linktoexamplesfolder

## v0.1.0

I am super happy to finally release the `0.1` release of `tonic`. This is the first stepping
stone in a great ecosystem built on top of [`tower`]. As always, there is a  [`changelog`] and
an [`issue`] tracker if you run into any issues. Please, feel welcome to also join us on [`discord`]
if you need any help! 

